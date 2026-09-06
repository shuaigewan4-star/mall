FROM eclipse-temurin:17-jre
WORKDIR /app
COPY mall-portal/target/mall-portal-1.0-SNAPSHOT.jar app.jar
EXPOSE 8085
ENTRYPOINT ["java", "-jar", "app.jar"]
