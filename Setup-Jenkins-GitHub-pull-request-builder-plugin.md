  1. install jenkins with GitHub pull request builder plugin as `http://jenkins:9090/`
  2. install gitbucket as `http://gitbucket:8080/`
  3. Create repository on gitbucket as `http://gitbucket:8080/root/test`
  3. Create user on gitbucket for jenkins as `jenkinsbot`
  3. Add `jenkinsbot` to collaborator of repository `root/test`
  4. Add repository webhook `http://jenkins:9090/ghprbhook/`
  5. create personal access token on `http://gitbucket:8080/jenkinsbot/_application`
  6. set jenkins global setting of GitHub Pull Request Builder on `http://jenkins:9090/manage`
    * GitHub server api URL = `http://gitbucket:8080/api/v3`
    * Access Token = created on step 5.
    * Save.
  7. create job as `http://jenkins:9090/job/testjob/`
  8. on http://jenkins:9090/job/testjob/configure,
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

  9. hint
   * jenkins log has a lot information that show on `http://jenkins:9090/log/all` .
   * user that create access token has permission to write repository ?
   * when you replace access token, but jenkins not use soon. please restart jenkins.
