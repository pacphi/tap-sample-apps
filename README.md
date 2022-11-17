# Sample workloads for Tanzu Application Platform

Curated collection of [Workload](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-cli-plugins-apps-create-workload.html) definitions.


## Prerequisites

* a cluster with Tanzu Application Platform (1.3.0 or better) installed
  * `KUBECONFIG` environment variable set with authorized user account
* [kapp](https://carvel.dev/kapp/docs/latest/install/) and [ytt](https://carvel.dev/ytt/docs/latest/install/) CLIs installed on local workstation


## How to use

You may opt to deploy and delete these samples one-by-one or all-at-once.

### One-by-one

To deploy

```bash
kapp deploy --app {app-name} --file <(ytt -data-value namespace={namespace} --file {path-to-file}) --diff-changes --yes
```

> Replace `{app-name}`, `{namespace}`, and `{path-to-file}` place-holders above as appropriate

Samples

```bash
kapp deploy --app basic-java-app --file <(ytt --data-value namespace=workloads --file basic/java-sample.yml) --diff-changes --yes
kapp deploy --app tested-java-app --file <(ytt --data-value namespace=workloads --file with-tests/java-maven-sample.yml) --diff-changes --yes
```

To delete

```bash
kapp delete --app {app-name} --diff-changes --yes
```

> Replace `{app-name}` place-holder above as appropriate


### All-at-once

Note that all workloads will be deployed to a single target namespace.

```bash
cat <<EOF >>values.yml
namespace: {namespace}
EOF

kapp deploy --app basic-apps --file deploy-basic.yml --diff-changes --yes
kapp deploy --app tested-apps --file deploy-with-tests --diff-changes --yes
```

> Replace `{namespace}` place-holder above as appropriate.  Namespace must already exist in cluster.  Also known as a "developer namespace".

To remove all workloads from a namespace

```bash
cat <<EOF >>values.yml
namespace: {namespace}
EOF

kapp delete --app {collection-name} --diff-changes --yes
```

> Replace `{collection-name}` and `{namespace}` place-holders above as appropriate.  Namespace must already exist in cluster.  Also known as a "developer namespace".
