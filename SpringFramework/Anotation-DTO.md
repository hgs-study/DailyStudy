## Anotation-DTO
+ @Valid
``` 
[SpringLegacy-maven]
<dependency> 
	<groupId>javax.validation</groupId> 
	<artifactId>validation-api</artifactId> 
	<version>1.0.0.GA</version> 
</dependency> 

<dependency> 
	<groupId>org.hibernate</groupId> 
	<artifactId>hibernate-validator</artifactId> 
	<version>4.2.0.Final</version> 
</dependency>

[SpringBoot-gradle]
    implementation 'org.springframework.boot:spring-boot-starter-validation'
```

+ @Valid
  - DTO에 들어오는 유효성 검사 확인 가능
  - @NotNull(boolean, int) -null값 허용 안한다
  - @NotEmpty - 공백 허용 안한다.
  - @NotBlank(String)  - null값과 공백 허용 안한다.
