# docker login

FROM maven:3.9.5-eclipse-temurin-17 AS builder

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]


# docker build -t mahov/app_java_spring_maho:latest .
# docker images
# docker run -d -p 8080:8080 --name app_java_spring_maho-container mahov/app_java_spring_maho:latest
# docker ps
# On modifie notre application.properties et on ajoute ça (pour que le serveur écoute le bon port et la bonne adresse) :
    # server.port=8080
    # server.address=0.0.0.0
# docker stop app_java_spring_maho-container
# docker rm app_java_spring_maho-container
# docker tag mahov/app_java_spring_maho:latest mahov/app_java_spring_maho:v1.0
# docker push mahov/app_java_spring_maho:v1.0
# https://hub.docker.com/r/mahov/app_java_spring_maho