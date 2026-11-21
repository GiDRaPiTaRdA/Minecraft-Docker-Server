ARG JAVA_VERSION=25

FROM eclipse-temurin:${JAVA_VERSION}-jdk

RUN apt-get update && \
    apt-get install -y --no-install-recommends git curl && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /build

ENTRYPOINT ["java", "-jar", "BuildTools.jar"]