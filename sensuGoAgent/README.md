# **Sensu Go Agent Installer**

## **How To Use**
1. Clone the repositroy
2. Install ansible
3. Run the follwoing command:
`	$ ansible-playbook install_sensu_go_agent.yml`

## **How it works**
This installer constests of config, setup and templates.
### configs:
  Contains the configuration playbook files of sensu.
### setup:
  Contains the setup playbook files for RedHat Families and Debian Families and Cloudlinux so far.
### templates
  Contains the template files that will be used by config playbooks.
### The Main Playbook: install_sensu_go_agent.yml
When you run the installer inital playbook install_sensu_go_agent.yml the logic will start to gather facts "OS Distribution", and then will import the proper task to handover it to one of three files inside setup directory or all of them in case of installing the agent on multi distributions servers
  * rhel-family_srvrs.yml
  * debian-family_srvr.yml
  and
  * other-dists_srvrs.yml - *prepared so far for Cloudlinux distribution*

This playbook is ready for the following distributions:
1. RedHat 7 and 8
2. CentOS 7 and 8
3. Debian 8, 9 and 10
4. Ubuntu 16.04, 18.04 and 20.04
5. CloudLinux on CentOS 7 and it can modified to work on CentOS 8 if needed

**When use AWX, you have to set these variables like the following:**

```
---
SENSU_CONFIGS:
  - {
  SENSU_NAMESPACE: my_namespace,
  SENSU_SUBSCRIPTION: my_subscription,
  SENSU_BACKEND: backend.sensu.tld,
  SENSU_BACKEND_IP: xx.xxx.xxx.xx,
  SENSU_PORT: xxxx
  }
```
