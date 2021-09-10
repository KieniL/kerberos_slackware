# kerberos_slackware

Ansible playbook to install kerberos on slackware (educational purposes)

You must have this installed on the host:
* ansible
* sshpass

For the ssh connection an additional parameter is needed to negotiate the connection:
-oKexAlgorithms=+diffie-hellman-group1-sha1

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


# Kerberos
An ansible galaxy role was generated to handle installation and configuration


