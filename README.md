# Suzaku Operator Boilerplate

Boilerplate repository to spin up a testnet [Suzaku Operator](https://docs.suzaku.network/suzaku-restaking/for-operators/introduction) using the [ash.avalanche](https://github.com/AshAvalanche/ansible-avalanche-collection) Ansible collection.

## Documentation

The complete documentation related to this repository can be found in the [Suzaku Operator Guide](https://docs.suzaku.network/suzaku-restaking/for-operators/operator-guide).

## Requirements

- Python >=3.9 with `venv` module installed
- A VPS / bare metal machine with Ubuntu 22.04 LTS / 24.04 LTS and SSH access to a user with `sudo` privileges

## Setup the environment

1. Clone the Getting Started repository:

   ```bash
   git clone https://github.com/suzaku-network/suzaku-operator-boilerplate
   cd suzaku-operator-boilerplate
   ```

2. Setup and activate Python venv:

   ```bash
   bin/setup.sh
   source .venv/bin/activate
   ```

3. Install the [`ash.avalanche`](https://github.com/AshAvalanche/ansible-avalanche-collection) collection:

   ```bash
   ansible-galaxy collection install git+https://github.com/AshAvalanche/ansible-avalanche-collection.git
   ```

4. Edit the `inventories/fuji/hosts` file to include the IP addresses of the Avalanche nodes:

   ```ini
   suzaku-testnet-node ansible_host=X.X.X.X avalanchego_http_host=0.0.0.0 ansible_user=ubuntu ansible_ssh_private_key_file=files/ansible_key.pem
   ```

   -  Replace `X.X.X.X` with the IP address of the VPS / bare metal machine used to run the Suzaku Operator.
   -  Provide the path to `ansible_ssh_private_key_file` to allow Ansible to SSH into the target machine with the configured `ansible_user`.

## Run the Avalanche node

1. Bootstrap the Avalanche nodes:

   ```bash
   ansible-playbook ash.avalanche.provision_nodes -i inventories/fuji
   ```

   This command **installs AvalancheGo** on the target machine, configures it to **track the *SuzakuSeiryu* Suzaku network** and **start the AvalancheGo service**.

## Node registration

See the [Suzaku Operator Guide](https://docs.suzaku.network/suzaku-restaking/for-operators/operator-guide#node-registration) for instructions on how to register your node to become a validator.
