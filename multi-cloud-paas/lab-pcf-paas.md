# Lab: Pcf PaaS

1. Open a browser to [PWS Pivotal Web Services ](https://run.pivotal.io)
2. Create a new account to get a Cloud Foundry Organization and Space
3. Create an organization, unfortunately, it will ask for a phone number to verify
4. Without creating an organization, deployment will fail
5. Login into Aws Cloud9 environment we have used in the previous labs from the Aws [console](https://console.aws.amazon.com/)
6. Install cf `cli` on Amazon Linux:
7. ```text
   sudo wget -O /etc/yum.repos.d/cloudfoundry-cli.repo \
     https://packages.cloudfoundry.org/fedora/cloudfoundry-cli.repo && \
   sudo yum install cf-cli -y
   ```
8. Verify the installation: `cf --version`
9. Authenticate to PWS with the command: `cf login -a api.run.pivotal.io`
10. Provide email address and password used to create PWS account
11. We should have a sample [node.js end-point ](https://github.com/vkhazin/courseware-nodejs-container)application cloned in the Cloud9 environment already
12. Change directory into the `api` folder: `cd ~/environment/courseware-nodejs-container/api/`
13. Push the application to PWS: `cf push {your-name}-nodejs-end-point -c "node my-app.js"`
14. The deployment will fail, and you can use see the logs using `cli`: `cf logs nodejs-end-point --recent` or PWS web console
15. The reason for failure is the start command referencing a non-existent `my-app.js` file
16. Re-run the command `cf push {your-name}-nodejs-end-point -c "node server.js"` and wait for it to complete
17. Locate the deployed application end-point using PWS web console and launch it in a browser, don't forget that our end-point expects a parameter: `?name=John`
18. Navigate through the PWS console links to see application logs, routes, and uptime monitoring
19. To monitor the application PWS comes with its own monitoring, you can open from the application overview by selecting: `View in PCF Metrics`
20. Can also navigate directly to [https://metrics.run.pivotal.io/apps](https://metrics.run.pivotal.io/apps) and search by application name
21. You should find familiar metrics: latency and resource utilization
22. There is also an integration with [DataDog](https://docs.datadoghq.com/integrations/pivotal_platform/), it takes a bit more time than we have to complete the setup, maybe, do at home
23. Pivotal offers back-end services through Marketplace e.g. when you need a database or an in-memory cache
24. When satisfied with your experiments delete the application to avoid future charges: `cf delete -r nodejs-end-point`
