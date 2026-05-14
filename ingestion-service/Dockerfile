# Stage 1: Build the application
FROM maven:3.9.9-eclipse-temurin-21-alpine AS builder

WORKDIR /app

# Copy Maven wrapper files for dependency caching
COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .

# Give execute permission
RUN chmod +x mvnw

# Download dependencies first (better Docker cache)
RUN ./mvnw dependency:go-offline

# Copy source code
COPY src ./src

# Build jar
RUN ./mvnw clean package -DskipTests


# ==============================
# Stage 2: Lightweight runtime
# ==============================
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copy built jar
COPY --from=builder /app/target/*.jar app.jar

# JVM optimization for containers
ENTRYPOINT ["java", \
"-XX:+UseContainerSupport", \
"-XX:MaxRAMPercentage=75.0", \
"-jar", \
"app.jar"]