# Use Eclipse Temurin JDK 21 image for compiling the application
FROM eclipse-temurin:21-jdk AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy Maven Wrapper files and Maven configuration
COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .

# Give execute permission to Maven Wrapper
RUN chmod +x mvnw

# Copy the application source code
COPY src ./src

# Build the Spring Boot application and create the JAR file
# -DskipTests skips running tests during the build process
RUN ./mvnw clean package -DskipTests


# Use a lightweight JRE image since only the compiled application needs to run
FROM eclipse-temurin:21-jre

# Set the working directory inside the runtime container
WORKDIR /app

# Copy the generated JAR file from the build stage
COPY --from=builder /app/target/*.jar app.jar

# Start the Spring Boot application when the container launches
ENTRYPOINT ["java", "-jar", "app.jar"]