# Ansible Patchworks for list of hosts-PoC

_Assuming that this this IT Estate has hundreads of RHEL/CentOS machines. It needs frequent patch works from ansible control/management server. 90% of those hosts can be reached from control server. Remaining 10% of those hosts we need to copy the scripts and execute them._

**Following list of activities described in this PoC**
  - Group the hosts(from host01 to host06) into category01
  - Rest of the hosts can be grouped into category02- this needs separate action plan to execute the patch works
  - List out various pre_tasks /checks need to made in each hosts
  - Sent out an Email to Concerned team/Privileged Users about Patch works which is ready to deployed and asking them to save their current work.[Ansible supports various other notifications including slack coms.]
  - To aknowledge the above email-ansible expects particular lock file in folder /var/lock needs to be removed
  - Once lock file removed, ansible apply the patches and reboot the machine and restared various services.
  - Sends Email about system availability
  - logs the details about successful deployment of patches into logfile. This needs to be pulled by fileBeats from ELK stack and analyzed/reported for MI purpose

## Ansible Playbook
  ### Inventory
    Sample Inventory consisits of host01 to host06 under category01
    Assuming that this hosts can be reached from control server.
    Ansible config file(ansible.cfg) kept in the current working folder.

    Some of the tasks needs privilege access. Hence ansible-vault used to keep the prvilege access details

    Env variable set to ANSBIBLE_VAULT_PASSWORD_FILE and these setup are in scripts folder

  ### Variables
    Privilege_user which has got encrypted access privilge details for root privileges. This file is included
    smtp mail server host, port and lock file details are declared as variables in the playbook

  ### Pre_tasks
    Simulate that there are going to be n number of pre-tasks which needs to be carried out before coming to main tasks. we have 2 pre_tasks to simulate the same

  ### Tasks
    This takes care of creating Lock file and sending Emails to various ops/application team about system ready for patch deployment. These tasks executed only when the above pre_tasks ran successfully.

    Task- Look out for Lock file removal
    using ansible wait_for module, playbooks halts until the lock file removed from remote hosts. Once the lock file removed, ansible applies necaary patches and reboots the machine.

**Note:** *Following Tasks is not fully completed. It needs to be worked upon* 
  - This entire set up needs to be converted as ansible roles
  - smtp server setup. As of now we setup smtp mail forward server using *postfix* as MTA and *mailx* as MUA programs.
  - creation of log files and forward them to fileBeats/Logstash, ElasticSearch and Kibana are not included



