FROM eclipse-temurin:21-jdk-jammy AS builder
WORKDIR /app
COPY pom.xml mvnw mvnw.cmd ./
COPY .mvn/ /app/.mvn
RUN sed -i 's/\r$//' mvnw && chmod +x mvnw && ./mvnw dependency:go-offline -B

COPY . .

RUN sed -i 's/\r$//' mvnw && chmod +x mvnw && ./mvnw package -DskipTests -B

FROM gcr.io/distroless/java21-debian12:nonroot

WORKDIR /app
COPY --from=builder --chown=nonroot:nonroot /app/target/*.jar ./app.jar
EXPOSE 8080
CMD ["/app/app.jar" ]