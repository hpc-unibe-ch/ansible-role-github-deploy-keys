github-deploy-keys
=========

This role configures github deploy keys on a ansible host and manages
a users ssh-config to use the generated keys for authentication with github.

This role can be useful when automatic access to private or internal repositories
needs to be configured. A possible use case is [Ansible Semaphore](https://www.ansible-semaphore.com/).


Role Variables
--------------

```yaml
gdk_deploy_keys:
  - name: "repo_name"
    user: "root"
    key: "/root/.ssh/id_rsa_name"
    deploy_keyname: "deploy-token-name"
    repo: "repo"
    owner: "repo_owner"
    token: "github_securetoken"
    state: "present"

gdk_manage_ssh_config: false
gdk_ssh_config_path: '/root/.ssh/config'
```

A list of dictionaries controls the repositories that this role should grant access to. If you need multiple repositories, 
simply copy the `-name` block and adapt the repository settings. 

 * `name` sets the repository name (currently only used as unique identifier)
 * `user` set the user account on the ansible host that should own the key.
 * `key` is the name of the ssh-key to be installed
 * `deploy_keyname` is the name of the deploy key as shown on Github
 * `repo` sets the github repository to access
 * `owner` sets the github account / organization the key should access
 * `token` is the github access token used with the API to register the keys
 * `state` (default is present). Can be set to absent to remove deploy keys

For details of the dict keys please see the
[the ansible github_deploy_key module](https://docs.ansible.com/ansible/latest/collections/community/general/github_deploy_key_module.html)
documentation.

```yaml
gdk_manage_ssh_config: false
```

boolean to control whether the user ssh-config. *Note:* This is currently a template. Any other contents of an existing ssh-config will be overwritten.

```yaml
gdk_ssh_config_path: '/root/.ssh/config'
```
Set the path of the ssh config file to modify.


Example Playbook
----------------

```yaml
- name: Generate deploy keys and ssh config for semaphore
  hosts: semaphore
  vars:
    gdk_deploy_keys:
      - name: "repo_name"
        user: "root"
        key: "/root/.ssh/id_rsa_name"
        deploy_keyname: "deploy-token-name"
        repo: "repo"
        owner: "repo_owner"
        token: "github_securetoken"
        state: "present"
    gdk_ssh_config_path: "/var/lib/semaphore/.ssh/config"
    gdk_manage_ssh_config: true
  roles:
    - role: ubelix.github_deploy_keys
``` 

License
-------

MIT

