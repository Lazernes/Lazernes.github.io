---
title: "Gemini Chat Bot"
excerpt: "Gemini를 이용한 chatbot을 만들어 음식에 대한 설명을 완성해주는 기능 구현"

wirter: Myeongwoo Yoon
categories:
  - 백엔드 아키텍처 심화 과정
tags:
  - Spring
  - Java

toc: true
toc_sticky: true
 
date: 2026-03-03
last_modified_at: 2026-03-03
---

&ensp;먼저 `https://aistudio.google.com/` 에서 API키를 발급하고 Intellij에서 환경변수 처리를 해준다.
```yml
# Google Ai
google:
  ai:
    api-key: ${GOOGLE_API_KEY}
```

&ensp;build.gradle에서 **Google GenAI** dependency를 추가한다.
```
// Gemini
implementation 'org.springframework.ai:spring-ai-starter-model-google-genai'
```

&ensp;다음과 같이 GeminiConfig를 Bean 등록을 해준다.
```java
@Configuration
public class GeminiConfig {

    @Bean
    public Client geminiClient() {
        return new Client();
    }
}
```

&ensp;이제 Gemini는 배달앱의 음식 설명을 생성해야 하므로 다음과 같이 generateProductDescription과 prompt를 작성한다.
```java
@Slf4j
@Service
@RequiredArgsConstructor
public class AiDescriptionServiceImpl implements AiDescriptionService {

    private final Client geminiClient;

    @Override
    public String generateProductDescription(String productName, String point) {

        String prompt = """
                당신은 배달앱 음식 메뉴 소개를 작성하는 마케터입니다.
                
                [메뉴 이름]
                %s
                
                [메뉴 특징]
                %s
                
                위 정보를 기반으로 배달앱에 등록할 음식 설명을 작성하세요.
                
                조건:
                - 답변을 최대한 간결하게 50자 이하로
                - 먹고 싶어지게 작성
                - 과장 광고 금지
                - 이모지 사용 금지
                """.formatted(productName, point);

        GenerateContentResponse response = geminiClient.models.generateContent(
                "gemini-3-flash-preview",
                prompt,
                null
        );

        String result = response.text();

        if(result == null || result.isBlank()) {
            throw new RuntimeException("AI 설명 생성 실패");
        }

        return result.trim();
    }
}
```

&ensp;구현한 generateDescription은 ProductService에서 사용한다.
```java
public ProductResponseDto generateDescription(UUID productId, String point) {

    Product product = productRespository.findByProductIdAndDeletedAtIsNull(productId)
            .orElseThrow(() -> new IllegalArgumentException("상품이 존재하지 않습니다."));

    String aiDescription = aiDescriptionService.generateProductDescription(product.getName(), point);

    product.updateDescription(aiDescription);

    return ProductResponseDto.from(product);
    }
```

```java
@PostMapping("/{productId}/ai-description")
public ResponseEntity<ProductResponseDto> generateAiDescription(@PathVariable UUID productId, @RequestBody AiDescriptionRequestDto request) {
    return ResponseEntity.ok(
            productService.generateDescription(productId, request.getPoint())
    );
}
```

&ensp;이제 Postman에서 테스트를 진행해본다.<br/>
<p align="center"><img src="/assets/img/백엔드 아키텍처 심화 과정/Gemini chatbot/0-1-제품 생성.png" width="500"></p>

<p align="center"><img src="/assets/img/백엔드 아키텍처 심화 과정/Gemini chatbot/0-2-제품 설명 AI 자동생성.png" width="500"></p>

&ensp;정상적으로 설명이 생성된 것을 확인할 수 있다.