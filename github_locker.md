# Github Locker Plugin

Feature: autolock merges to master branch in GitHub when RadiatorView is red
(when there are failing tests).

- `radiator plugin -> red` | then | `lock master branch`
- `failed job claimed` | then | `push rights to master`
- `radiator plugin -> green` | then | `unlock master branch`
- `radiator plugin change status` | then | `post in slack`

The master branch is _protected_, which means that `tests` and `checks` must
succeed before a merge. However, there is currently no support in the API for
restricting the branch (but it is an upcoming feature).

# Main structure

1. A plugin in Jenkins that tracks the status of RadiatorView
2. Some link between RadiatorView status and GitHub
3. Still no idea of how to lock master branch automatically.

# TODO:
- post claimers
- install on EdgeWare Jenkins

# Questions
- get status from `RadiatorView`
- how does RadiatorView get access to all jobs? This can be used to have a listener if there are failed jobs.
- how do I use GitHub _tokens_ for secure communication?
- distinguish instances of `RadiatorView`
- make sure the plugin updates frequently (and posts on change)

# Strategies
Choosen: Meta Job.
### GitHub API: Services
Webhook in GitHub --> Jenkins --> check Radiator Status --> Send status
'failed'/'access' back.
### cURL to restrict branch
Need secure communitcation with _TOKEN_s.
### Auto-revert commits
### Meta Job
A job in jenkins that sends a status to GitHub depending on the status of
`RadiatorView`.
- read on how to make status check

