# Demo1: Docker intro

Run the hello-world docker container from the defdev docker registry. Below shows the Dockerfile code

    FROM alpine

    RUN apk update --no-cache && apk add bash

    COPY entrypoint.sh /
    RUN ["chmod", "+x", "/entrypoint.sh"]

    CMD ["/entrypoint.sh"]

In this case the 'entrypoint.sh' contains nothing more than:

    #!/bin/bash

    echo "Hello world from Docker!"

Run this container from the defdev registry ("defdev/hello-world") in Jenkins by incorperating it either the "pipe-line-script" or the "pipe-line-script-parallel" files.
