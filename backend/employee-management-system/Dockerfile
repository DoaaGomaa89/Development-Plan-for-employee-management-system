# syntax=docker/dockerfile:1

# --- Build stage ---
FROM maven:3.9.8-eclipse-temurin-17 AS build
WORKDIR /workspace
COPY pom.xml .
# Pre-fetch deps for faster rebuilds
RUN mvn -q -DskipTests dependency:go-offline
COPY src ./src
# Build the jar
RUN mvn -q -DskipTests package

# --- Runtime stage ---
FROM eclipse-temurin:17-jre
WORKDIR /app
# Copy whatever jar was produced
COPY --from=build /workspace/target/*.jar app.jar
ENV JAVA_OPTS=""
EXPOSE 8080
ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar /app/app.jar"]
