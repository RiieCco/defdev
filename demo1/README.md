# Demo1: Docker intro

Run the hello-world docker container from the defdev docker registry. Below is shown the Dockerfile code

    FROM alpine

    RUN apk update --no-cache && apk add bash

    COPY entrypoint.sh /
    RUN ["chmod", "+x", "/entrypoint.sh"]

    CMD ["/entrypoint.sh"]

