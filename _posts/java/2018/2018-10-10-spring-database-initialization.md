---
layout: post
title: Spring에서 JPA / Hibernate 초기화 전략
categories:
  - Java
tags:
  - SpringMVC
  - Spring Boot
  - JPA
  - Hibernate
last_modified_at: 2018-10-10T22:10+09:00
comments: true
---

Spring-data-JPA와 DBMS를 연결해서 사용할 때 간편히 개발환경의 변경사항을 적용하여 테스트 할 수 있다.

특히 테스트를 위한 in-memory Database인 H2 Database를 염두에 둔 DB 초기화 전략에서 신경쓸 점을 간략히 정리해보았다.

## DDL generation

Spring은 EntityScan을 통해 `@Entity` 애노테이션이 명시한 클래스를 찾는다.

`spring.jpa.generate-ddl=true` 옵션을 true로 설정하면 해당 데이터를 근거로 서버 시작 시점에 DDL문을 생성하여 DB에 적용한다.

jpa.generate 설정은 JPA 구현체 DDL생성 옵션의 링크이고, true / false 밖에 선택할 수 없다.

대부분 구현체로 Hibernate를 사용하기 때문에 `spring.jpa.hibernate.ddl-auto` 옵션을 통해서 보다 상세한 데이터베이스 초기화 전략을 설정할 수 있다.

다음 목록중 하나를 `spring.jpa.hibernate.ddl-auto` 옵션의 값으로 지정할 수 있다.

- `none`: 아무것도 실행하지 않는다 (대부분의 DB에서 기본값이다)
- `create-drop`: SessionFactory가 시작될 때 drop및 생성을 실행하고, SessionFactory가 종료될 때 drop을 실행한다 (in-memory DB의 경우 기본값이다)
- `create`: SessionFactory가 시작될 때 데이터베이스 drop을 실행하고 생성된 DDL을 실행한다
- `update`: 변경된 스키마를 적용한다
- `validate`: 변경된 스키마가 있다면 변경점을 출력하고 애플리케이션을 종료한다

## SQL script

Spring 기본값으로 classpath 루트에 `schema.sql` 파일이 있다면 서버 시작시 스크립트를 실행한다.
보통 `schema.sql`은 DDL 스크립트를 명시해두고, 데이터를 위한 DML문은 `data.sql` 파일로 작성해두면 역시 자동으로 실행한다.

**Hibernate에도 기본 실행 스크립트가 있다는 점을 조심해야 한다.**

Hibernate는 classpath 루트의 `import.sql` 파일이 있다면 서버 시작시 해당 스크립트를 자동 실행한다.

만약 Spring이 인식하는 `schema.sql` 파일과 `data.sql` 파일이 있거나, `spring.datasource.data` 옵션을 통해 추가로 적용할 스크립트 파일을 지정했음에도 불구하고, 동일 내용으로 `import.sql` 파일을 작성한다면 중복 입력으로 오류가 발생할 수 있다.(테이블 생성, PK 컬럼의 데이터 ...)

> 추가로, Hibernate가 인식하는 `import.sql` 파일의 경우, 각 명령을 한 줄로 작성해야 문법오류가 발생하지 않는다

또한, JPA는 `schema-${platform}.sql`과 `data-${platform}.sql` 파일이 있다면 실행시켜 데이터베이스 플랫폼에 맞춘 스크립트 실행이 가능하다.
사용할 플랫폼 정의는 `spring.datasource.platform`값을 따른다

### SQL script 사용하지 않기

Spring은 `spring.datasource.initialization-mode`값에 따라 embedded DataSource의 schema를 생성한다. (기본값: `always`, 생성안함: `never`)

Spring 버전에 따라 설정에 조금 차이가 있다

```properties
spring.datasource.initialization-mode=never # Property for Spring boot 2.0
spring.datasource.initialize=false # Property for Spring boot 1.0
```

### SQL script 인코딩

`schema.sql`에는 알파벳 이외의 문자를 사용할 일이 없으나, `data.sql`에서는 한글을 사용할 가능성이 있다.
개발시 서버를 기동하는 상황에서는 문제가 없을 수 있지만, 빌드 후 스크립트가 실행되는 상황에서는 한글이 깨져서 입력될 수 있다.

이때, 초기화되는 데이터 인코딩 설정을 위해서 다음 옵션을 적용하면 된다.

`spring.datasource.sql-script-encoding=UTF-8`

## 오류가 발생해도 서버 시작하기

Spring JDBC initializer 동작시 fail-fast이므로, 스크립트에서 문제가 발생하면 어플리케이션 동작이 실패한다.

`spring.datasource.continue-on-error` 설정값(기본값: `false`)을 `true`로 변경하여 종료되지 않게 할 수 있다.
