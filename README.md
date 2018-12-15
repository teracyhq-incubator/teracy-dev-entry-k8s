# teracy-dev-entry for k8s

A convenient teracy-dev-entry with default extensions to setup a local k8s cluster working the same as
k8s cluster in the cloud


## Prerequisites

- Remember to skip this step: http://dev.teracy.org/docs/getting_started.html#teracy-dev-git-clone-and-vagrant-up

- Install Vagrant (>=2.1.0), VirtualBox (>=5.2) by following this guide http://dev.teracy.org/docs/getting_started.html.


## How to use

- Clone `teracy-dev` and init `teracy-dev-entry` to `vagrant up`:

```bash
$ cd ~/
$ git clone https://github.com/teracyhq/dev.git -b v0.6.0-a5 k8s-dev
$ cd k8s-dev
$ TERACY_DEV_ENTRY_LOCATION_GIT_REMOTE_ORIGIN=https://github.com/teracyhq-incubator/teracy-dev-entry-k8s.git \
  TERACY_DEV_ENTRY_LOCATION_GIT_BRANCH=master TERACY_DEV_ENTRY_LOCATION_SYNC=true \
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

You can retry with `$ vagrant up --provision`


## Accessing Kubernetes API

See: https://github.com/teracyhq-incubator/teracy-dev-k8s#accessing-kubernetes-api


## Domain Aliases

To create domain alisases for the master node, override the default values from the
`config_default.yaml` file:

```yaml
nodes:
  - _id: "0"
    plugins:
      - _id: "entry-hostmanager"
        name: vagrant-hostmanager
        config_key: hostmanager
        enabled: true
        options:
          aliases: # set domain aliases for the master node
            - k8s.local
```

with your preferred domain aliases on the `config_override.yaml` file, for example:

```yaml
nodes:
  - _id: "0"
    plugins:
      - _id: "entry-hostmanager"
        options:
          aliases: # set domain aliases for the master node
            - teracy.local
            - auth.teracy.local
            - service.teracy.local
            - my.example.com
```

or by appending more domain aliases:


```yaml
nodes:
  - _id: "0"
    plugins:
      - _id: "entry-hostmanager"
        options:
          _ua_aliases:
            - "auth.%{node_hostname_prefix}.%{node_domain_affix}"
            - "login.%{node_hostname_prefix}.%{node_domain_affix}"
```


As nginx ingress is enabled by default, you can use the domain aliases above with the nginx ingress
configuration to access the provided services from the cluster.


## Certificates

To configure self-signed certificates, please follow https://github.com/teracyhq-incubator/teracy-dev-certs#configuration-reference

For example, you add the following line into the `teracy-dev-entry/config_override.yaml` file:

```
teracy-dev-certs:
  alt_names:
    - "accounts.%{node_hostname_prefix}.%{node_domain_affix}"
    - "login.%{node_hostname_prefix}.%{node_domain_affix}"
```

## How to use the latest develop branch with bleeding edge updates

By default, the stable master branch is always synced. To use the develop branch, we must add the following
config to the `teracy-dev-entry/config_override.yaml` file, for example:


```yaml
teracy-dev:

  entry_location:
    git:
      branch: develop
```

## How to develop

By default, the "master" branch is always synced. To develop, we must create and configure the `teracy-dev-entry/config_override.yaml` file, for example:


```
teracy-dev:

  entry_location:
    git:
      remote:
        origin: git@github.com:hoatle/teracy-dev-entry-k8s.git # your forked repo
        upstream: git@github.com:teracyhq-incubator/teracy-dev-entry-k8s.git
    sync: false # you must update the repo manually
```
