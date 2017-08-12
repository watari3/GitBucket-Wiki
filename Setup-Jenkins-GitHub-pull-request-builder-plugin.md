This page provides information on how to setup [Jenkins GitHub pull request builder plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin) with GitBucket to automatically build pull-requests with Jenkins.

  1. Install Jenkins with GitHub pull request builder plugin as `http://jenkins:9090/`
  2. Install GitBucket as `http://gitbucket:8080/`
  3. Create repository on GitBucket as `http://gitbucket:8080/root/test`
  4. Create user on GitBucket for Jenkins as `jenkinsbot`
      * Add `jenkinsbot` to collaborator of repository `root/test`
  5. Add repository webhook `http://jenkins:9090/ghprbhook/`
  6. Create personal access token on `http://gitbucket:8080/jenkinsbot/_application`
  7. Set on Jenkins global setting `http://jenkins:9090/manage`
      * 'GitHub Plugin Configuration'
        * Credentials -> Add
          * Type = 'Secret text'
          * Secret = created on step 6.
          * Add.
        * GitHub API URL = `http://gitbucket:8080/api/v3`
      * `GitHub Pull Request Builder`
        * GitHub Server API URL = `http://gitbucket:8080/api/v3`
        * Credentials = select credentials that you created some time ago.
      * **old version ghprb**
        * GitHub server api URL = `http://gitbucket:8080/api/v3`
        * Access Token = created on step 6.
        * Save.
  8. Create job as `http://jenkins:9090/job/testjob/`
  9. On http://jenkins:9090/job/testjob/configure,
    * GitHub project = `http://gitbucket:8080/root/test/`
    * Source Code Management
      * git
        * Repositories
          * Repository URL = `http://gitbucket:8080/git/root/test.git`
          * Refspec = `+refs/pull/*:refs/remotes/origin/pr/*`
        * Branches to build
          * Branch Specifier (blank for 'any') = `${sha1}`
    * Build trigger
      * GitHub Pull Request Builder
        * Admin list = `root`
    * Save.

  10. Hint
   * Jenkins log has a lot information that show on `http://jenkins:9090/log/all` .
   * User that create access token has permission to write repository ?
   * When you replace access token, but Jenkins not use soon. please restart Jenkins.