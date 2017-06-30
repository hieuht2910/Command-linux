*****************************************************************************
  Mount
*****************************************************************************


$mount  -t auto /dev/device  /mnt/mount/point


#mount a hard disk, the type can be ntfs , vfat, ext3, reiserfs...
$mount  -t type /dev/sda[?]  /mnt/mount/point


#mount a cd/dvd, the type can be iso9660, udf
$mount  -t type /dev/cdroom  /mnt/mount/point


#mount samba network
#Creat a mount point


mkdir mnt
mkdir mnt/netwok
mkdir mnt/network/eXoServer
mkdir mnt/network/eXoServer/eXo


mount -t smbfs //192.168.2.10/eXo  /mnt/network/eXoServer/eXo  -o rw,username=username,password=password


Frequently used types, these are delineated by the -t flag on the mount command


  * auto is for user-mounted floppies, allowing ext2, ext3, and vfat (DOS/Windows)
  * iso9660 is used for cdroms and is the default type
  * udf is used for DVD and is the default type
  * vfat is window fat 32  file system
  * ntfs is window  NT/XP  file system
  * ext3 is the default  linux  file system
  * reiserfs is a new and advaced  file system of linux
  * nfs is for files that are mounted over the network


Options used in examples, these are delineated by the -o flag on the mount command


  * noauto   means that the file system is not automatically mounted at boot time,
             but may be mounted later using   mount -a <path>
  * auto     causes a file system to be mounted at boot time
  * default  options are rw, suid, dev, exec, auto, nouser, and async
  * ro       allows the file system to be mounted read-only
  * rw       allows read-write


Filesystem-specific options. The following are used with the smbfs file type
 
  mount -t smbfs //server/path  /mount/dir  -o rw,username=username,password=password


configure /etc/fstab


dir/devices         Mount Point        File Type        mount Options


/dev/fd0            /mnt/floppy          auto           noauto
/dev/hda            /mnt/cdrom           udf,iso9660    user,noauto      
/dev/zip             /mnt/zip            vfat           auto,exec


//server/path       /mnt/server/path     smbfs          rw,username=username,password=password
**********************************************************************************************************
IFCONFIG commands
**********************************************************************************************************
#set default gateway: route add default gw {IP-ADDRESS} {INTERFACE-NAME}
$route add default gw 192.168.1.254 eth0


**********************************************************************************************************
  SSH Command Family
**********************************************************************************************************
SH has a number of excellent security features beyond the basic encryption
of your password and login session as they pass over the net.  SSH can
provide a stronger encryption algorithm ("RSA") and it can allow X11 and
other network protocols to securely "tunnel" through your encrypted SSH
session as they pass over the net.


SSH commands include: 


sshd         Server program run on the server machine.  This
             listens for connections from client machines, and
             whenever it receives a connection, it performs
             authentication and starts serving the client.


#To start/stop sshd server
$/etc/init.d/ssh [start/stop/restart]




ssh          This is the client program used to log into another
             machine or to execute commands on the other machine.


  #To connect to a server
  $ssh username@server


  #To run a command on a remote computer
  $ssh username@server command


  #To connect to a server with a indentity file, the identity file is a private key that is
  #generated  by ssh-keygen command
  $ssh -i ~/.ssh/id_dsa username@server


scp          Securely copies files from one machine to another.


  #The general form is:
  $scp source destination


  #Copy from a  remote computer to the local computer
  $scp username@server:/path/file.txt destination/


  #Copy from a  local computer to the remote computer
  $scp file  username@server:/path/destination


  #Copy a directory from a  local computer to the remote computer
  $scp -r directory  username@server:/path/destination


  #Copy from a  local computer to the remote computer  using private key file
  $scp -i KeyFile file  username@server:/path/destination


ssh-keygen   Used to create RSA keys (host keys and user
             authentication keys).


  #Generate a  pair of key with dsa algorithm
  $ssh-keygen -t dsa -f FileName


  #Generate a  pair of key with rsa algorithm
  $ssh-keygen -t rsa -f FileName


  #To login using your private key, copy the public key to the remote computer
  #and append the public key to the file ~/.ssh/authorized_keys
  $scp FileName.pub username@remote.computer:~/
  $ssh username@remote.computer
  $cat FileName.pub >> ~/.ssh/authorized_keys


ssh-agent    Authentication agent.  This can be used to hold RSA
             keys for authentication.


ssh-add      Used to register new keys with the agent.


make-ssh-known-hosts
             Used to create the /etc/ssh_known_hosts file.


rsynch       Allow you to syncronize  the content of 2 directories


  #The general form is:
  $rsync -v src-dir destination/src-dir


  #tar and  compress, useful  when you synchronize  with a remote computer
  #If you have a large  number of files and directories ,  you should turn off -v option
  #the synchronization will run much faster
  $rsync -avz src-dir user@remote.computer:/path/destination/src-dir


  #Deleting the files  that  are not existed on the src-dir
  $rsync -avz -delete src-dir user@remote.computer:/path/destination/src-dir


  #Excluding the some file pattern on the src-dir
  $rsync -avz --exclude "target" --exclude ".svn" src-dir user@remote.computer:/path/destination/src-dir


  #Including only the file pattern on the src-dir
  $rsync -avz --include "*.java" --include "*.properties" src-dir user@remote.computer:/path/destination/src-dir


  Option meanings:


    * -v verbose, show the detail messages
    * -q quiet, show only the error messages
    * -a archive the directory
    * -r recurse into subdirectories
    * -l copy symlinks as symlinks
    * -p retain file permissions
    * -t retain file time stamps
    * -g retain group ownership
    * -o retain owner


**********************************************************************************************************
Mysql commands
**********************************************************************************************************
#Check  mysql server status start,  stop and restart  the  mysql server (You need to be the root user)
$/etc/init.d/mysql [status, start, stop, restart]


#Check to see if the server alive
$mysqladmin --host=hostname/IP --port=3306 --user=root --password=root ping


#To create a new database  instance
$mysqladmin --host=hostname/IP --port=3306 --user=root --password=root create dbname


#To drop a database  instance
$mysqladmin --host=hostname/IP --port=3306 --user=root --password=root -f drop dbname


#To change the admin password
$mysql -u root -poldpassword <<< "GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'root'"


#To create a new user and assign the permissions to him
$mysql -u root -ppassword <<< "GRANT ALL PRIVILEGES ON *.* TO moom@localhost IDENTIFIED BY 'moom' WITH GRANT OPTION"
$mysql -u root -ppassword <<< "GRANT ALL PRIVILEGES ON *.* TO moom@'%' IDENTIFIED BY 'moom' WITH GRANT OPTION"


#To  Run a sql script file
$mysql -u username -ppassword  dbname < script.sql
#or
$mysql -u username -ppassword  dbname < script.sql >  output.txt




#To backup  database of a database instance
$mysqldump [--compress] -u username -ppassword --databases databasename > databasename.sql


#Restore database (or database table) from backup.
mysql -u username -ppassword databasename < databasename.sql


**********************************************************************************************************
SVN commands
**********************************************************************************************************


#To  fix the  svn objectweb server repository:
#ssh benjmestrallet@forge.objectweb.org
#Go to /svnroot/exoplatform/db
#and launch db4.2_recover


#To import a module  to the svn repository, use


  svn import -m "new module" module.dir svn+ssh://username@svn.forge.objectweb.org/svnroot/exoplatform/module.dir


#To list the available module:


  svn ls svn+ssh://username@svn.forge.objectweb.org/svnroot/exoplatform


#To remove a module:


  svn rm -m "message" svn+ssh://tuan08@svn.forge.objectweb.org/svnroot/exoplatform/module.dir


#To check out a module:


  svn co svn+ssh://username@svn.forge.objectweb.org/svnroot/exoplatform/module.dir


**********************************************************************************************************
hdparm commands, Use to test the read write performance of the hard disk
**********************************************************************************************************
#To check the read performance
/sbin/hdparm -Tt /dev/hdb


#To check the device information
/sbin/hdparm -v /dev/hdb


#To check the device information
/sbin/hdparm -I /dev/hdb


#To check hard disk for the bad sector
$fsck.ext4 -c /dev/name
**********************************************************************************************************
iperf command, Use to test the network performance
**********************************************************************************************************
#On one computer run the iperf in the server mode
iperf -s
#On the other computer run the iperf in the client mode
iperf -c hostname/ip


**********************************************************************************************************
Change Timezone
**********************************************************************************************************
dpkg-reconfigure tzdata
**********************************************************************************************************
Synchronize time
**********************************************************************************************************
ntpdate -u ntp.nasa.gov


**********************************************************************************************************
Clear DNS Cache
**********************************************************************************************************
#On Unix/Linux/Mac
$lookupd -flushcache


#On window
C:\>ipconfig /flushdns


**********************************************************************************************************
Install mintty
**********************************************************************************************************
#Run the cygwin setup.exe,  select the package  chere  and mintty
#Run the command:
$cd ~  && chere -i


#Create a shorcut to the mintty:  d:\tools\cygwin\bin\mintty.exe -e d:\tools\cygwin\bin\bash -c "/bin/xhere /bin/bash.exe '%L'"


**********************************************************************************************************
Install Git
**********************************************************************************************************
- TODO


**********************************************************************************************************
Git command
**********************************************************************************************************
- TODO