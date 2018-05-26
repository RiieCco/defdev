# Demo1: Docker intro 1.0

##### Note: Only look at the example files when absolutely stuck! (SPOILERS)

Run the hello-world docker container from the defdev docker registry. Below shows the Dockerfile code:

    FROM alpine

    RUN apk update --no-cache && apk add bash

    COPY entrypoint.sh /
    RUN ["chmod", "+x", "/entrypoint.sh"]

    CMD ["/entrypoint.sh"]

In this case the 'entrypoint.sh' contains nothing more than:

    #!/bin/bash

    echo "Hello world from Docker!"

Run this container from the defdev registry ("defdev/hello-world") in Jenkins by incorperating it either the "pipe-line-script" file and put it in the configuration for your job. These exercises are meant to also train your Google foo. So do not hessitate to go look it up online!

# Demo1: Docker intro 1.1

We repeat the same challenge but this time we swap the defdev/hello-world image for the image defdev/hello-world-env.
This image also includes running the container with an enviroment variable. With this enviroment variable we can run
the containers with dynamic parameters. The Dockerfile as shown in example 1.1 has not changed for this exersice. However, the entrypoint.sh was adapted to the following format:

    #!/bin/bash

    set -e

    exit_env_error() {
        echo "Error: env var '${1}' not set" >&2
        exit 1
    }

    [ -z "${ENV_VARIABLE}" ] && exit_env_error ENV_VARIABLE

    echo "Hello world from: ${ENV_VARIABLE}"

In this manner we can run the containers with different parameters and have generic containers that can be re-used against different repositories.
