FROM openjdk:8

ENV SONAR_VERSION=6.7.5 \
    SONARQUBE_HOME=/opt/sonarqube \
    # Database configuration
    # Defaults to using H2
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

RUN groupadd -r sonarqube && useradd -r -g sonarqube sonarqube

# grab gosu for easy step-down from root
RUN set -x \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture)" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture).asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && (gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
        || gpg --keyserver ipv4.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4) \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -rf "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

RUN set -x \

    # pub   2048R/D26468DE 2015-05-25
    #       Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
    # uid                  sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
    # sub   2048R/06855C1D 2015-05-25
    && (gpg --keyserver ha.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE \
	    || gpg --keyserver ipv4.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE) \

    && cd /opt \
    && curl -o sonarqube.zip -fSL https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && curl -o sonarqube.zip.asc -fSL https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    && gpg --batch --verify sonarqube.zip.asc sonarqube.zip \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && chown -R sonarqube:sonarqube sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*
RUN set -x \
    && curl -o sonar-java-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-java-plugin/sonar-java-plugin-5.8.0.15699.jar \
    && curl -o sonar-js-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-javascript-plugin/sonar-javascript-plugin-5.0.0.6962.jar \
    && curl -o sonar-ts-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-typescript-plugin/sonar-typescript-plugin-1.8.0.3332.jar \
    && curl -o sonar-css-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-css-plugin/sonar-css-plugin-1.0.2.611.jar \
    && curl -o sonar-groovy-plugin.jar -fSL https://github.com/musketyr/sonar-groovy/releases/download/1.6-RC5/sonar-groovy-plugin-1.6-RC5.jar \
    && curl -o sonar-yaml-plugin.jar -fSL https://github.com/sbaudoin/sonar-yaml/releases/download/v1.1.0/sonar-yaml-plugin-1.1.0.jar \
    && curl -o sonar-github-plugin.jar -fSL http://central.maven.org/maven2/org/sonarsource/github/sonar-github-plugin/1.4.2.1027/sonar-github-plugin-1.4.2.1027.jar \
    && curl -o sonar-auth-github-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-auth-github-plugin/sonar-auth-github-plugin-1.3.jar \
    && curl -o sonar-checkstyle-plugin.jar -fSL https://github.com/checkstyle/sonar-checkstyle/releases/download/4.11/checkstyle-sonar-plugin-4.11.jar \
    && curl -o sonar-findbugs-plugin.jar -fSL https://github.com/spotbugs/sonar-findbugs/releases/download/3.8.0/sonar-findbugs-plugin-3.8.0.jar \
    && curl -o sonar-jdepend-plugin.jar -fSL https://github.com/willemsrb/sonar-jdepend-plugin/releases/download/sonar-jdepend-plugin-1.1.1/sonar-jdepend-plugin-1.1.1.jar \
    && curl -o sonar-jacoco-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-jacoco-plugin/sonar-jacoco-plugin-1.0.1.143.jar \
    && curl -o sonar-scm-git-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-scm-git-plugin/sonar-scm-git-plugin-1.4.1.1128.jar \
    && curl -o sonar-html-plugin.jar -fSL https://binaries.sonarsource.com/Distribution/sonar-html-plugin/sonar-html-plugin-3.0.0.1411.jar \
    && mv *.jar sonarqube/extensions/plugins/

VOLUME "$SONARQUBE_HOME/data"

WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
ENTRYPOINT ["./bin/run.sh"]
