# BUILD TOOL - maven
FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

COPY auth-service/pom.xml .
# Resolve the same graph the build uses (honors exclusions), cached across builds
RUN --mount=type=cache,target=/root/.m2 mvn -B dependency:resolve

COPY auth-service/src ./src
COPY proto /proto

RUN --mount=type=cache,target=/root/.m2 mvn -B clean package -DskipTests

# RUNTIME - JRE
FROM eclipse-temurin:21-jre AS runner

WORKDIR /app

COPY --from=builder /app/target/auth-service.jar ./app.jar

EXPOSE 8085

ENTRYPOINT ["java", "-Xms256m", "-Xmx512m", "-jar", "app.jar"]