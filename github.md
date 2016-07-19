
Base url:

```https://api.github.com```


# User access
1. ```-u user:password```
2. 2-Factor Authentication ```-H 'X-GitHub-OTP: 123456'```
3. ACCESS TOKEN


# Protected Branches API


Must use ```Accept``` header ```application/vnd.github.loki-preview+json```.

- List branches ```GET /repos/:owner/:repo/branches```
- Get branch ```GET /repos/:owner/:repo/branches/:branch```
- Get branch protection ```GET
  /repos/:owner/:repo/branches/:branch/protection```
- Update branch protection ```PUT
  /repos/:owner/:repo/branches/:branch/protection```
- Remove branch protection ```DELETE
  /repos/:owner/:repo/branches/:branch/protection```
- Get required status checks of protected branch ```GET
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks```
- Update required status checks of protected branch ```PATCH
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks```
- Remove required status checks of protected branch ```DELETE
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks```
- List required status checks contexts of protected branch ```GET
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks/contexts```
- Replace required status checks contexts of protected branch ```PUT
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks/contexts```
- Add required status checks contexts of protected branch ```POST
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks/contexts```
- Remove required status checks contexts of potected branch ```DELETE
  /repos/:owner/:repo/branches/:branch/protection/required_status_checks/contexts```
### ```restrictions``` only available for organization-owned repositories.
- Get restrictions of protected branch ```GET
  /repos/:owner/:repo/branches/:branch/protection/restrictions```
- Remove restrictions of protected branch ```DELETE
  /repos/:owner/:repo/branches/:branch/protection/restrictions
```
- List team restrictions of protected branch ```GET
  /repos/:owner/:repo/branches/:branch/protection/restrictions/teams```
- Replace team restrictions of protected branch ```PUT
  /repos/:owner/:repo/branches/:branch/protection/restrictions/teams```
- Add team restrictions of protected branch ```POST
  /repos/:owner/:repo/branches/:branch/protection/restrictions/teams```
- Remove team restrictions of protected branch ```DELETE
  /repos/:owner/:repo/branches/:branch/protection/restrictions/teams```
- List user restrictions of protected branch ```GET /repos/:owner/:repo/branches/:branch/protection/restrictions/users```
- Replace user restrictions of protected branch ```PUT
  /repos/:owner/:repo/branches/:branch/protection/restrictions/users```
- Add user restrictions of protected branch ```POST
  /repos/:owner/:repo/branches/:branch/protection/restrictions/users```
- Remove user restrictions of protected branch ```DELETE
  /repos/:owner/:repo/branches/:branch/protection/restrictions/users
```


# Status API
- ```state``` = ```pending``` | ```success``` | ```error``` | ```failure```
- ```target_url```
- ```description```
- ```context```


### Example:
```
{
  "state": "success",
  "target_url": "https://example.com/build/status",
  "description": "The build succeeded!",
  "context": "continuous-integration/jenkins"
}
```


# Webhook
Recieves ```POST``` requests. 

