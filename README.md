zal_ari.deploy-docker-compose
=========

Deploy a docker-compose stack (TM) remotely.

Requirements
------------

The host needs to have `docker` and `docker-compose` installed and the `ansible_user` must be able to run docker.

Role Variables
--------------

* **docker_compose_path**: Path to `docker-compose` binary defaults to `/usr/local/bin/docker-compose`
* **remote_deploy_base_path**: _Base_ path, where the docker-compose stack is deployed, defaults to `/srv/docker` (which will most likely need _sudo_ perms)
* **remote_deploy_name**: Folder name that gets appended to the **remote_deploy_base_path**; defaults to `docker-stack`
* **remote_pull_images**: Should `docker-compose pull` be run before bringing up the stack? Defaults to `false`.
* **deploy_path**: Path where to _local_ `docker-compose.yml` is stored; will get copied to server and **must** end with `/`!
* _(optional)_ **remote_docker_login_registry**: Private docker registry; initially not set and thus will default to the DockerHub.
* _(optional)_ **remote_docker_login_user**: Login for (private) docker registry; initially not set.
* _(optional)_ **remote_docker_login_pass**: Password for (private) docker registry; initially not set. `
* _(optional)_ **force_recreation**: _Force_ container recreation, by calling `docker-compose down` before bringing the stack `up -d`. It
 defaults to `false`. This is useful if you have changes that a normal `up -d` would not pick up (i.e. changes in _bind-mounted_ volumes).
* _(optional)_ **force_recreation_args**: Additional arguments that are used for the remote `docker-compose down` call _before_ bringing 
up the stack. Defaults to ` || true` (sic!) to prevent failure of `docker-compose down` because it exits with non-zero codes, while for 
example trying to delete an external network.
* **remote_remove_only**: Remove remote docker-compose stack by invoking `docker-compose down`; defaults to `false`. Setting it to true, will skip deploying the stack (but will still copy it).
* **remote_remove_args**: Additional arguments that get passed to `docker-compose down` if **remote_remove_only** is set to `true`. Defaults to `--rmi all --remove-orphans`

**Note:** it is advised to set **remote_docker_login_pass** via the environment when invoking `ansible-playbook` instead of setting it in the actual playbook (and thus keep secrets out of a repo). An easy way to do so would be `ansible-playbook -i inventory -e "remote_docker_login_pass=$MY_DOCKER_LOGIN" playbook-yml`


Dependencies
------------

Nope.

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: zal_ari.deploy_docker_compose
          deploy_path: test/fixture/
          remote_deploy_base_path: /tmp/docker
          remote_deploy_name: hello-world
          remote_pull_images: True

TODO
----
* allow for login into multiple private docker registries
* add docker-based test env
* use Travis for building and automagic releases to Ansible Galaxy.

PRs are welcome.

License
-------

MIT

Author Information
------------------

Christian Ulbrich [christian.ulbrich@zalari.de](), Zalari GmbH
