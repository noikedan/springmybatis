# springmybatis

##docker-compose 下で Java + Spring Boot + PostgreSQL (Mybatis)

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
