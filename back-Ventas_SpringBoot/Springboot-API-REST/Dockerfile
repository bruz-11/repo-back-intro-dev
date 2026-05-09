# STAGE 1: Build Maven
FROM maven:3.9-eclipse-temurin-17-alpine AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# STAGE 2: Run Java
FROM eclipse-temurin:17-jre-alpine AS production
WORKDIR /app
# Usuario random sin privilegios pa que no te hackeen desde Lo Espejo
RUN addgroup -S duocgroup && adduser -S duocuser -G duocgroup
COPY --from=build /app/target/*.jar ./app.jar
USER duocuser
EXPOSE 8080 
CMD ["java", "-jar", "app.jar"]