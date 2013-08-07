<table>
<tr><th>type</th><th>message</th><th>additional information</th></tr>
<tr><td>create_repository</td><td>$user created $owner/$repo</td><td>-</td></tr>
<tr><td>open_issue</td><td>$user opened issue $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>close_issue</td><td>$user closed issue $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>close_issue</td><td>$user closed pull request $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>reopen_issue</td><td>$user reopened issue $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>comment_issue</td><td>$user commented on issue $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>comment_issue</td><td>$user commented on pull request $owner/$repo#$issueId</td><td>-</td></tr>
<tr><td>create_wiki</td><td>$user created the $owner/$repo wiki</td><td>$page</td></tr>
<tr><td>edit_wiki</td><td>$user edited the $owner/$repo wiki</td><td>$page</td></tr>
<tr><td>push</td><td>$user pushed to $owner/$repo#$branch to $owner/$repo</td><td>$commitId:$shortMessage\n*</td></tr>
<tr><td>create_tag</td><td>$user created tag $tag at $owner/$repo</td><td>-</td></tr>
<tr><td>create_branch</td><td>$user created branch $branch at $owner/$repo</td><td>-</td></tr>
<tr><td>fork</td><td>$user forked $owner/$repo to $owner/$repo</td><td>-</td></tr>
<tr><td>open_pullreq</td><td>$user opened pull request $owner/$repo#issueId</td><td>-</td></tr>
<tr><td>merge_pullreq</td><td>$user merge pull request $owner/$repo#issueId</td><td>-</td></tr>
</table>