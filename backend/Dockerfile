# Stage 1: Build the app using Maven
FROM maven:3.9.4-eclipse-temurin-11 AS build

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies first (for caching)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the Spring Boot app (skip tests for faster builds)
RUN mvn clean package -DskipTests

# Stage 2: Run the app with minimal JDK
FROM eclipse-temurin:11-jre-alpine

WORKDIR /app

# Copy the JAR from the build stage
COPY --from=build /app/target/student-service-docker.jar app.jar

# Expose port 8080 (default Spring Boot port)
EXPOSE 8080

# Run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
