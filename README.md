# Provision a Vagrant box and deploy Sinatra application with Git post-receive hook

### Goals

#### Consitently repeatable
Remove human error. Provision a machine from a defined state. This definition can be used to configure as many machines as needed. Can be extended to multiple OS platforms or cloud providers.

#### Idempotent
Each configuration management run checks the state of the system and adjusts it according to the pre defined state.

#### Captured effort
All work is under version control. Provide a shared codebase that unifies different teams.

#### Executable self documentation
Serves as documentation which validates the systems and inverse is also true - systems validate documentation. Both guarantee each others quality.

### Future Roadmap

* Terraform for Infrastructure Automation on a cloud provider
* Jenkins for Continuous Delivery and testing of the application
* Introduce Docker to package the application for portability
* Make Ansible roles OS independent
* Utilise Ansible vault for passwords or use Hashicorp Vault
* Disable SSH root access
* Improve documentation and code comments
* Add regular server backups and notification via email


## Start here to Deploy Sinatra Application

Assume Ansible, VirtualBox and Vagrant are installed on the developer's OSX machine.

If not please see:

* http://docs.ansible.com/ansible/intro_installation.html
* https://www.virtualbox.org/wiki/Downloads
* https://www.vagrantup.com/intro/getting-started/install.html

1. Clone this repository and `cd` into it.

2. In `site.yml` file add a public SSH key to `deploy_key` variable. Associated private key has to be installed on the developer's machine. This is used to deploy the code as described below.

3. Bring Vagrant machine up, configured in `Vagrantfile`:

```
    vagrant up
```

Now the machine is setup and ready for application deployment. The application will be deployed by utilising git post-receive hook, deployed with Ansible in the step above.

4. To deploy simple-sinatra-app code changes from the local filesystem to the dev machine, clone and go into the repository directory:

```
    git clone https://github.com/rea-cruitment/simple-sinatra-app.git
    cd ~/Dev/simple-sinatra-app
```

5. Add new git remote. This destination was setup with Ansible above. Authentication is possible because of the `deploy_key` variable, added in `site.yml` playbook

```
    git remote add dev ssh://deploy@localhost:2222/home/deploy/sinatra.git
```

6. Deploy to the development machine

```
    git push dev master
```

Note: when the Vagrant host is destroyed and re-created, there will be SSH fingerprint warning. In that case remove the corresponding line from `~/.ssh/known_hosts`

Repeat the above step when ever deploying any new changes to the app.

Access Sinatra instance locally running at *http://localhost:8080*

Now that the deployment structure is setup, it can be easily ported when a production remote and host are added. The setup encourages the developer to commit often during the development however can be optimised for live changes in the future.
