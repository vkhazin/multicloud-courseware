# Lab: End-Point in a Container

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. We will use Cloud9 as a development environment
3. And later we will deploy the Docker container to a Kubernetes cluster running on GCP
4. From the services select Cloud9 and open the environment we used before for terraform setup
5. Open a terminal and clone the repo with end-point source code:  
    `git clone https://github.com/vkhazin/courseware-nodejs-container.git`

6. Review the `*.js` files under `api` folder to get an idea of the source code

7. To build a docker image execute: `docker build ./ -t node/end-point`
8. Expected result: `...Successfully tagged node/end-point:latest`
9. To run the newly created image we need to create a container from it
10. Images are immutable, containers can mutate over their lifecycle
11. To launch a new container execute: `docker run --name node-end-point -d --rm -p 3001:3001 node/end-point`
12. `--name` gives our container a name
13. `-p 3001:3001` maps host port 3001 to container port 3001
14. `-d` starts detached to be able to continue using the terminal
15. `--rm` deletes the container when it is stopped
16. And now you are able to access the endpoint: `curl http://localhost:3001/?name=John`
17. This is how you create a node.js end-point running in a container
18. To delete the container: `docker rm node-end-point -f`
19. To delete the image: `docker rmi node/end-point`
20. There are a couple of yml files in the repository we will discuss in the next lab



