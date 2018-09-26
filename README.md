# docker-alpine-maven

Maven for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-alpine-maven on Github](https://github.com/ci-and-cd/docker-alpine-maven)

[cirepo/maven on Docker Hub](https://hub.docker.com/r/cirepo/maven/)

## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.8

COPY --from=cirepo/glibc:2.23-r3-alpine-3.8-archive /data/root /
COPY --from=cirepo/java-oracle:8u181-alpine-3.8-archive /data/root/usr/lib/jvm/java-8-oracle /usr/lib/jvm/java-8-oracle
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

COPY --from=cirepo/maven:3.5.4-alpine-archive /data/root /
ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}

```
