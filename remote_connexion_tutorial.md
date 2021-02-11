# Mount remote server on your linux machine

You might want to access the ```/inhouse``` directory on your own machine from
any internet connection. An easy way to do it is using ssh connection.

You can actually use different remote hosts to access the ```/inhouse```
directory. If you are out of the esrf walls and want to connect to any of the
esrf hosts, you will first need to connect to the esrf firewall. This is why the
easiest way to make the connection to the ```/inhouse``` directory is by
connecting to ***firewall.esrf.fr***. Therefore, in the following, you can
replace ***remoteserver*** with ***firewall.esrf.fr***.

## First, configure your ssh connection
Each time you connect to a remote server you must enter your user password,
wich is not always convenient. Especially if you want to mount a remote 
directory but also if you plan on running scripts that need to connect several 
times to different remote servers.

In order to avoid entering the password when mouting the ```/inhouse```
directory, you may generate a key on your local machine. This key will somehow
replace you password so that you do not have to type it when accessing the
remote host.


Generate the key on your local machine :
```Bash
ssh-keygen -t rsa
```	
You will be asked to enter a passphrase, just press ```ENTER``` twice if you do
not need it.

Then you must configure the remote host so it may accept the connection from you
local machine thanks to the key you generated.

Make a .ssh directory in the ```~/``` on the remote server (the directory may
already exist).
```Bash
ssh user@remotehost mkdir -p .ssh
```
You need to enter your password.

Finally, add the key you generated on your local machine to
```user@remotehost:.ssh/authorized_keys```
```Bash
cat .ssh/id_rsa.pub | ssh user@remotehost 'cat >> .ssh/authorized_keys'
```
Enter your password one last time.


Finally, you can log into the remote host from your local machine without
password:
```Bash
ssh user@remotehost
```

## Second, install sshfs module
```sshfs``` module allows to mount directory using ssh connection.


Install it with the following command	:  
```Bash
sudo apt-get install sshfs
```

## Finally, mount the remote directory on your local machine

You need to make a new directory so that you can mount the remote directory in.
As you might want to mount several directories, you can create a ```~/mount```
directory that contains the remote directories.
```
mkdir ~/mount
```
Make the inhouse directory
```Bash
mkdir ~/mount/inhouse
```
Now, run the following command to mount the remote folder:
```Bash
sshfs user@remotehost:/data/id01/inhouse ~/mount/inhouse
```

No password shoud be required as you configure the ssh connection between your
local machine and the remote server.

To unmount the remote directory use the command:
```Bash
umount ~/mount/inhouse
```

## Tip: use alias
If you want to mount your remote directory easily and without remembering the
command, you can create an alias in the ```~/bash_aliases```. For instance, add
the following line:
```Bash
alias mountinhouse='sshfs user@remotehost:/data/id01/inhouse ~/mount/inhouse'
```
Then, the ```/inhouse``` directory will be mounted with the ```mountinhouse```
command from your terminal.
