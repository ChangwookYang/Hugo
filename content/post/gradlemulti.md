---
title: "Gradle 멀티 프로젝트"
date: 2021-01-20T00:10:00+09:00
---
### Gradle 멀티 프로젝트 관리

참고 : https://jojoldu.tistory.com/123



> 공통으로 사용되는 클래스를 여러 프로젝트에서 사용한다면 비효율적이다.
>
> Multi module 방식으로 사용하면 소스관리도 쉽고 편안!



##### 루트 프로젝트

setting.gradle

```java
// gradle-multi-modules 프로젝트가 
// 'module-common', 'module-api', 'module-web' 프로젝트를 하위 프로젝트로 관리
rootProject.name = 'gradle-multi-modules'
include 'module-common', 'module-api', 'module-web'
```




build.gradle

```java
buildscript {
    ext {
        springBootVersion = '1.5.1.RELEASE'
    }
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
        classpath "io.spring.gradle:dependency-management-plugin:0.6.0.RELEASE"
    }
}

subprojects {
    // subprojects는 settings.gradle에 include 된 프로젝트들을 전부 관리
    // 루트 프로젝트까지 포함하고싶으면 allprojects로 하면된다.
    group 'com.blogcode'
    version '1.0'

    // 하위 프로젝트들 모두 Spring boot와 의존성이 있끼에 관련된 PLUGIN을 등록
    apply plugin: 'java'
    apply plugin: 'spring-boot'
    apply plugin: 'io.spring.dependency-management'

    sourceCompatibility = 1.8

    repositories {
        mavenCentral()
    }

    dependencies {
        testCompile group: 'junit', name: 'junit', version: '4.12'
    }
}

// project의 경우 하위 프로젝트 간의 의존성을 관리한다.
// 참고로 :는 디렉토리 PATH를 의미한다.(루트프로젝트에서 하위프로젝트이므로)
// module-api와 module-web은 모두 module-common에 의존한다.
project(':module-api') {
    dependencies {
        compile project(':module-common')
    }
}

project(':module-web') {
    dependencies {
        compile project(':module-common')
    }
}
```





