---
title: "Firebase example"
excerpt: "."

wirter: Myeongwoo Yoon
categories:
  - AIML
tags:
  - Spring
  - Java

toc: true
toc_sticky: true
use_math: true 

date: 2024-09-01
last_modified_at: 2023-09-01
---

초기 설정
======
&ensp;먼저 build.gradle에서 다음과 같은 dependency를 추가한다.
```java
dependencies {
    implementation group: 'com.google.firebase', name: 'firebase-admin', version: '9.3.0'
}
```

&ensp;이후 resources 폴더에 firebase/serviceAccountKey.json을 생성하고, firebase의 키 내용을 입력해준다.<br/>

init()
------
&ensp;먼저 firebase에 대한 초기화 코드를 작성한다.
```java
package com.example.firebase_test.firebase;

import com.google.auth.oauth2.GoogleCredentials;
import com.google.firebase.FirebaseApp;
import com.google.firebase.FirebaseOptions;
import jakarta.annotation.PostConstruct;
import org.springframework.context.annotation.Configuration;

import java.io.FileInputStream;
import java.io.IOException;

@Configuration
public class firebaseConfig {

    @PostConstruct
    public void firestore() throws IOException {
        FileInputStream serviceAccount =
                new FileInputStream("src/main/resources/firebase/serviceAccountKey.json");

        FirebaseOptions options = new FirebaseOptions.Builder()
                .setCredentials(GoogleCredentials.fromStream(serviceAccount))
                .build();

        FirebaseApp.initializeApp(options);

    }

}
```

&ensp;다음으로 Phone 객체를 생성해준다.
```java
package com.example.firebase_test.firebase;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@NoArgsConstructor
@Getter
@Setter
public class Phone {
    private String company;
    private String phoneName;
    private String telecom;

    public Phone(String company, String phoneName, String telecom) {
        this.company = company;
        this.phoneName = phoneName;
        this.telecom = telecom;
    }
}
```

Create
======

&ensp;이제 서비스를 생성해본다. 다음은 데이터를 저장하고 저장된 시간을 String으로 보내주는 코드이다.
```java
package com.example.firebase_test.firebase;

import com.google.api.core.ApiFuture;
import com.google.cloud.firestore.Firestore;
import com.google.cloud.firestore.WriteResult;
import com.google.firebase.cloud.FirestoreClient;
import org.springframework.stereotype.Service;

import java.util.concurrent.ExecutionException;

@Service
public class PhoneService {
    public static final String COL_NAME = "phone";

    public String savePhoneInfo(Phone phone) throws ExecutionException, InterruptedException {
        Firestore dbFireStore = FirestoreClient.getFirestore();
        ApiFuture<WriteResult> collectionApiFuture = dbFireStore.collection(COL_NAME).document(phone.getPhoneName()).set(phone);

        return collectionApiFuture.get().getUpdateTime().toString();
    }
}
```

&ensp;다음은 위 코드를 테스트하기 위한 코드이다.
```java
package com.example.firebase_test.firebase;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.concurrent.ExecutionException;

import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest
class phoneServiceTest {

    @Autowired
    private PhoneService phoneService;

    @Test
    public void saveAndGetPhoneInfo() throws ExecutionException, InterruptedException {
        Phone phone = new Phone("1231231", "samsung", "sk");
        String result = phoneService.savePhoneInfo(phone);
        System.out.println(result);
    }
}
```

&ensp;위 코드를 실행하면 다음과 같이 잘 실행됐음을 알 수 있다.<br/>
<p align="center"><img src="/assets/img/AIML/firebase example/2-1-Create-example.png"></p>

Read
======

&ensp;이제 Database의 정보를 조회해 본다. 다음과 같이 phoneService.java에 코드를 추가한다.
```java
public Phone getPhoneInfo(String phoneNM) throws ExecutionException, InterruptedException {
    Firestore dbFireStore = FirestoreClient.getFirestore();
    DocumentReference documentReference = dbFireStore.collection(COL_NAME).document(phoneNM);

    ApiFuture<DocumentSnapshot> future = documentReference.get();
    DocumentSnapshot document = future.get();

    Phone phone = null;
    if (!document.exists()) {
        return null;
    }

    phone = document.toObject(Phone.class);
    return phone;
}
```

&ensp;이제 테스트를 진행해본다.
```java
@Test
public void getPhoneInfo() throws ExecutionException, InterruptedException {
    Phone result = phoneService.getPhoneInfo("samsung");
    System.out.println(result.getPhoneName());
    System.out.println(result.getCompany());
    System.out.println(result.getTelecom());
}
```

&ensp;위 테스트를 진행하면 테스트가 잘 진행됨을 확인할 수 있다.

Update
======

&ensp;이제 Database의 정보를 갱신해본다.
```java
public String updatePhoneInfo(Phone phone) throws ExecutionException, InterruptedException {
    Firestore dbFireStore = FirestoreClient.getFirestore();
    ApiFuture<WriteResult> collectionApiFuture = dbFireStore.collection(COL_NAME).document(phone.getPhoneName()).set(phone);

    return collectionApiFuture.get().getUpdateTime().toString();
}
```

&ensp;테스트 코드와 결과는 다음과 같다.
```java
@Test
public void updatePhoneInfo()throws ExecutionException, InterruptedException {
    Phone phone = new Phone("123123", "samsung", "kt");
    String result = phoneService.updatePhoneInfo(phone);
    System.out.println(result);
}
```

<p align="center"><img src="/assets/img/AIML/firebase example/4-1-Update-example.png"></p>

&ensp;PhoneName이 samsung의 데이터가 수정됨을 확인할 수 있다.

Delete
======

&ensp;마지막으로 Database의 정보를 삭제해본다.
```java
// Delete
public String deletePhoneInfo(String phoneName) {
    Firestore dbFireStore = FirestoreClient.getFirestore();
    ApiFuture<WriteResult> writeResult = dbFireStore.collection(COL_NAME).document(phoneName).delete();
    return "삭제완료";
}
```

```java
@Test
public void deletePhoneInfo() {
    String result = phoneService.deletePhoneInfo("samsung");
    System.out.println(result);
}
```