# ERPnext + Modules

### Create app.json

```json
[
  {
    "url": "https://github.com/frappe/erpnext",
    "branch": "version-15"
  },
  {
    "url": "https://github.com/frappe/payments",
    "branch": "version-15"
  },
  {
    "url": "https://github.com/frappe/hrms",
    "branch": "version-15"
  },
  {
    "url": "https://github.com/frappe/webshop",
    "branch": "version-15"
  }
]
```

### Export ENV

```shell
export APPS_JSON_BASE64=$(base64 -w 0 ./apps.json)
```

### Test ENV

```shell
echo -n ${APPS_JSON_BASE64} | base64 -d > apps-test-output.json
```

### Clone Repo

```shell
git clone https://github.com/frappe/frappe_docker
cd frappe_docker
```
