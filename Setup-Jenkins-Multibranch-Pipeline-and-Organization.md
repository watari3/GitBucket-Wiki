This page shows how to setup Jenkins Multibranch Pipeline and GitBucket.

## Prerequisite

- GitBucket is running.
- Jenkins is running.
- Jenkins can access to GitBucket without port number and context path, such as `http://gitbucket.example.com`.
- Jenkins Multibranch Pipeline is installed.

## Creating a Multibranch Pipeline job

Configure Jenkins:
1. Open **Manage Jenkins** - **Configure System**.
1. Add a server in **GitHub Enterprise Servers** section. API endpoint should be like `http://gitbucket.example.com/api/v3`.

Add a job on Jenkins:
1. Open **New Item**.
1. Enter job name and mark as a **Multibranch Pipeline**. Then, click **OK**.
1. Add a GitHub source in **Branch Sources** section.
    * **API endpoint**: select `GitBucket (http://gitbucket.example.com/api/v3)`.
    * **Credentials**: required if anonymous access is denied on GitBucket.
    * **Owner**: user or group
    * **Repository**: select your repository
1. Click **Save**.

Configure a webhook on GitBucket:
1. Open your repository - **Settings** - **Service Hooks**.
1. Click **Add webhook** button.
1. Fill followings and save.
    * **Payload URL**: `http://jenkins.example.com/github-webhook/`
    * **Which events would you like to trigger this webhook?**: Push, Pull Request

Create a `Jenkinsfile` on your repository:
```groovy
pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'echo Building ${BRANCH_NAME}...'
      }
    }
  }
}
```

A new build should be started immediately on Jenkins.

## Creating an Organization folder

Add a job on Jenkins:
1. Open **New Item**.
1. Enter job name and mark as a **GitHub Organization**. Then, click **OK**.
1. Add a GitHub source in **Branch Sources** section.
    * **API endpoint**: select `GitBucket (http://gitbucket.example.com/api/v3)`.
    * **Credentials**: required if anonymous access is denied on GitBucket.
    * **Owner**: group
1. Click **Save**.

Configure a webhook on GitBucket:
1. Open your group - **Edit group** - **Service Hooks**.
1. Click **Add webhook** button.
1. Fill followings and save.
    * **Payload URL**: `http://jenkins.example.com/github-webhook/`
    * **Which events would you like to trigger this webhook?**: Push, Pull Request

Then, your group and repositories should be shown in Jenkins.

## For better security

Configure GitBucket:
1. Open **System settings** and set **Anonymous access** to **Deny**.
1. Create `jenkins` user and add it to your group.

Configure Jenkins:
1. Open **Credentials** - *Add Credentials*.
1. Fill followings and save.
    * **Username**: `jenkins`
    * **Password**: password of above
    * **Description**: `GitBucket access credential`

Then, fix jobs to use credentials.

## See also

- [Branches and Pull Requests - jenkins.io](https://jenkins.io/doc/book/pipeline/multibranch/)
