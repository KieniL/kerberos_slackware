# kerberos_slackware

Ansible playbook to install kerberos and a service on a host based on slackware (educational purposes)

I increased the memory of slackware to 512 MB

You must have this installed on the host:
* ansible
* sshpass
* tar
* xz

For the ssh connection an additional parameter is needed to negotiate the connection:
-oKexAlgorithms=+diffie-hellman-group1-sha1

Add the ip address to /etc/hosts like so:
192.168.56.33 vmwarebase.local

## Example:
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 root@vmwarebase.local

You also need to install Python on slackware:
download python on the host (since i don't wanted to have internet access) and extract it:
<code>
wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
tar -xf Python-3.7.2.tar.xz
</code>

Upload the python file to the slackware machine:
<code>
scp -r -oKexAlgorithms=+diffie-hellman-group1-sha1 ./Python-3.7.2 root@vmwarebase.local:/root
</code>

Compile Python in the directory you uploaded it into (/root/Python-3.7.2)
<code>
./configure
make
make install
</code>

# Structure of the inventory
there is one host which contains both roles
# Usage


ansible-playbook -i inventory main.yml

to only run the playbooks against a specific hostgroup:
ansible-playbook -i inventory --limit ssh main.yml
ansible-playbook -i inventory --limit kerberos main.yml

-K means the sudo password on the control machine
# Kerberos
An ansible galaxy role was generated to handle installation and configuration


The steps taken are from: https://karellen.blogspot.com/2014/03/mit-kerberos-for-slackware.html

It installs haveged and rng-tools to have more entropy for randomize data in kerberos

You can see the entropy with this command:
cat /proc/sys/kernel/random/entropy_avail

It also generates a test principals (Lukas:Test1234567) --> set in inventory
## Obtain a ticket from client

Have this installed:
sudo apt-get install krb5-user

Configure /etc/krb5.conf

--> Add default_realm under [libdefaults]
default_realm = VMWAREBASE

--> Add this to [realms]
VMWAREBASE = {
                 kdc = vmwarebase.local:88
                 admin_server = vmwarebase.local:749
        }

--> Add this to [domain_realms]
.vmwarebase = VMWAREBASE
vmwarebase = VMWAREBASE

With this command you can request a ticket with a lifetime of 1 minute
kinit -l 1m PRINCIPALNAME

Then use the password from the creation

with klist you can see the credentials

Remove the ticket with kdestroy

Renew the ticket with kinit PRINCIPALNAME -R


Then you can ssh into the machine with the ticket provided:
ssh -k vmwarebase


# Whats the process?

A kerberos services is hosted in the network which exposes port 88
The clients connects to port 88 on the server to request the ticket
After the client authenticated itself the services which are integrated with kerberos can be used with a new authentication provided.


# Create a new user
addprinc in kadmin.local
adduser in bash

Now kinit and then you can ssh


You don't need to add the user to keytab file (only the host is needed)