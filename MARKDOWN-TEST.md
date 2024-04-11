# Go live steps
## Gather Facts (required before installation)
1. Get IPs for Registry, Master nodes, Database nodes.
2. Get Floating (reserved) IP for Keepalived.
3. Get Floating (reserved) for floating DNS IP.
4. Get the DNS record for floating IP. Needed for embedded the internal routing mapped to the clients DNS record (specifically required for keycloak).
## Fetch Artifacts
`Internet Laptop`

These steps should be executed on the internet connected machine.


1. Fetch infrastructure artifacts.
2. Fetch cluster artifacts.

## Copy artifacts
`Internet Laptop`

On the internet machine, copy all artifacts to the repo node. The rest of the playbooks will be executed on the repo.
Important to note that a passwordless sudo user is required for the synchronize module of ansible to work. Run playbooks/nodes/makes-passwordless-sudo.yaml to set this for the connection user.


1. Copy infrastructure artifacts.
2. Copy cluster artifacts.

<style>
.my-class {
    color: red;
}
</style>
<p class="my-class">fsdf</p>

## Install Infrastructure
`Control Node`
<b class="my-class">fsdf</b>

Infrastructure Software:
* Azul Zulu (Java)
* Nexus
* Gogs
* RKE2

Context from where to execute playbooks:
* If `Internet Machine` with Linux, execute from wherever the Ansible project was copied to on that machine.
* If using Ansible on the `Registry None`, execute the playbooks from `/var/tmp/artifacts/ansible-infra` (directory where project would have been copied to):

1. **[Optional: If using Ansible Control from registry]** Copy SSH private key to repo machine, matching the public keys installed onto the other machines. Adjust the following in the .ssh/config for the repo machine.
```
Host 192.168.103.6?
    User support
    IdentityFile ~/.ssh/<priv_key>
```

2. Configure an ansible vault and password file:
   1. In the user home, create a password file (file with password in it).
   2. Create a vault with that password file, also stored in the user home.
3. Copy the site config.yaml to the user Home and adjust the config to match the network setup. Specifically update the number of hosts, IPs, client hostname, ssh key, etc. (everything that’s not in the artifacts section).
4. Install registry software
   1. playbooks/repo/install.yaml
   2. Configure Nexus:
      1. Create Helm repo
      2. Create Docker repo
   3. Configure Gogs


	  

	

      3. Keep all the settings the same, those are populated through Ansible.
      4. Create an Admin user, use mmsadmin for the Admin username (admin is reserved). 
      5. Create an org: mms and create the repository onprem-deployment (should match the org and repo name from the config.yaml).
      6. Restore template repository for site onto Gogs.
         1. Configure SSH config to Gogs (update IPs and IdentityFile):


Host 173.0.2.96
    User gogs
    HostName 173.0.2.96
    IdentityFile ~/.ssh/gogs-poc
    Preferredauthentications publickey

      7. Update repo with appropriate configuration for client.
Update Argo onprem-deployment git project
* Update MetalLB IP to the assigned IP for the domain.
Install Cluster Applications
1. Copy cluster artifacts
