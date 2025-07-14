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

### Clone repo

```shell
git clone https://github.com/frappe/frappe_docker
cd frappe_docker
```

### Build image

```shell
# Build and push amd64 image
docker buildx build \
  --platform linux/amd64 \
  --build-arg FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg FRAPPE_BRANCH=version-15 \
  --build-arg APPS_JSON_BASE64=$APPS_JSON_BASE64 \
  --tag ghcr.io/stevenuster/erpnext/erpnext:latest-amd64 \
  --file images/layered/Containerfile \
  --push .
```

```shell
# Build and push arm64 image
docker buildx build \
  --platform linux/arm64 \
  --build-arg FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg FRAPPE_BRANCH=version-15 \
  --build-arg APPS_JSON_BASE64=$APPS_JSON_BASE64 \
  --tag ghcr.io/stevenuster/erpnext/erpnext:latest-arm64 \
  --file images/layered/Containerfile \
  --push .
```

### Create manifest

```shell
docker manifest create ghcr.io/stevenuster/erpnext/erpnext:latest \
  --amend ghcr.io/stevenuster/erpnext/erpnext:latest-amd64 \
  --amend ghcr.io/stevenuster/erpnext/erpnext:latest-arm64
```

### Push manifest

```shell
docker manifest push ghcr.io/stevenuster/erpnext/erpnext:latest
```
