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
  },
  {
    "url": "https://github.com/frappe/newsletter",
    "branch": "develop"
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

### Clone repo

```shell
git clone https://github.com/frappe/frappe_docker
cd frappe_docker
```

### Build image

```shell
docker build \
  --build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg=FRAPPE_BRANCH=version-15 \
  --build-arg=APPS_JSON_BASE64=$APPS_JSON_BASE64 \
  --tag=ghcr.io/stevenuster/erpnext/erpnext:latest \
  --file=images/layered/Containerfile .
```

### Push image

```shell
docker push ghcr.io/stevenuster/erpnext/erpnext:latest
```
