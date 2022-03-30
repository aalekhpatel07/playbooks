# Ansible Playbooks

Manage configurations and states of packages/tooling across multiple machines using `ssh`.

## Setup

- Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) and other requirements with a compatible Python (>= 3.8) on a control node (main/master machine): 

  ```sh
  # Setup a virtual environment.
  virtualenv .venv --python /usr/local/bin/python3.9
  
  # Install the dependencies.
  .venv/bin/python -m pip install -r requirements.txt
  ```

- Set up ssh-key based access to the managed node. 
  - If the control node does not have a ssh keypair generated already, you may generate one by running the command below and then following the instructions that appear:
  	```sh
  	ssh-keygen -t ed25519
  	```
  - Once the location of the public key is known, the following command will copy the control node's ssh key to a node you wish to manage:
  	```sh
  	ssh-copy-id -i ~/.ssh/<public_key>.pub admin@<node-ip>
  	```
  - Make sure all the managed nodes allow privileged access to user `admin` (create with `adduser` if does not exist). This might need sudo credentials. However you do it, make sure `/etc/sudoers` has that entry for `admin`.
  	```sh
  	ssh root@<node-ip> "echo 'admin ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers"
  	```
  - Modify the `vars.yaml` and `hosts.yaml` files with the correct node ips (i.e. add/remove nodes to/from groups, etc.), and location to the ssh key.

## Maintain

- Add new playbooks and include a corresponding entry in the `main.yaml` file.
- Lint the main playbook again: `./lint`
- Generate execution flow graphs: `./make-graphs`

## Usage

- Run the main playbook (or use the `./run-playbook` script),:
	```sh
	ansible-playbook --inventory-file hosts.yaml main.yaml --verbose
	```
- Generate execution flow graphs: `./make-graphs`

## Contributing

- PRs for more playbooks are welcome.
