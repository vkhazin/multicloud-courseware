# Lab: End-Point in a Container

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)

2. From the services select Cloud9 and open the environment we used before for terraform setup

3. Open a terminal and clone the repo with end-point source code:  
    `git clone https://github.com/vkhazin/courseware-nodejs-container.git`

4. Review the `*.js` files under `api` folder to get an idea of the source code

5. To build a docker image execute: `docker build ./ -t node/end-point`

6. Expected result: `...Successfully tagged node/end-point:latest`

7. To run the newly created image we need to create a container from it

8. Images are immutable, containers can mutate over their lifecycle

9. To launch a new container execute: `docker run --name node-end-point -d --rm -p 3001:3001 node/end-point`

10. --name gives our container a name

11. -p 3001:3001 maps host port 3001 to container port 3001

12. -d starts detached to be able to continue using the terminal

13. --rm delete the container when it is stopped

14. And now you are able to access the endpoint: `curl http://localhost:3001/?name=John`

15. This is how you create a node.js end-point running in a container

16. To delete the container: `docker rm node-end-point -f`

17. To delete the image: `docker rmi node/end-point`

18. There are a couple of yml files in the repository we will discuss in the next lab



