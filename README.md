# teracy-dev-entry for k8s

A convenient teracy-dev-entry with default extensions to setup a local k8s cluster working the same as
k8s cluster in the cloud


## How to use

- Clone `teracy-dev` v0.6 and init teracy-dev-entry to `vagrant up`:

```bash
$ cd ~/
$ git clone https://github.com/teracyhq/dev.git -b develop k8s-dev
$ cd k8s-dev
$ TERACY_DEV_ENTRY_LOCATION_GIT=https://github.com/teracyhq-incubator/teracy-dev-entry-k8s.git \
  TERACY_DEV_ENTRY_LOCATION_BRANCH=develop TERACY_DEV_ENTRY_LOCATION_SYNC=true \
  vagrant up
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

See: https://github.com/teracyhq-incubator/teracy-dev-k8s#accessing-kubernetes-api
