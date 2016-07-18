
Base url:

```https://api.github.com```

- endpoints like ```/repos/user``` are added to this url.

# User access
1. ```-u user:password```
2. 2-Factor Authentication ```-H 'X-GitHub-OTP: 123456'```
3. ACCESS TOKEN


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

