This page lists valid URL formats for GitBucket. Items in bold are fields to be filled in. 

The information used to create this document can mostly be found [here](https://github.com/gitbucket/gitbucket/tree/master/src/main/scala/gitbucket/core/controller).

## Main
| URL Format | Description | Comments |
| --- | --- | --- |
| / | Shows the news feed for the current user | |
| /signin | Shows the login page | |
| /signout | Logs out the current user, and goes to the homepage | |
| /activities.atom | Global news feed | |
| /search | The search page | |

## Users
| URL Format | Description | Comments |
| --- | --- | --- |
| /**username** | Show user profile | |
| /**username**.atom | User feed | |
| /**userName**/_avatar | User avatar image | |
| /register | Show user registration form if enabled, else redirect to homepage | |

## Groups
| URL Format | Description | Comments |
| --- | --- | --- |
| /groups/new | New group | Only if user is logged in |
| /**groupName**/_editgroup | Show group edit page | Only if user is logged in and is a manager of the group |
| /**groupName**/_deletegroup | Show group edit page | Only if user is logged in and is a manager of the group | 

## Dashboard
| URL Format | Description | Comments |
| --- | --- | --- |
| /dashboard/issues | Shows issues created by the current user | Only if user is logged in |
| /dashboard/issues/assigned | Shows issues assigned to the current user | Only if user is logged in |
| /dashboard/issues/created_by | Shows issues created by the current user | Only if user is logged in |
| /dashboard/issues/mentioned | Shows issues which the current user was mentioned in | Only if user is logged in |
| /dashboard/pulls | Shows pull requests created by the current user | Only if user is logged in |
| /dashboard/pulls/created_by | Shows pull requests created by the current user | Only if user is logged in |
| /dashboard/pulls/assigned | Shows pull requests assigned to the current user | Only if user is logged in |
| /dashboard/pulls/mentioned | Shows pull requests which the current user was mentioned in | Only if user is logged in |

## Repositories
| URL Format | Description | Comments |
| --- | --- | --- |
| /new | Create a new repository | Only if user is logged in |
| /**owner**/**repository** | Shows the file list of a repository | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/tree/**branch**/ | Shows the file list of a branch of a repository | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/commits | Shows the list of commits of a repository | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/fork | Fork a repository | Only if user is logged in and has read access to the repository and forking is enabled |
| /**owner**/**repository**/new/**branch**/**path** | Creates a file | Only if user is logged in and has write access to the repository |
| /**owner**/**repository**/edit/**branch**/**path** | Edits a file | Only if user is logged in and has write access to the repository |
| /**owner**/**repository**/remove/**branch**/**path** | Deletes a file | Only if user is logged in and has write access to the repository |
| /**owner**/**repository**/blob/**branch**/**path** | Shows a file | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/raw/**branch**/**path** | Shows a file's raw contents | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/commit/**id** | Shows a commit's details | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/archive/**branch**.zip | Downloads the whole repository as a zip | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/archive/**branch**.tar.gz | Downloads the whole repository as a tar.gz | Only if current user has read access to the repository, or the repository is public |

## Milestones
| URL Format | Description | Comments |
| --- | --- | --- |
| /**owner**/**repository**/issues/milestones | Show the milestones of a repository | Only if current user has read access to the repository, or the repository is public |
| /**owner**/**repository**/issues/milestones/**milestoneID** | Show a milestone of a repository | Only if current user has write access to the repository, or the repository is public |
| /**owner**/**repository**/issues/milestones/**milestoneID**/edit | Edits a milestone of a repository | Only if current user has write access to the repository, or the repository is public |
| /**owner**/**repository**/issues/milestones/**milestoneID**/close | Closes a milestone of a repository | Only if current user has write access to the repository, or the repository is public |
| /**owner**/**repository**/issues/milestones/**milestoneID**/open | Re-opens a milestone of a repository | Only if current user has write access to the repository, or the repository is public |
| /**owner**/**repository**/issues/milestones/**milestoneID**/delete | Deletes a milestone of a repository | Only if current user has write access to the repository, or the repository is public |

## Issues
| URL Format | Description | Comments |
| --- | --- | --- |
| /**owner**/**repository**/issues | Shows the issues of a repository | Only if current user has read access to the repository, or the repository is public. Only if issues are enabled |
| /**owner**/**repository**/issues/**id** | Shows an issue | Only if current user has read access to the repository, or the repository is public |

## Labels
| URL Format | Description | Comments |
| --- | --- | --- |
| /**owner**/**repository**/issues/labels | Shows the labels of issues of a repository | Only if current user has read access to the repository, or the repository is public. Only if issues are enabled |

## Pull Requests
| URL Format | Description | Comments |
| --- | --- | --- |
To do

## Wiki
| URL Format | Description | Comments |
| --- | --- | --- |
To do

## Repository Settings
| URL Format | Description | Comments |
| --- | --- | --- |
To do

## System Settings
| URL Format | Description | Comments |
| --- | --- | --- |
To do

## API
To do
