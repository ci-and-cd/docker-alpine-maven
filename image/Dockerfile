
FROM cirepo/nix:2.2.1-alpine-3.9


USER root


ARG IMAGE_ARG_FILESERVER
ARG IMAGE_ARG_MAVEN_VERSION


COPY --from=cirepo/glibc:2.29-r0-alpine-3.9-archive /data/root /
COPY --from=cirepo/java-11-openjdk:11.0.2-alpine-3.9-archive /data/root/usr/lib/jvm/java-11-openjdk /usr/lib/jvm/java-11-openjdk
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk


COPY --chown=root:root docker /data


RUN set -ex \
  && MAVEN_ARCH=apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.6.0}-bin.tar.gz \
  && if [[ ! -f /data/${MAVEN_ARCH} ]]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o ${MAVEN_ARCH} \
       ${IMAGE_ARG_FILESERVER:-http://www-us.apache.org/dist}/maven/maven-3/${IMAGE_ARG_MAVEN_VERSION:-3.6.0}/binaries/${MAVEN_ARCH}; \
     fi \
  && mkdir -p /opt \
  && tar xf /data/${MAVEN_ARCH} -C /opt \
  && ln -sf /opt/apache-maven-${IMAGE_ARG_MAVEN_VERSION:-3.6.0} /opt/maven \
  && rm -f /data/${MAVEN_ARCH} \
#  ${IMAGE_ARG_FILESERVER:-http://dl.bintray.com}/jcgay/maven/fr/jcgay/maven/maven-profiler/2.6/maven-profiler-2.6-shaded.jar
  && if [[ ! -f /data/maven-profiler-2.6-shaded.jar ]]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o maven-profiler-2.6-shaded.jar \
       http://dl.bintray.com/jcgay/maven/fr/jcgay/maven/maven-profiler/2.6/maven-profiler-2.6-shaded.jar; \
     fi \
  && mv /data/maven-profiler-2.6-shaded.jar /opt/maven/lib/ext/


USER alpine


ENV M2_HOME /opt/maven
ENV PATH ${JAVA_HOME}/bin:${M2_HOME}/bin:${PATH}
