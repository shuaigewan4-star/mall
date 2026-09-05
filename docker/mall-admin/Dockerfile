FROM  eclipse-temurin:17-jre
WORKDIR /app
COPY mall/mall-admin/target/mall-admin-1.0-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT  ["java", "-jar", "app.jar"]
