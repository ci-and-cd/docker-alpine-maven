# docker-alpine-maven

Maven for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-alpine-maven on Github](https://github.com/ci-and-cd/docker-alpine-maven)

[cirepo/maven on Docker Hub](https://hub.docker.com/r/cirepo/maven/)

## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.8

COPY --from=cirepo/glibc:2.29-r0-alpine-3.9-archive /data/root /
COPY --from=cirepo/java-11-openjdk:11.0.2-alpine-3.9-archive /data/root/usr/lib/jvm/java-11-openjdk /usr/lib/jvm/java-11-openjdk
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk

COPY --from=cirepo/maven:3.6.0-alpine-archive /data/root /
ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}

```
