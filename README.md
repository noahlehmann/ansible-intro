# How to set up a fresh Linux system (Debian based)

Set up and install by following the installation instructions on terminal.
Once installed, start setting up the base system.

## Set up the base system

When logging into a fresh system the first time, a few things should be done to make sure working with it is smooth.

### Set up a password and move your ssh keys to the system.

Depending on whether you have set a password while installing, set a password manually.

```bash
passwd
```

Modern ubuntu systems will not enable root login by default. If you need this you can enable it by setting a root password.

```bash
sudo passwd root
```

For connecting to the system more easily, move your local systems ssh keys to the new system.
If you are on Linux or MacOS, use the following.

```bash
ssh-copy-id student@10.90.43.11
```

## Use Ansible to set up the system

The tasks showed above can be automated using ansible.

### Initial setup

Create a python virtual environment and activate it.

```bash
python3 -m venv .venv && source .venv/bin/activate
```

Install ansible and the necessary dependencies.

```
pip install -r requirements.txt
```

Create and fill the password file for the vault.

```bash
echo "somevaultpass" > .env/password
```

### Try connecting to your hosts

Once python and ansible are set up, try connecting to all the hosts in the hosts.yaml file.

```bash
ANSIBLE_CONFIG=ansible.cfg ansible all -m ping --private-key ~/.ssh/id_ed25519
```

__NOTE: Make sure the ssh keys are copied, without them ansible relies on `sshpass` to be installed on the system running ansible.__

### Run a playbook

Once you have verified that you can connect to the hosts, run a playbook to set up the system.

```bash
ANSIBLE_CONFIG=ansible.cfg ansible-playbook playbooks/example.yaml --private-key ~/.ssh/id_ed25519 --vault-password-file .env/password
```