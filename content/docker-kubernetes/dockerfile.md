# Author a Dockerfile

* Docker images are build using Dockerfile definitions
* E.g. hello-world image:

```
FROM scratch
COPY hello /
CMD ["/hello"]
```

* Where `scratch` is `name:tag` of base image, in this case, most basic image `scratch:latest`
* 'COPY' command copies executable file 'hello' to the root of file system
* 'CMD' is the command executed when container is running



