FROM maven:3.8-eclipse-temurin-17 AS build
WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn clean package -DskipTests

# ETAPA 2: Producción con JRE de Temurin y Alpine
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Comandos correctos de Alpine para crear usuario
RUN addgroup -S cittgroup && adduser -S cittuser -G cittgroup

COPY --chown=cittuser:cittgroup --from=build /app/target/*.jar app.jar

USER cittuser

ENV DB_PORT=3306 \
    DB_NAME=bd_despachos \
    PORT=8081

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]