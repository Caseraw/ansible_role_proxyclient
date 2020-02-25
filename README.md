# Ansible role proxy client

Ensure access to environment proxy values.

- [Ansible role proxy client](#ansible-role-proxy-client)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure privileged permissions are set for the user executing this role to:
  - Edit global environment files

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_proxyclient_environment_variable_list | List of proxy environment variables. |

## Example Playbook

```yaml
---
- name: Ensure access to environment proxy values
  become: True
  gather_facts: False
  roles:
   - role: ansible_role_proxyclient

...
```

## Useful shell commands

During deployment a file `/etc/profile.d/proxyclient.sh` is placed on the remote global user profile directory. It gets loaded upon a successful login by any authorized user process. Once it gets loaded, the following commands are made available to switch the variables on and off from the user perspective.

```shell
$ setproxy

$ unsetproxy
```

Example output:

```shell
function setproxy(){
    export http_proxy="http://proxy.example.com:8080"
    export HTTP_PROXY="http://proxy.example.com:8080"
    export https_proxy="http://proxy.example.com:8080"
    export HTTPS_PROXY="http://proxy.example.com:8080"
    export ftp_proxy="http://proxy.example.com:8080"
    export FTP_PROXY="http://proxy.example.com:8080"
    export no_proxy="http://something.example.com:8080"
    export NO_PROXY="http://something.example.com:8080"
}

function unsetproxy(){
    unset http_proxy
    unset HTTP_PROXY
    unset https_proxy
    unset HTTPS_PROXY
    unset ftp_proxy
    unset FTP_PROXY
    unset no_proxy
    unset NO_PROXY
}
```

Actual environment values:

```shell
[user@host /]$ env | grep -i proxy

NO_PROXY=http://something.example.com:8080
http_proxy=http://proxy.example.com:8080
FTP_PROXY=http://proxy.example.com:8080
ftp_proxy=http://proxy.example.com:8080
HTTPS_PROXY=http://proxy.example.com:8080
https_proxy=http://proxy.example.com:8080
no_proxy=http://something.example.com:8080
HTTP_PROXY=http://proxy.example.com:8080

```

## Additional documentation resources

N/A

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

Some specific configurations might require a full OS instead of a minimal container image. In these use-cases make use of [molecule driver for vagrant](https://molecule.readthedocs.io/en/latest/configuration.html#vagrant) with the [libvirt provider](https://molecule.readthedocs.io/en/latest/configuration.html#molecule-vagrant-module). The Molecule driver and platform configuration part could look something like this:

```yaml
driver:
  name: vagrant
  provider:
    name: libvirt
platforms:
  - name: ansible_role_proxyclient-ansible-molecule-centos-7
    box: centos/7
    imemory: 1024
    cpus: 1
```

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_proxyclient>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_proxyclient>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_proxyclient>
