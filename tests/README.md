# Testing the role
This is currently a little bit hackish.

The test setup is simply a sym-link to the role itself and two simple tests:

* [test-deploy.yml](./test-deploy.yml) for testing the actual deployment
* [test-remove.yml](./test-remove.yml) for testing removal

Your test environment is your _local_ machine (sic!), it needs the following pre-requisites:

* `docker`, `docker-compose`, _writable_ `/tmp`
* the host is `localhost`, thus you need to be able to do `ssh localhost` without any password
* the actual role is not installed globally, because this would have precedence over the local one; thus make sure that you have called 
`ansible-galaxy remove zal_ari.deploy_docker_compose` _before_ running the test

## Testing
If you have a _ssh-enabled_ `localhost`, you can use the supplied [inventory](./inventory)


1. `ansible-playbook -i inventory test-deploy.yml` should be good
1. `ansible-playbook -i inventory test-remove.yml` should remove above stack
1. `ansible-playbook -i inventory test-wrong-compose-binary.yml` should fail
