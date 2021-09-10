# kerberos_slackware

Ansible playbook to install kerberos on slackware (educational purposes)

You must have this installed on the host:
* ansible
* sshpass

For the ssh connection an additional parameter is needed to negotiate the connection:
-oKexAlgorithms=+diffie-hellman-group1-sha1

## Example:
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 root@192.168.56.18

You also need to install Python on slackware:
download python on the host (since i don't have internet access) and extract it:
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
there are two hosts (kerberos and ssh which can contain multiple servers)
both hosts are grouped to a slackware group to only have to set the variables once
# Usage
ansible-playbook -i inventory main.yml

to only run the playbooks against a specific hostgroup:
ansible-playbook -i inventory --limit ssh main.yml
ansible-playbook -i inventory --limit kerberos main.yml
# Kerberos
An ansible galaxy role was generated to handle installation and configuration


From https://karellen.blogspot.com/2014/03/mit-kerberos-for-slackware.html I found that the configure should have some parameters