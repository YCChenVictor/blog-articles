---
layout: post
title:
description: ''
date: '2022-09-12'
categories: API
note:
mathjax:
mermaid:
p5:
threeJS:
anchor:
publish: true
---

## Introduction

This article describes how to CRUD of Jenkins

## Why?

I want to use Jenkins to run tests

## How?

### install

start `jenkinsci/blueocean` with

```bash
docker run \
  --rm \
  -u root \
  -p 8080:8080 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v "$HOME":/home \
  jenkinsci/blueocean
```

then start `http://localhost:8080` on browser and then follow the steps to install it

### create user

Just follow the steps in localhost

* get token:

on dashboard -> Manage Jenkins -> Manage Users -> select user -> Add new token

### Create Job

I tried to find whether there is api for yaml or DSL but it seems only can create it with xml. To create job on UI, please follow [How to Create a New Build Job in Jenkins Freestyle Project](https://www.guru99.com/create-builds-jenkins-freestyle-project.html).

#### steps

1. create empty job on UI
2. retrieve `config.xml` from this job
3. post new job with that `config.xml`
4. update the build in the job with DSL api

#### implementation

(1) create empty job on UI

On `http://localhost:8080/`, press `new item`

(2) retrieve `config.xml` from this job

```javascript

```

There are two ways to create job:

* through xml
* through dsl

and test it on Postman

#### xml

```javascript
const request = require('request');
const configXml = '<project><builders/><publishers/><buildWrappers/></project>';

const options = {
    method: 'POST',
    url: 'http://<Jenkins_URL>/createItem?name=<job_name>',
    auth: {
        user: 'username',
        pass: 'api_token'
    },
    headers: {
        'Content-Type': 'application/xml'
    },
    body: configXml
};

request(options, (error, response, body) => {
    if (error) {
        console.error(error);
        return;
    }
    console.log(`Status code: ${response.statusCode}`);
    console.log(body);
});
```

#### dsl

```javascript
const request = require('request');
const jobDsl = {
    "freeStyleJob": {
        "name": "example-job",
        "scm": {
            "git": {
                "url": "https://github.com/example/example-repo.git"
            }
        },
        "triggers": {
            "timer": "H/15 * * * *"
        }
    }
};

const options = {
    method: 'POST',
    url: 'http://<Jenkins_URL>/job/<job_name>/job-dsl',
    auth: {
        user: 'username',
        pass: 'api_token'
    },
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(jobDsl)
};

request(options, (error, response, body) => {
    if (error) {
        console.error(error);
        return;
    }
    console.log(`Status code: ${response.statusCode}`);
    console.log(body);
});
```

## What?

TODO: try it in the future. The problem is I cannot create job through JSON with its API.

```javascript
const axios = require('axios');
const jenkinsCreateJob = require('../services/jenkinsCreateJob.js');

module.exports = (app) => {
  app.post('/addTask', async (req, res) => {
    // post a job with config.xml
    // update build with DSL
  })

  app.post('/createJob', async (req, res) => { // create (this should be only create job and should add another api to update DSL)
    const pipeline = ''

    forCrumb = await axios.get(`http://${process.env.JENKINS_USER}:${process.env.JENKINS_PASSWORD}@localhost:8080/crumbIssuer/api/json`)

    axios({
      method: 'post',
      url: `http://${process.env.JENKINS_USER}:${process.env.JENKINS_TOKEN}@localhost:8080/createItem?name=test1`,
      data:  JSON.stringify(pipeline),
      headers: {
        [forCrumb['data']['crumbRequestField']]: forCrumb['data']['crumb'],
        'Content-Type': 'application/json'
      }
    }).then((response) => {
        console.log('success')
        res.send(response)
        res.end()
      })
      .catch((response) => {
        console.log('error')
        res.send(response)
        res.end()
      });
  })

  app.post('/addBuildDSL', async (req, res) => {
    const pipeline = jenkinsCreateJob

    forCrumb = await axios.get(`http://${process.env.JENKINS_USER}:${process.env.JENKINS_PASSWORD}@localhost:8080/crumbIssuer/api/json`)

    axios({
      method: 'post',
      url: `http://${process.env.JENKINS_USER}:${process.env.JENKINS_TOKEN}@localhost:8080/job/test/job-dsl`,
      data:  JSON.stringify(pipeline),
      headers: {
        [forCrumb['data']['crumbRequestField']]: forCrumb['data']['crumb'],
        'Content-Type': 'application/json'
      }
    }).then((response) => {
        console.log('success')
        res.send(response)
        res.end()
      })
      .catch((response) => {
        console.log('error')
        res.send(response)
        res.end()
      });
  })

  app.get('/jobs', async (req, res) => { // read jobs
    const response = await axios.get(`http://${process.env.JENKINS_USER}:${process.env.JENKINS_PASSWORD}@localhost:8080/api/json`);
    const jobs = response.data.jobs;
    res.send(jobs)
    res.end()
  })

  app.get('/jobConfig', async (req, res) => {
    const config = axios.get(`http://${process.env.JENKINS_USER}:${process.env.JENKINS_PASSWORD}@localhost:8080/job/${req.query.job}/config.xml`)
    res.send(config)
    res.end()
  })

  app.get('/', () => { // read one job

  })
}
```

## Reference

[使用npm构建Node.js和React应用](https://www.jenkins.io/zh/doc/tutorials/build-a-node-js-and-react-app-with-npm/)

[how to trigger jenkins job via curl command remotely](https://serverfault.com/questions/888176/how-to-trigger-jenkins-job-via-curl-command-remotely)

[Jenkins](https://www.npmjs.com/package/jenkins#job-build)

[How to Create a New Build Job in Jenkins Freestyle Project](https://www.guru99.com/create-builds-jenkins-freestyle-project.html)

[Jenkins: 403 No valid crumb was included in the request](https://stackoverflow.com/questions/44711696/jenkins-403-no-valid-crumb-was-included-in-the-request)

[How to create a job using the REST API and cURL?](https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-masters/how-to-create-a-job-using-the-rest-api-and-curl)
