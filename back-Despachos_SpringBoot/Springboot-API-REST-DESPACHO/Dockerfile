FROM maven:3.8.4-openjdk-17-slim AS build
WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app

RUN addgroup -S cittgroup && adduser -S cittuser -G cittgroup

COPY --chown=cittuser:cittgroup --from=build /app/target/*.jar app.jar

USER cittuser

ENV DB_PORT=3306 \
    DB_NAME=bd_despachos \
    PORT=8081

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]