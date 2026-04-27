FROM maven:3.9-eclipse-temurin-21-alpine AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:resolve
COPY src ./src
RUN mvn -DskipTests -B clean package

FROM eclipse-temurin:21-alpine AS jre-builder
RUN $JAVA_HOME/bin/jlink \
    --add-modules java.base,java.logging,java.net.http,java.sql,jdk.httpserver,java.naming \
    --strip-debug \
    --compress=2 \
    --output /jre-minimal

FROM alpine:3.18
RUN apk add --no-cache libc6-compat zlib
COPY --from=jre-builder /jre-minimal /opt/jre
RUN addgroup -S app && adduser -S app -G app
WORKDIR /app
USER app
COPY --from=builder /app/target/words.jar .
COPY --from=builder /app/target/dependency ./dependency
EXPOSE 8080
CMD ["/opt/jre/bin/java", "-Xmx8m", "-Xms8m", "-cp", "words.jar:dependency/*", "Main"]

