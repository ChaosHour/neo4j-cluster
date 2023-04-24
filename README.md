# neo4j-cluster
A 3 node neo4j-cluster created with Vagrant and Ansible. Ubuntu Jammy on the OS Side.

## Requirements
* Vagrant
* Ansible
* Virtualbox

## Usage
* Clone this repo
* Run `touch secrets.yml` to create the secrets file for Vagrant inline variables
* Add the following to the secrets.yml file. Do not check in this file to git.
```yaml
user: "Your User Name"
pass: "Your Password"
```
* Run `vagrant status` to see the status of the VMs
* Run `vagrant up` to start the VMs
* Run `ansible -i inventory all -m ping` to verify the VMs are up and running
* Run `ansible-playbook -i inventory build.yml` to build the cluster
* Login to the cluster from a browser at http://192.168.3.11:7474 



## Notes
```bash
klarsen@Mac-Book-Pro2 neo4j-cluster % vagrant status
Current machine states:

node1                     not created (virtualbox)
node2                     not created (virtualbox)
node3                     not created (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.


vagrant up



    node3: Restarting the system to load the new kernel will not be handled automatically, so you should consider rebooting. [Return]
    node3:
    node3: Services to be restarted:
    node3:  systemctl restart packagekit.service
    node3:
    node3: Service restarts being deferred:
    node3:  systemctl restart networkd-dispatcher.service
    node3:  systemctl restart unattended-upgrades.service
    node3:
    node3: No containers need to be restarted.
    node3:
    node3: No user sessions are running outdated binaries.
    node3:
    node3: No VM guests are running outdated hypervisor (qemu) binaries on this host.
==> node3: Running provisioner: shell...
    node3: Running: inline script
    node3: There are 4 choices for the alternative editor (providing /usr/bin/editor).
    node3:
    node3:   Selection    Path                Priority   Status
    node3: ------------------------------------------------------------
    node3: * 0            /bin/nano            40        auto mode
    node3:   1            /bin/ed             -100       manual mode
    node3:   2            /bin/nano            40        manual mode
    node3:   3            /usr/bin/vim.basic   30        manual mode
    node3:   4            /usr/bin/vim.tiny    15        manual mode
    node3:
    node3: Press <enter> to keep the current choice[*], or type selection number: update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in manual mode


klarsen@Mac-Book-Pro2 neo4j-cluster % ansible -i inventory all -m ping
node2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
node3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
node1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}



klarsen@Mac-Book-Pro2 neo4j-cluster % ansible-playbook -i inventory build.yml

PLAY [Build Play] ****************************************************************

TASK [ansible-java17-oracle : Install add-apt-repostory] *************************
ok: [node1]
ok: [node2]
ok: [node3]

TASK [ansible-java17-oracle : Add Oracle Java Repository] **************************
changed: [node2]
changed: [node3]
changed: [node1]

TASK [ansible-java17-oracle : Update apt cache] *************************************
changed: [node1]
changed: [node2]
changed: [node3]

TASK [ansible-java17-oracle : Accept Java 17 License] *******************************
changed: [node2]
changed: [node3]
changed: [node1]

TASK [ansible-java17-oracle : Install Oracle Java 17 and packages | Ubuntu] *********
changed: [node2]
changed: [node1]
changed: [node3]

TASK [ansible-neo4j : Install neo4j tasks] *******************************************
included: /projects/neo4j-cluster/roles/ansible-neo4j/tasks/install_neo4j.yml for node1, node2, node3

TASK [ansible-neo4j : Install neo4j dependencies] ************************************
changed: [node2]
changed: [node3]
changed: [node1]

TASK [ansible-neo4j : Add Neo4j apt key] **********************************************
changed: [node2]
changed: [node1]
changed: [node3]

TASK [ansible-neo4j : Set Neo4j repository] ********************************************
changed: [node3]
changed: [node1]
changed: [node2]

TASK [ansible-neo4j : Set Neo4j universe repository] ***********************************
changed: [node1]
changed: [node3]
changed: [node2]

TASK [ansible-neo4j : Accept License neo4j-enterprise] **********************************
changed: [node1]
changed: [node2]
changed: [node3]

TASK [ansible-neo4j : Install neo4j package] *********************************************
changed: [node3]
changed: [node1]
changed: [node2]

TASK [ansible-neo4j : Configure Neo4j] *****************************************************
changed: [node2]
changed: [node1]
changed: [node3]

TASK [ansible-neo4j : Set open fd soft limit] ***********************************************
changed: [node1]
changed: [node2]
changed: [node3]

TASK [ansible-neo4j : Set open fd hard limit] ***********************************************
changed: [node1]
changed: [node2]
changed: [node3]

TASK [ansible-neo4j : Ensure Neo4j is enabled and started] **********************************
changed: [node2]
changed: [node1]
changed: [node3]

RUNNING HANDLER [ansible-neo4j : restart neo4j] **********************************************
changed: [node2]
changed: [node1]
changed: [node3]

PLAY RECAP ********************************************************************************************************
node1                      : ok=17   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node2                      : ok=17   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node3                      : ok=17   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0



Open a browser and go to:
http://192.168.3.11:7474



klarsen@Mac-Book-Pro2 neo4j-cluster % vagrant destroy -f
==> node3: Forcing shutdown of VM...
==> node3: Destroying VM and associated drives...
==> node2: Forcing shutdown of VM...
==> node2: Destroying VM and associated drives...
==> node1: Forcing shutdown of VM...
==> node1: Destroying VM and associated drives...
```


### Neo4j-Cluster dashboard

<img src="screenshots/Screenshot%202023-04-14%20at%201.09.43%20PM.png width="653" height="420" />