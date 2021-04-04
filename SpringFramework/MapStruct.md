## MapStruct
+ MapStruct & Lombok gradle 의존성 순서&버전 중요 ★
``` 
ext {
    mapstructVersion = "1.3.0.Final"
    lombokVersion = "1.18.6"
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation "org.mapstruct:mapstruct:${mapstructVersion}"
    annotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"
    annotationProcessor "org.projectlombok:lombok:${lombokVersion}"
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
