# Demo2: Dependency check

Run the dependency-check docker container from the defdev docker registry ("defdev/dependency-check-lite"). Below is shown the code from the entrypoint.sh and Dockerfile used to build this container. Look into the code to figure out how this docker container should be deployed in the pipe-line-script file!

### Dockerfile

    FROM java:8

    MAINTAINER Riccardo ten Cate <riccardo.ten.cate@owasp.org>

    ENV VERSION_URL="https://jeremylong.github.io/DependencyCheck/current.txt"
    ENV DOWNLOAD_BASEURL="https://dl.bintray.com/jeremy-long/owasp"

    COPY entrypoint.sh /

    RUN mkdir -p /opt/dependency-check \
     && wget -O /tmp/dependency-check-latest.zip "${DOWNLOAD_BASEURL}/dependency-check-$(wget -O - -o /dev/null  "${VERSION_URL}")-release.zip" \
     && unzip /tmp/dependency-check-latest.zip -d /opt \
     && rm /tmp/dependency-check-latest.zip \
     && chmod +x /entrypoint.sh

    ENTRYPOINT ["/entrypoint.sh"]


### Entrypoint.sh

    #!/bin/bash

    set -e

    exit_env_error() {
        echo "Error: env var '${1}' not set" >&2
        exit 1
    }

    PROJECT_FOLDER="${PROJECT_FOLDER:-/project}"
    OUTPUT_FOLDER="${OUTPUT_FOLDER:-/project}"
    OUTPUT_FORMAT="${OUTPUT_FORMAT:-XML}"

    [ -z "${SOURCE_REPO}" ] && exit_env_error SOURCE_REPO


    rm -rf "${PROJECT_FOLDER}" "${OUTPUT_FOLDER}"
    git clone "${SOURCE_REPO}" "${PROJECT_FOLDER}"

    mkdir -p "${OUTPUT_FOLDER}"

    /opt/dependency-check/bin/dependency-check.sh \
        --project "${PROJECT_FOLDER}" \
        --format "${OUTPUT_FORMAT}" \
        --out "${OUTPUT_FOLDER}" \
        --enableExperimental \
        --scan "${PROJECT_FOLDER}/**"

    cat "${OUTPUT_FOLDER}/dependency-check-report.xml"


