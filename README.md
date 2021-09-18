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

## Example:
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 root@192.168.56.18

You also need to install Python on slackware:
download python on the host (since i don't wanted to have internet access) and extract it:
<code>
wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
tar -xf Python-3.7.2.tar.xz
</code>

Upload the python file to the slackware machine:
<code>
scp -r -oKexAlgorithms=+diffie-hellman-group1-sha1 ./Python-3.7.2 root@192.168.56.17:/root
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
Add the ip address to /etc/hosts like so:
192.168.56.33 vmwarebase

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

It also generates a test principals (Test:Test1234567)
## Obtain a ticket from client

Have this installed:
sudo apt-get install krb5-user

Configure /etc/krb5.conf

kinit PRINCIPALNAME

Then use the password from the creation

with klist you can see the credentials

Remove the ticket with kdestroy

Renew the ticket with kinit PRINCIPALNAME -R


# ToDo:
* use principal for ssh authentication

with help from: https://subscription.packtpub.com/book/networking-and-servers/9781904811329/1/ch01lvl1sec09/installing-linux-pam