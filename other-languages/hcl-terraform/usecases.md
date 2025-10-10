# Issues / Troubleshootings

## AWS



## Azure
### 403 Error on local 'tf init'
-> check backen configuration file e.g. <env>.backend.hcl 
-> if there is any auth related code, delete it. azure_user_auth = true

### import error of role assignment
-> list role assignments
```

```
