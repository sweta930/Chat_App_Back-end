# Build stage: compile with Java 21
FROM eclipse-temurin:21-jdk AS build
WORKDIR /app

# copy wrapper & pom first to benefit from Docker layer cache
COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .

# then copy sources
COPY src ./src

# build
RUN chmod +x mvnw && ./mvnw -DskipTests clean package

# Run stage: lightweight JRE 21
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/target/*.jar /app/app.jar

ENV JAVA_OPTS=""
EXPOSE 8080
CMD ["sh", "-c", "java $JAVA_OPTS -Dserver.port=${PORT:-8080} -jar /app/app.jar"]
