# README

## Developer Workspace

[![Contribute](https://www.eclipse.org/che/contribute.svg)](https://che.openshift.io/f?url=https://raw.githubusercontent.com/l0rd/nodejs-sample/master/devfile.yaml)

This Che Factory can also be invoked with any host:
{hostURL}/f?url=https://github.com/l0rd/nodejs-sample/
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
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-backend.deployment.prod.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-backend.service.yaml
kubectl apply -n "${NAMESPACE}" -f "${K8S_MANIFEST_DIR}"/guestbook-frontend.deployment.prod.yaml
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
    node install && node --inspect=9229 app.js
    ```

8. Debug application

## TODO

- [ ] Check if it works on openshift.io
- [ ] Fix the `chectl devfile:generate --plugin=` syntax
- [ ] Fix the services problem: I had to change to localhost in deployments...that won't work in prod https://github.com/eclipse/che/issues/14970 https://github.com/l0rd/nodejs-sample/commit/d0e1ec77cc2670f2556e56062b7db8c751c33125
- [ ] Check if we really need to include services in the devfile?
- [ ] To avoid patching the original frontend dockerfile why not replacing it with a nodejs container that works well as arbirary user and has a fancy PS1?
    ===> for example the typescript sidecar?
- [ ] Add instructions to pre-pull the required images
- [ ] Add instructions to set `imagePullStrategy` to `Never` for all containers
- [ ] Use an offline plugin/devfile registries
- [ ] Start prometheus / enable proemetheus endpoints
- [ ] Add the k8s tooling extension to the workspace
- [ ] Enable hot reload (for express to watch dependencies)
- [ ] Try to build with buildah using the k8s tooling and push to the internal registry
- [ ] Use an internal container registry better, if we could use an hostname instead of the IP so that it's repeatable
- [ ] Add one edits to the flow: code is modified: hot reload
- [ ] Add code navigation to the flow
- [ ] Change the guestbook color (should be Che colors) and font (should be Overpass)
- [ ] Make the go version
- [ ] Make the java version
