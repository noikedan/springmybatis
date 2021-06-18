# springmybatis

## docker-compose 下で Java + Spring Boot + PostgreSQL (Mybatis)

./mvnw clean package -DskipTests  
docker build ./ -t springbootapp

CREATE TABLE IF NOT EXISTS staff (
  id SERIAL,
  name VARCHAR(60),
  email VARCHAR(254) NOT NULL,
  status BOOLEAN NOT NULL,
  registration DATE NOT NULL
);

INSERT INTO staff(name, email, status, registration)
VALUES('中池', 'nakaike@example.com', 't', CURRENT_DATE);
INSERT INTO staff(name, email, status, registration)
VALUES('田中', 'tanaka@example.com', 't', CURRENT_DATE);

docker rm --force postgres
docker rm --force /db

docker run -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=admin -d --volumes-from PostgresData postgres

docker exec -it postgres bash

psql -h localhost -p 5432 -U postgres -d postgres

Docker用の場合はapplication.ymlを下記に書き換えが必要


src/main/resources/application.properties 
@@ -0,0 +1,8 @@
mybatis.configuration.map-underscore-to-camel-case=true
#spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.url=jdbc:postgresql://postgres:5432/testdb
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.username= postgres
spring.datasource.password=admin
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
