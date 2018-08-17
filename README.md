# teracy-dev-entry for k8s

A convenient teracy-dev-entry with default extensions to setup a local k8s cluster working the same as
k8s cluster in the cloud


## How to use

- Clone `teracy-dev` project v0.6:

```bash
$ cd ~/
$ git clone https://github.com/teracyhq/dev.git k8s-dev
$ cd k8s-dev
$ git checkout develop
```

- Clone the `teracy-dev-entry-k8s` repo into the `workspace` directory:

```
$ cd ~/k8s-dev/workspace
$ git clone https://github.com/teracyhq-incubator/teracy-dev-entry-k8s.git teracy-dev-entry
$ cd teracy-dev-entry
$ git checkout develop
```

- `$ vagrant up`:

```
$ cd ~/k8s-dev
$ vagrant up
```

When ansible reports that everything is ok, check it out:

```bash
$ cd ~/k8s-dev
$ vagrant ssh
$ kubectl cluster-info
$ kubectl version
$ kubectl get pods
```

If ansible is not running sucessfully, for example:

```bash
fatal: [k8s-01]: FAILED! => {"attempts": 5, "changed": false, "cmd": "/usr/local/bin/kubectl get secrets -o custom-columns=name:{.metadata.name} --no-headers | grep -m1 default-token", "delta": "0:00:00.190677", "end": "2018-07-26 15:30:33.207118", "msg": "non-zero return code", "rc": 1, "start": "2018-07-26 15:30:33.016441", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}

NO MORE HOSTS LEFT *************************************************************
  to retry, use: --limit @/vagrant/workspace/kubespray/cluster.retry

PLAY RECAP *********************************************************************
k8s-01                     : ok=354  changed=111  unreachable=0    failed=1

Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.
==> k8s-01: The previous process exited with exit code 1.
```

You can retry with `$ vagrant reload --provision`


## Accessing Kubernetes API

See: https://github.com/kubernetes-incubator/kubespray/blob/master/docs/getting-started.md#accessing-kubernetes-api

You should see the generated artifacts within the `workspace/inventory/artifacts` directory

You can append this config into the `~/.kube/config` file and use `kubectl` anywhere without specifying
the kubeconfig file:

```
$ cd workspace/inventory/artifacts/
$ cat admin.conf > ~/.kube/config # append the generated admin config to the config file
```

Use it:

```
$ kubectl config use-context admin-cluster.local
$ kubectl cluster-info
```


## Configuration Override

To override default config, you need to create `workspace/teracy-dev-entry/config_override.yaml` to
override the values from `teracy-dev-k8s/config_default.yaml`.

For example:

```yaml
teracy-dev-k8s:
  ansible:
    mode: host
    verbose: vv
  vm_memory: 1600
  vm_cpus: 4
  num_instances: 3
```
