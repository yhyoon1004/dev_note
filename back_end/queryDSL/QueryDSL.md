# QueryDSL 사용법

목차

1. QueryDSL 라이브러리 추가 방법
2. 코드 작성법

---

## 1. QueryDSL 라이브러리 추가 방법

⚠️ SpringBoot 3.2.2 버전 이상 기준 ( 최신버전에서는 javax 에서 jakarta 로 변경되었음)

- build.gradle파일의 dependencies 에 아래의 의존성 추가

```groovy
dependencies {
    //Querydsl 필요 라이브러리
    implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
    annotationProcessor "com.querydsl:querydsl-apt:5.0.0:jakarta"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api"
    annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}
```

## 2. 코드 작성법