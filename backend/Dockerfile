FROM alpine:3.14

RUN apk add --no-cache openjdk11 maven

WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY ./src/ /app/

RUN mvn dependency:go-offline

RUN mvn package -DskipTests && \
    mv target/*.jar app.jar && \
    rm -rf ~/.m2 && \
    apk del maven && \
    rm -rf /var/cache/apk/*

RUN chown -R appuser:appgroup /app

USER appuser

CMD ["java", "-jar", "app.jar"]