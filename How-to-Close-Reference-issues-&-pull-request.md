# Closing issues

Find below a list of keywords that MUST appear in a commit message to perform a `close` action on an issue.

The message MUST containt a string matching the following pattern: `KEYWORD #ISSUE`. For example: `close #1`

- `close #X`
- `closes #X` 
- `closed #X` 
- `fix #X` 
- `fixes #X` 
- `fixed #X` 
- `resolve #X` 
- `resolves #X` 
- `resolved #X` 

It is also possible to close several issues in the same commit: just repeat several time the pattern to close issues. For example, the following commit message: `add new quick sort algorithm, fixes #4, resolve #6, closed #12` would close, the issues 4, 6 & 12 of the project on which the commit would occure.

# Closing Pull Request

Not yet available in version `3.4

# Referencing Isssues and Pull Requests

just use the pattern `#ISSUE` in a commit message or in any comment on issues/PRs to make an html link reference to the item corresponding to `#ISSUE`.