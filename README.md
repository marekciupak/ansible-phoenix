# ansible-phoenix

Scripts for provisioning a server to host [Phoenix Framework](https://www.phoenixframework.org/) app.

## Requirements

### Target machine

* A server with [Debian GNU/Linux](https://www.debian.org/) 10 (Buster) operating system.

### Control machine

* Install [Python](https://www.python.org/)

  <details>
    <summary>See sample instruction for macOS users</summary>

    #### Installing Python on macOS using [Homebrew](https://brew.sh/) and [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm):

    ```shell
    brew install asdf
    echo -e "\n. $(brew --prefix asdf)/asdf.sh" >> ~/.zshrc
    echo -e "\n. $(brew --prefix asdf)/etc/bash_completion.d/asdf.bash" >> ~/.zshrc
    asdf plugin-add python
    asdf install python 3.8.2
    asdf global python 3.8.2
    pip install --upgrade pip
    ```
  </details>

* Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

  <details>
    <summary>See sample instruction for macOS users</summary>

    #### Installing Ansible using [Homebrew](https://brew.sh/):

    ```shell
    pip install ansible
    asdf reshim python # if you use asdf-vm
    ```
  </details>

## Configuration

1. Copy sample config:

    ```shell
    cp vagrant/inventory inventory && cp -r vagrant/host_vars/ host_vars/
    ```

2. Update sample `inventory` and `host_vars/` (in main directory of this repo) with your own config.

## Usage

### Preparation (first run)

Before you run the whole script, you may need to create a non-root user(s):

```shell
ansible-playbook playbook.yml -i inventory -u root -t users
```

### Running scripts

```shell
ansible-playbook playbook.yml -i inventory -u admin
```

## Testing locally (using vagrant)

```shell
(cd vagrant/ && vagrant up)
ansible-playbook playbook.yml -i vagrant/inventory -u root -t users
ansible-playbook playbook.yml -i vagrant/inventory -u admin
(cd vagrant/ && vagrant halt)
```
