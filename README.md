# README

## Developer Workspace

[![Contribute](https://che.openshift.io/factory/resources/factory-contribute.svg)](https://che.openshift.io/factory/resources/factory-contribute.svg[link="https://che.openshift.io/f?url=https://raw.githubusercontent.com/l0rd/nodejs-sample/master/devfile.yaml)

This Che Factory can also be invoked with any host:
{hostURL}/f?url=https://github.com/slemeu
It will read the `devfile.yaml` from the repository to instanciate the developer workspace.

## DEMO Preparation steps

### Deploy minikube

### Deploy Che

### Deploy kube sample application

```bash
REPO="l0rd/nodejs-sample"
BRANCH="master"
K8S_MANIFEST_DIR="https://raw.githubusercontent.com/${REPO}/${BRANCH}/kubernetes-manifests"
NAMESPACE="nodejs-sample"
kubectl create namespace "${NAMESPACE}"
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/mongo.deployment.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/mongo.service.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-backend.deployment.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-backend.service.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-frontend.deployment.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-frontend.service.yaml
```

## DEMO flow

1. Show Kubernetes application running: `minikube service -n ${NAMESPACE} node-guestbook-frontend`
2. Show source code:
3. Generate a devfile with chectl

    ```bash
    chectl devfile:generate --git-repo=https://github.com/l0rd/nodejs-sample.git --language=typescript --plugin=redhat/vscode-yaml/latest
    ```

4. Run the workspace:

    ```bash
    chectl devfile:generate --git-repo=https://github.com/l0rd/nodejs-sample.git --language=typescript > devfile.yaml
    chectl workspace:start --devfile devfile.yaml
    ```

5. Show the prod devfile
6. Open the existing "prod" workspace

    ```bash
    chectl workspace:start --devfile=https://raw.githubusercontent.com/l0rd/nodejs-sample/master/devfile.yaml
    ```

7. Run application

    ```bash
    node --inspect=9229 app.js
    ```

8. Debug appliaction


