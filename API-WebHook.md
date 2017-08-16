The GitBucket api/webhook is designed for compatibility with [GitHub's implementation](https://developer.github.com/v3/).

The API endpoints are reachable under: `http(s)://yourgitbucket/api/v3`

GitBucket does not fully implement the GitHub API/webhook and respects only a subset of parameters. If you find some behavior different from GitHub, it can be a GitBucket bug or a feature not yet implemented :).

## Authentication

 * [x] [personal OAUTH token in header](https://developer.github.com/v3/#oauth2-token-sent-in-a-header)
   * The tokens can be created via the UI, by going into the "Account Settings" menu and selecting the "Applications" tab.
 * [x] [Basic Authentication](https://developer.github.com/v3/#basic-authentication) (since v4.3)
 * [ ] [<del>OAuth2 Token (sent as a parameter)</del>](https://developer.github.com/v3/#oauth2-token-sent-as-a-parameter) not supported yet.
 * [ ] [<del>OAuth2 Key/Secret</del>](https://developer.github.com/v3/#oauth2-keysecret) not supported yet.


## API

 * Root Endpoint
   * [List endpoints](https://developer.github.com/v3/#root-endpoint) (since v4.3, Only rate-limit endpoint explained)
 * Issues
   * [List issues for a repository](https://developer.github.com/v3/issues/#list-issues-for-a-repository) (since v4.9)
   * [Get a single issue](https://developer.github.com/v3/issues/#get-a-single-issue) (since v4.8)
   * [Create an issue](https://developer.github.com/v3/issues/#create-an-issue) (since v4.9)
   * Comments
     * [List comments on an issue](https://developer.github.com/v3/issues/comments/#list-comments-on-an-issue)
     * [Create A Comment](https://developer.github.com/v3/issues/comments/#create-a-comment)
   * Labels
     * [List all labels for this repository](https://developer.github.com/v3/issues/labels/#list-all-labels-for-this-repository) (since v3.11)
     * [Get a single label](https://developer.github.com/v3/issues/labels/#get-a-single-label) (since v3.11)
     * [Create a label](https://developer.github.com/v3/issues/labels/#create-a-label) (since v3.11)
     * [Update a label](https://developer.github.com/v3/issues/labels/#update-a-label) (since v3.11)
     * [Delete a label](https://developer.github.com/v3/issues/labels/#delete-a-label) (since v3.11)
 * Pull Requests
   * [List pull requests](https://developer.github.com/v3/pulls/#list-pull-requests)
   * [Get a single pull request](https://developer.github.com/v3/pulls/#get-a-single-pull-request)
   * [List commits on a pull request](https://developer.github.com/v3/pulls/#list-commits-on-a-pull-request)
 * Repositories
   * [Get](https://developer.github.com/v3/repos/#get)
   * [Create](https://developer.github.com/v3/repos/#create)
   * [Enabling and disabling branch protection](https://developer.github.com/v3/repos/#enabling-and-disabling-branch-protection) (since v3.11)
   * Status
     * [Create a Status](https://developer.github.com/v3/repos/statuses/#create-a-status)
     * [List Statuses for a specific Ref](https://developer.github.com/v3/repos/statuses/#list-statuses-for-a-specific-ref)
     * [Get the combined Status for a specific Ref](https://developer.github.com/v3/repos/statuses/#get-the-combined-status-for-a-specific-ref)
   * Branches
     * [List all branches for this repository](https://developer.github.com/v3/repos/branches/#list-branches) (since v4.3)
   * Contents
     * [List all contents of a file or directory in this repository](https://developer.github.com/v3/repos/contents/#get-contents) (since v4.3)
   * Reference
     * [Get a reference](https://developer.github.com/v3/git/refs/#get-a-reference) (since v4.3)
   * Collaborators
     * [List collaborators](https://developer.github.com/v3/repos/collaborators/#list-collaborators) (since v4.3)
 * Users
   * [Get a single user](https://developer.github.com/v3/users/#get-a-single-user)
   * [Get the authenticated user](https://developer.github.com/v3/users/#get-the-authenticated-user)
   * [List repositories for this user](https://developer.github.com/v3/repos/#list-user-repositories) (since v4.3)
 * Groups
   * [Get a single group](https://developer.github.com/v3/orgs/#get-an-organization) (since v4.3)
   * [List repositories for this group](https://developer.github.com/v3/repos/#list-organization-repositories) (since v4.3)


### Example

With a default local installation, having created a token for the `root` user, calling

`curl -H "Authorization: token 6b690aa9e528c54835619b2cb717f61035e9a013" http://localhost:8080/api/v3/user`

answers

`{"login":"root","email":"root@localhost","type":"User","site_admin":true     ,"created_at":"2016-04-01T13:27:12Z","url":"http://localhost:8080/api/v3/users/root","html_url":"http://localhost:8080/root"}`

## WebHook Events

 * [IssuesEvent](https://developer.github.com/v3/activity/events/types/#issuesevent)
 * [IssueCommentEvent](https://developer.github.com/v3/activity/events/types/#issuecommentevent)
 * [PullRequestReviewCommentEvent](https://developer.github.com/v3/activity/events/types/#pullrequestreviewcommentevent)
 * [PullRequestEvent](https://developer.github.com/v3/activity/events/types/#pullrequestevent)
 * [PushEvent](https://developer.github.com/v3/activity/events/types/#pushevent)
 * [GollumEvent](https://developer.github.com/v3/activity/events/types/#gollumevent)
