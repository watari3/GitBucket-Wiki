This page provides information on how to setup [Jenkins GitHub Branch Source plugin](https://wiki.jenkins.io/display/JENKINS/GitHub+Branch+Source+Plugin) with GitBucket to use the **GitHub Organization** template job.

> tested with https://github.com/gitbucket/gitbucket/releases/tag/4.15.0

## Prerequisites

  - Install Jenkins as `http://jenkins:9090/`
    - Install the NodeJS plugin
  - Install GitBucket as `http://gitbucket:8080/`

### Create a repository on GitBucket and clone it

> I will create a NodeJS project, to run simple build and tests

- Create a group on GitBuket as eg `http://gitbucket:8080/wey-yu/hello` *(my group is named "wey-yu")*
- Create a `hello` repository on GitBucket (in the `wey-yu` group) as `http://gitbucket:8080/wey-yu/hello` (and initialize this repository with a README file)
- Now locally clone the repository (`git clone http://gitbucket:8080/git/wey-yu/hello.git`)
- Create this repository structure:

  ```
  .
  ├── README.md
  ├── index.js
  ├── Jenkinsfile
  ├── package.json
  ├── .gitignore
  └── tests
    └── test.js
  ```
### Source code(s)

#### `package.json`

```json
{
  "name": "hello",
  "description": "hello",
  "main": "index.js",
  "scripts": {
    "test": "./node_modules/.bin/mocha tests/**",
    "start": "node index.js"  
  },
  "devDependencies": {
    "chai": "^4.1.1",
    "mocha": "^3.5.0"
  }
}
```

#### `test.js`

```javascript
'use strict';

const expect = require('chai').expect;

describe('# 42 is 42', () => {
  it('should equal 42', () => {
    expect(42).to.equal(42);
  });
});
```

#### `.gitignore`

```
node_modules/*
```

#### `Jenkinsfile`

> the `Jenkinsfile` tells to Jenkins what to do at each push of commit

```groovy
node {
  stage('Checkout') {
      git url: env.GITBUCKET_URL + "/wey-yu/hello.git", branch: env.BRANCH_NAME
  }
 
  stage('Build') {
      def nodeHome = tool name: 'nodejs', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
      env.PATH = "${nodeHome}/bin:${env.PATH}"
      sh "npm install"
      sh "npm test"  
  }
}
```

> you can write what you want inside `index.js`

### Push the repository to GitBucket

```shell
git add .
git commit -m "🚀 first version of hello"
git push origin master
```

## Setup of the GitBucket repository

- Create a user on GitBucket for Jenkins as `indythebot` *(in fact whatever you want)*
  - Add `indythebot` as a collaborator of repository `wey-yu/hello`
- Add repository webhook `http://jenkins:9090/github-webhook/` *(⚠️ don't forget the `/` at the end of the url)*
  - Choose `application/json` for **Content type**
  - Check **Pull request**
  - Check **Push**
- Create personal access token for `indythebot` on `http://gitbucket:8080/indythebot/_application`

## Setup of Jenkins

### Configure system

- Go to `http://jenkins:9090/configure`
- At the **Global properties** section:
  - Check `Environment variables`
  - Add this variable: `GITBUCKET_URL` with this value: `http://gitbucket:8080/git` *(⚠️ don't forget the `/git` at the end of the url)*, this variable is used by the `Jenkins` file
- At the **GitHub Enterprise Servers** section: *(add a record if the section is empty)*
  - Type the API endpoint: `http://gitbucket:8080/api/v3`
  - Give a name to the endpoint: eg `gitbucket`
- Save

### Global Tool Configuration

- Go to `http://jenkins:9090/configureTools`
- At the **NodeJS** section, add a "NodeJS item": 
  - Type the name, eg `nodejs` *(⚠️ this value is very important, it's used in the `Jenkinsfile`)*
  - Keep the default settings
    - `Install automatically` is checked
    - **Install from nodejs.org**: choose the last version of NodeJS (or an other version if you need it)
- Save

## Create and setup a new Jenkins job

### Initialize the job

- Go to `http://jenkins:9090/view/all/newJob`
  - Type the name of the new job, use the name ou your `wey-yu` group (aka orgnization)
  - Select **GitHub Organization**
  - Click on the **OK** Button

### Setup the job

- At the **Projects/GitHub Organization** section:
  - Set the `API endpoint` with the `gitbucket` endpoint
  - Create a "Username with password" credential with `indythebot` as user and its `password` (so, the web token is probably useless) and then select this new credential
  - Type the name of the **Owner**: it's the name of the group (`wey-yu`)
  - Keep only the **Discover branches** behavior and select the `all branches` strategy
  - Check the **Project Recognizers** section: you should have a **Pipeline Jenkinsfile** record with `Jenkinsfile` value for the `Script Path` field
- At **the Scan Organization Triggers**
  - Uncheck the `Periodically if not otherwise run` checkbox
- Save

## Return to the `hello` repository

- Go to the settings of the `hello` repository (`http://gitbucket:8080/wey-yu/hello/settings/options`)
- Select the `Branches` tab
  - At the **Protected branches** section
    - Choose `master` branch
    - Check **Protect this branch**
    - Check **Require status checks to pass before merging**
      - Check `continuous-integration/jenkins/branch` 
    - Check **Include administrators**
    - Click on **Save changes**

## Test the settings

### New Branch

- create a new branch (`wip-display-hello`) from `master` in GitBucket
- update the `index.js` file
- commit the update
- you will see a new branch in the Jenkins dashboard (`http://jenkins:8080/job/wey-yu/job/hello/`)

### New Pull Request

- create a pull request from the `wip-display-hello` branch
- now, in the pull request **Conversation** tab, you can see that the pull request has been checked by Jenkins

### Write a bad test

- on the same branch (`wip-display-hello`) change the content of `/tests/test.js` to write a "bad" test:
  ```javascript
  'use strict';

  const expect = require('chai').expect;

  describe('# 42 is 42', () => {
    it('should equal 42', () => {
      expect(42).to.equal(42);
    });
  });
  ```
- commit your changes
- return to the pull request
- you can see that you cannot merge the pull request

That's all 😉