
# Ansible Role for install Kafka

This role installs Confluent Kafka.

## Role Dependencies

This role depends on 'common' that sets a baseline to the host. Means that 'common' role will be automatically executed before this one.

## Role Variables

| Name                                                                                                    | Required                                        | Description |
|---------------------------------------------------------------------------------------------------------|-------------------------------------------------|-|
| version | Y | Version of Confluent Kafka |
| repository_version | Y | Version of the repository with artefacts to install |
| repositories | Y | List of repository info|
| ⇥ name| Y | Name of the repository |
| ⇥ description| Y | Description of the repository |
| ⇥ file| Y | File to install |
| ⇥ baseurl| Y | URL where the artefacts are |
| ⇥ gpgcheck| Y | Flag to check if the repository is trustable  |
| ⇥ gpgkey| Y | GPG key to check if the repository is trustable |
| ⇥ enabled| Y | Flag to enable the repository|
| kafka_properties | Y | kafka properties|
| ⇥ file | Y | Path to file with kafka properties|
| ⇥ dataDir | Y | Kafka data directory |
| ⇥ other_configs | Y | Other configuration to insert in kafka properties file |

## Role Defaults

This role also defines the following variables in defaults/main.yml which can be overwritten on inventory file:

| Name                                                        | Description |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-|
