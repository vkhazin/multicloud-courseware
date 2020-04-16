# Author a Dockerfile

* Docker images are build using Dockerfile definitions
* E.g. hello-world image:

```text
FROM scratch
COPY hello /
CMD ["/hello"]
```

* Where `scratch` is `name:tag` of the base image, in this case, the most basic image `scratch:latest`
* COPY command copies the executable file `hello` to the root of a file system
* CMD is the command executed when a container is running

