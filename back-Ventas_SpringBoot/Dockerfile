FROM maven:3.8-eclipse-temurin-17 AS builder
#una
WORKDIR /app

COPY Springboot-API-REST/pom.xml .

RUN mvn dependency:go-offline -B

COPY Springboot-API-REST/src ./src

RUN mvn clean package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

RUN addgroup -S innovatech && adduser -S innovatech -G innovatech

COPY --chown=innovatech:innovatech --from=builder /app/target/*.jar app.jar

USER innovatech

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]