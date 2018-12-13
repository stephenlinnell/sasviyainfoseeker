
Ansible Script- SAS Viya Information Seeker
-
On our team, we not only complete and validate a deployment, but we also document what was completed in the deployment.  This calls for the deployment specialist to collect information about the environment before ending the engagement with the customer.  Some of this information includes; hostnames, IP adresses, resource sizes, SAS Viya Service locations etc.  To streamline the collection of all of this information we have developed an Ansible Playbook to create a single file for the consultant to review when preparing Post Deployment Documentation.

Supported Operating Systems
--
* RedHat Linux 7.1+
* SUSE Linux 12+

Information collected by the SAS Viya Information Seeker
--
The following information will be retrieved from each Ansible host listed in inventory.ini ('''inventoryname''' refers to the name given for each host in inventory.ini)
* '''inventoryname/''' - A directory for each host in the environment containing:
** '''inventoryname'''rpm.txt - A text file with ALL SAS Viya RPMS installed on the host 
** '''inventoryname'''services.txt - A text file with ALL SAS Viya Serivces with their current status
** '''inventoryname'''info.txt - A text file with the following information
*** Hostname (the result of hostname -f)
*** IP Address (the result of hostname -i)
*** Number of cores on the host (the result of nproc)
*** Amount of RAM on the machine (the result of free -h)

This playbook will generate files that contain the following from the hosts that contain an instance of the Identities and HTTP Proxy Services
* '''inventoryname'''identities.txt - A text file with all fields from the SAS Viya Identities Configuration
* '''inventoryname'''urls.txt - A text file with all ACTIVE URLs for the current environment

Additionally this playbook will copy '''vars.yml''' and '''inventory.ini''' to the location for packaging

Getting Started with the SAS Viya Information Seeker
--
* Download the .yml file from my GitHub
* Place the file in you sas_viya_playbook directory (NOTE: This playbook will utilize your ansible.cfg, inventory.ini and vars.yml from the deployment)
* Run the playbook

Playbook Facts
--
* The playbook will run and create temporary files be default in /tmp on all hosts
* The playbook will default to return a .tar.gz with all items listed above
* This playbook must be run with a user that has sudo permissions

Executing the Ansible-Playbook
--

    ansible-playbook viya-info-seeker.yml
To run this playbook with a user that has a sudo password use the following to execute:
    
    ansible-playbook viya-info-seeker.yml --ask-become-pass
To run this playbook with a user that has an ssh password use the following to execute:
    
    ansible-playbook viya-info-seeker.yml --ask-pass  
To overwrite the location of temporary files use the following to execute: (Note this location must be present on all hosts)
    
    ansible-playbook viya-info-seeker.yml -e "RemoteTMP=/tmp/location/"
To overwrite where to save the .tar.gz file use the following to execute:
    
    ansible-playbook viya-info-seeker.yml -e "LocalPATH=/home/myuser/location"
Example of combining all of the above:
    
    ansible-playbook viya-info-seeker.yml --ask-become-pass --ask-pass -e "RemoteTMP=/tmp/location/ LocalPATH=/home/myuser/location"

