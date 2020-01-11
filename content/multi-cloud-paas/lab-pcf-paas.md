# Lab: Pivotal PaaS

Open a browser to [PWC](https://run.pivotal.io)

Create a new account to get a Cloud Foundry Organization and Space

Login into Aws Cloud9 environment we have used in previous labs from the Aws [console](https://console.aws.amazon.com/)

Install cf cli on Amazon Linux:

```
sudo wget -O /etc/yum.repos.d/cloudfoundry-cli.repo \
  https://packages.cloudfoundry.org/fedora/cloudfoundry-cli.repo && \
sudo yum install cf-cli -y
```

Verify installation: `cf --version`

Authenticate to PWS with the command: `cf login -a api.run.pivotal.io`

Provide email address and password used to create PWC account

We should have a sample [node.js end-point ](https://github.com/vkhazin/courseware-nodejs-container)application cloned in the Cloud9 environment

Change directory into the `api` folder: ` cd ~/environment/courseware-nodejs-container/api/`

Push the application to PWC: `cf push nodejs-end-point -c "node my-app.js"`

The deployment will fail, and you can use see the logs using cli: `cf logs nodejs-end-point --recent` or PWC web console

The reason for failure is start command is referencing a non-existent `my-app.js` file

Rerun the command `cf push nodejs-end-point -c "node server.js"` and wait for it to complete

Locate the deployed application end-point and launch it in a browser, don't forget that our end-point expects a parameter: `?name=John`

Navigate through the links to see application logs, routes, and uptime

To monitor the application PWC comes with its own monitoring, you can open from the application overview by selecting 

`View in PCF Metrics` 

Can also navigate directly to [https://metrics.run.pivotal.io/apps](https://metrics.run.pivotal.io/apps) and search by application name

You should find familiar metrics: latency and resource utilization

There is also an integration with [DataDog](https://docs.datadoghq.com/integrations/pivotal_platform/), it takes a bit more time than we have to complete the setup, maybe do at home

When satisfied with your experiments delete the application to avoid future charges: `cf delete -r nodejs-end-point`











