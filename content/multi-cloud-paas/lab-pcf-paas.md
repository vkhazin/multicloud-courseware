# Lab: Pivotal PaaS

1. Open a browser to [PWS Pivotal Web Services ](https://run.pivotal.io)
2. Create a new account to get a Cloud Foundry Organization and Space
3. Login into Aws Cloud9 environment we have used in the previous labs from the Aws [console](https://console.aws.amazon.com/)
4. Install cf `cli` on Amazon Linux:
5. ```
   sudo wget -O /etc/yum.repos.d/cloudfoundry-cli.repo \
     https://packages.cloudfoundry.org/fedora/cloudfoundry-cli.repo && \
   sudo yum install cf-cli -y
   ```
6. Verify the installation: `cf --version`
7. Authenticate to PWS with the command: `cf login -a api.run.pivotal.io`
8. Provide email address and password used to create PWS account
9. We should have a sample [node.js end-point ](https://github.com/vkhazin/courseware-nodejs-container)application cloned in the Cloud9 environment already
10. Change directory into the `api` folder: `cd ~/environment/courseware-nodejs-container/api/`
11. Push the application to PWS: `cf push nodejs-end-point -c "node my-app.js"`
12. The deployment will fail, and you can use see the logs using `cli`: `cf logs nodejs-end-point --recent` or PWS web console
13. The reason for failure is the start command referencing a non-existent `my-app.js` file
14. Re-run the command `cf push nodejs-end-point -c "node server.js"` and wait for it to complete
15. Locate the deployed application end-point using PWS web console and launch it in a browser, don't forget that our end-point expects a parameter: `?name=John`
16. Navigate through the PWS console links to see application logs, routes, and uptime monitoring
17. To monitor the application PWS comes with its own monitoring, you can open from the application overview by selecting: `View in PCF Metrics`
18. Can also navigate directly to [https://metrics.run.pivotal.io/apps](https://metrics.run.pivotal.io/apps) and search by application name
19. You should find familiar metrics: latency and resource utilization
20. There is also an integration with [DataDog](https://docs.datadoghq.com/integrations/pivotal_platform/), it takes a bit more time than we have to complete the setup, maybe, do at home
21. Pivotal offers back-end services through Marketplace e.g. when you need a database or an in-memory cache
22. When satisfied with your experiments delete the application to avoid future charges: `cf delete -r nodejs-end-point`



