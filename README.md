# Webanno

Ansible script which installs [WebAnno](https://webanno.github.io/webanno/). WebAnno is a Tomcat Java Webapp.

# Prerequisites
You need 
 - Ansible and Openstack, see ../portal/README.md
 - Install needed ansible roles via ```ansible-galaxy install -r requirements```
 - A recent backup of the portal from korp:/var/backup : DOW-webanno_backup.tar.gz. DOW is a three letter abbreviation of the present day, use `LC_ALL=C date +%a` to determine it. If your backup is from another day before you must rename it.


# Configuration
WebAnno is installed to cPouta, to webanno-pre-prod per default.
See group_vars/all and host_vars/webanno-pre-prod for configuration parameters.

# Get a most recent backup
Optional: To get the most recent state, run ```/etc/cron.daily/daily_server_backup``` on the present production machine and copy the backup from /var/backup on that machine to this directory.

## Create the preproduction machine

```
ansible-playbook webannoPouta.yml
```

## Move to production
Login to the "portal-prod" instance and set the webanno internal IP adress in /etc/hppd/conf.d/webanno.conf.
Change the name of the VM in pouta.csc.fi from *webanno-pre-prod* to *webanno-prod*. This protects the VM from being accidentally overwritten.
