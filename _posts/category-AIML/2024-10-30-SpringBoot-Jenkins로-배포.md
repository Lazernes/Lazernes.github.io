---
title: "SpringBoot를 Jenkins의 Pipeline을 통해 배포"
excerpt: "Github의 SpringBoot Repository를 Jenkins의 Pipeline을 활용해 배포한다."

wirter: Myeongwoo Yoon
categories:
  - AIML
tags:
  - CICD
  - Spring
  - Jenkins

toc: true
toc_sticky: true
use_math: true 

date: 2024-10-30
last_modified_at: 2023-10-30
---

&ensp;우선 AWS ubuntu에 t2.medium 인스턴스를 생성해준다.

Jenkins 설치
======

&ensp;먼저 ubuntu 환경에 Jenkins를 설치한다. 소프트웨어 공학 시간에 배운 방법 대신 Docker를 이용해 Jenkins를 설치해본다.
```
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

&ensp;위 과정을 진행하면 EC2 기계에 Docker가 설치된다. 이후 다음 명령을 입력하면 `http:<public-IP-Address>:8080`을 통해 Jenkins에 접속이 가능하다.
```
docker run -d -p 8080:8080 -p 50000:50000 --name jenkins \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:jdk17
```

* Jenkins 컨테이너를 Docker 소켓을 공유하게 설정하여, Jenkins가 호스트에서 Docker 명령을 사용할 수 있도록 한다.

&ensp;이후 Docker 그룹에 Jenkins 사용자를 추가해야한다.
```
docker exec -u root -it jenkins /bin/bash
usermod -aG docker jenkins
// Jenkins 사용자를 Docker 그룹에 추가하면 재시작을 해야한다.
docker restart jenkins
```

&ensp;이후 Jenkins 컨테이너에서 `docker ps` 명령을 하여 docker 명령을 사용할 수 있는지 확인한다. 만약 실행되지 않는다면 다음 명령을 통해 Jenkins 사용자가 Docker 그룹에 추가되었는지 확인할 수 있다.
```
docker exec -u root -it jenkins /bin/bash
cat /etc/group | grep docker
// docker:x:103:
cat /etc/passwd | grep jenkins
// jenkins:x:1000:1000::/var/jenkins_home:/bin/bash
usermod -aG docker jenkins

// 소켓의 권한 설정
chown jenkins:docker /var/run/docker.sock

// 이후 docker restart jenkins
```

&ensp;위 명령어로 `docker:x:103:`과 같은 출력이 나오면, Jenkins  사용자가 Docker 그룹에 포함되어 있다.


&ensp;Jenkins에 접속한 뒤, `Pipeline, Git Plugin, Docker Pipeline, Docker` 플러그인을 설치한다.<br/>

&ensp;컨테이너에 docker를 설치했는지는 기억이 잘 안난다.
```
docker exec -it jenkins_server /bin/bash
apt update
apt install -y docker.io
```

Pipeline 작성
======

&ensp;Pipeline을 생성한다. 지금은 파이프라인의 이름을 `AIMLPipeline`으로 하였다.

Google firebase serviceAccountKey.json
------

&ensp;credentials에 들어가서 Secret text를 Kind로 설정하고, Secret에 serviceAccountKey.jso 내용을 그대로 복사한다. ID에는 SERVICE_ACCOUNT_KEY를 입력한다.

Dockerfile 작성
------

&ensp;Dockerfile을 `/var/jenkins_home/workspace/AIMLPipeline/` 경로에 vim을 설치 후 작성한다.

```
# Base image
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy the build artifacts from the target directory
COPY build/libs/*.jar app.jar

# Firebase serviceAccountKey.json 파일 복사
COPY src/main/resources/firebase/serviceAccountKey.json /app/src/main/resources/firebase/serviceAccountKey.json

# Expose the port the app runs on
EXPOSE 8081

# Command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

* Base image: From 명령어는 Docker 이미지의 기본 이미지를 설정한다. 여기서는 OpenJDK 17을 사용한다.
* Working directory: WORKDIR 명령어는 컨테이너 내에서 작업할 디렉토리를 설정
* Copy artifacts: COPY 명령어는 Gradle 빌드에서 생성된 JAR 파일을 컨테이너로 복사한다. 이 경로는 Gradle 빌드 결과물에 따라 다를 수 있으므로, 빌드 후 JAR 파일이 위치한 경로를 확인해야 한다.
* Expose port: EXPOSE 명령어는 애플리케이션이 사용하는 포트를 외부에 노출한다.
* Entry point: ENTRYPOINT 명령어는 컨테이너가 시작될 때 실행할 명령을 정의한다. 여기서는 JAR 파일을 실행하는 Java 명령어이다.

pipeline script
------
&ensp; 다음과 같이 pipeline script를 작성한다.
```
pipeline {
    agent any
    
    environment {
        // 필요 시 환경 변수를 설정합니다.
        DOCKER_IMAGE_NAME = "my-image-name"
        SERVICE_ACCOUNT_KEY = credentials('SERVICE_ACCOUNT_KEY')  // Jenkins에 저장한 비밀 텍스트 ID
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Git 저장소에서 코드 가져오기
                git branch: 'master', url: 'https://github.com/2024-AIML/aiml_server_2024'
            }
        }
        
        stage('Build') {
            steps {
                script {
                     // serviceAccountKey.json 파일 생성
                    sh 'mkdir -p src/main/resources/firebase'
                    sh 'echo "$SERVICE_ACCOUNT_KEY" > src/main/resources/firebase/serviceAccountKey.json'

                    // 파일 생성 확인
                    sh 'ls -al src/main/resources/firebase/'
                    
                    // 파일 내용 확인 (보안이 필요 없는 경우만 출력)
                    sh 'cat src/main/resources/firebase/serviceAccountKey.json'
                }

                // gradlew에 실행권한 부여
                sh 'chmod +x ./gradlew'
                // 빌드 실행
                sh './gradlew build'
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script {
                    // Docker 빌드 명령
                    sh 'docker build -t my-image-name:latest .'
                    sh 'docker images'  // Docker 이미지 리스트를 출력하여 확인

                    // Docker 이미지를 푸시하려면 Docker Hub 또는 다른 레지스트리에 로그인하고 설정해야 합니다.
                    // docker.build('my-image-name:latest').push()
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // 기존 컨테이너 중지 및 제거
                sh 'docker stop my-image-name || true'
                sh 'docker rm my-image-name || true'
                    
                // 새로운 컨테이너 실행
                sh "docker run -d -p 8081:8081 --name ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_NAME}:latest"
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
```

&ensp;이제 `http:<public IP Address>:8081/`로 API를 사용할 수 있다.