
FROM cirepo/nix:2.0.4-alpine-3.7


USER root


ARG IMAGE_ARG_MAVEN_VERSION


COPY --from=cirepo/glibc:2.23-r3-alpine-3.7-archive /data/root /
COPY --from=cirepo/java-oracle:8u171-alpine-3.7-archive /data/root/usr/lib/jvm/java-8-oracle /usr/lib/jvm/java-8-oracle
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle


COPY --chown=root:root docker /data


RUN set -ex \
  && if [ ! -f /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3}-bin.tar.gz ]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3}-bin.tar.gz \
       ${IMAGE_ARG_FILESERVER:-http://mirrors.tuna.tsinghua.edu.cn}/apache/maven/maven-3/${IMAGE_ARG_MAVEN_VERSION:-3.5.3}/binaries/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3}-bin.tar.gz; \
     fi \
  && mkdir -p /opt \
  && tar xf /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3}-bin.tar.gz -C /opt \
  && ln -sf /opt/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3} /opt/maven \
  && rm -f /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.3}-bin.tar.gz


USER alpine


ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}
