# docker-alpine-maven

Maven for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-alpine-maven on Github](https://github.com/ci-and-cd/docker-alpine-maven)

[cirepo/alpine-maven on Docker Hub](https://hub.docker.com/r/cirepo/alpine-maven/)

## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.7

COPY --from=cirepo/alpine-glibc:3.7_2.23-r3-archive /data/root /
COPY --from=cirepo/java-8-oracle:8u171-archive /data/root/usr/lib/jvm/java-8-oracle /usr/lib/jvm/java-8-oracle
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

COPY --from=cirepo/alpine-maven:3.5.3-archive /data/root /
ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}

```
