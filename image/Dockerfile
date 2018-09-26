
FROM cirepo/nix:2.1.1-alpine-3.8


USER root


ARG IMAGE_ARG_MAVEN_VERSION


COPY --from=cirepo/glibc:2.23-r3-alpine-3.8-archive /data/root /
COPY --from=cirepo/java-oracle:8u181-alpine-3.8-archive /data/root/usr/lib/jvm/java-8-oracle /usr/lib/jvm/java-8-oracle
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle


COPY --chown=root:root docker /data


RUN set -ex \
  && if [ ! -f /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4}-bin.tar.gz ]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4}-bin.tar.gz \
       ${IMAGE_ARG_FILESERVER:-http://mirrors.tuna.tsinghua.edu.cn}/apache/maven/maven-3/${IMAGE_ARG_MAVEN_VERSION:-3.5.4}/binaries/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4}-bin.tar.gz; \
     fi \
  && mkdir -p /opt \
  && tar xf /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4}-bin.tar.gz -C /opt \
  && ln -sf /opt/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4} /opt/maven \
  && rm -f /data/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.5.4}-bin.tar.gz \
  && if [ ! -f /data/maven-profiler-2.6-shaded.jar ]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o maven-profiler-2.6-shaded.jar \
       ${IMAGE_ARG_FILESERVER:-http://dl.bintray.com}/jcgay/maven/fr/jcgay/maven/maven-profiler/2.6/maven-profiler-2.6-shaded.jar; \
     fi \
  && mv /data/maven-profiler-2.6-shaded.jar /opt/maven/lib/ext/



USER alpine


ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}
