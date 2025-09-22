# ERPnext + Modules

### Update apps.json

Add all modules you want to install to erpnext in the apps.json file.

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
  --build-arg FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg FRAPPE_BRANCH=version-15 \
  --build-arg APPS_JSON_BASE64=$APPS_JSON_BASE64 \
  --tag ghcr.io/stevenuster/erpnext/erpnext:latest-germanized-amd64 \
  --file images/layered/Containerfile .
```

### Optional: multi-platform build image

```shell
# Build and push amd64 image
docker buildx build \
  --platform linux/amd64 \
  --build-arg FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg FRAPPE_BRANCH=version-15 \
  --build-arg APPS_JSON_BASE64=$APPS_JSON_BASE64 \
  --tag ghcr.io/stevenuster/erpnext/erpnext:latest-germanized-amd64 \
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
  --tag ghcr.io/stevenuster/erpnext/erpnext:latest-germanized-arm64 \
  --file images/layered/Containerfile \
  --push .
```

### Create manifest

```shell
docker manifest create ghcr.io/stevenuster/erpnext/erpnext:latest-germanized \
  --amend ghcr.io/stevenuster/erpnext/erpnext:latest-germanized-amd64 \
  --amend ghcr.io/stevenuster/erpnext/erpnext:latest-germanized-arm64
```

### Push manifest

```shell
docker manifest push ghcr.io/stevenuster/erpnext/erpnext:latest-germanized
```
