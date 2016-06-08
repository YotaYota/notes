# Github Locker Plugin

- `radiator plugin -> red` | then | `lock master branch`
- `failed job claimed` | then | `push rights to master`
- `radiator plugin -> green` | then | `unlock master branch`
- `radiator plugin change status` | then | `post in slack`

# Strategy

- get status from `RadiatorView`
- how does RadiatorView get access to all jobs? This can be used to have a listener if there are failed jobs.
