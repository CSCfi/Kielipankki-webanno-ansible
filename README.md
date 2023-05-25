# Webanno

Ansible script which installs [WebAnno](https://webanno.github.io/webanno/). WebAnno is a Tomcat Java Webapp.

## Prerequisites

### Pouta Access

To deploy services to Pouta, you need:
- [Access to
  Pouta](https://docs.csc.fi/accounts/how-to-add-service-access-for-project/)
- Key pair for cPouta instances. Created in https://pouta.csc.fi/ (Project >
  Compute > Key Pairs) and must be named "kielipouta"
- Your cPouta project's [OpenStack RC
  file](https://docs.csc.fi/cloud/pouta/install-client/#configure-your-terminal-environment-for-openstack).
You need to source the file every time you start a new shell session.


### Software Requirements

Install needed ansible roles via ```ansible-galaxy install -r requirements```

For Python requirements, it is recommended to use a virtual environment:
```
virtualenv .venv -p python3
source .venv/bin/activate
pip install -r requirements_dev.txt
```

The activation step must be done separately for each new session.


## Configuration
WebAnno is installed to cPouta, to webanno-pre-prod per default.
See group_vars/all and host_vars/webanno-pre-prod for configuration parameters.

### Get a most recent backup
Optional: To get the most recent state, run ```/etc/cron.daily/daily_server_backup``` on the present production machine and copy the backup from /var/backup on that machine to this directory.

### Create the preproduction machine

```
ansible-playbook webannoPouta.yml -i inventories/openstack_static
```

### Move to production
Login to the "portal-prod" instance and set the webanno internal IP adress in /etc/hppd/conf.d/webanno.conf.
Change the name of the VM in pouta.csc.fi from *webanno-pre-prod* to *webanno-prod*. This protects the VM from being accidentally overwritten.

It seems there is a problem with the local firewall rules, they are disabled on our present production system --mma 25.5.2023
