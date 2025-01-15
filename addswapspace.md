oracle@hostname client]$ ./runInstaller -silent -responseFile /u03/app/oracle/client19c/client/response/client_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 415 MB.   Actual 2416 MB    Passed
Checking swap space: 0 MB available, 150 MB required.    Failed <<<<

Some requirement checks failed. You must fulfill these requirements before

continuing with the installation,


Exiting Oracle Universal Installer, log for this session can be found at /tmp/OraInstall2025-01-08_05-30-18AM/installActions2025-01-08_05-30-18AM.log

2. Check the current total/used/available swap space by below command.

[root@hostname ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:          15802         139       12501           0        3161       15398
Swap:             0           0           0

3. Use the dd command to create a swap file of the required size. We need. 150 MB for Oracle software installation; however, I’m giving 500 MB because, at the time of Oracle Database installation, we required 450 MB swap space.

[root@hostname ~]# dd if=/dev/zero of=/swapfile count=500 bs=1MiB
500+0 records in
500+0 records out
524288000 bytes (524 MB) copied, 0.202619 s, 2.6 GB/s

4. Check the swap file existing permission and provide the appropriate one.

[root@hostname ~]# chmod 600 /swapfile

5. Format the Swap file.

[root@hostname ~]# mkswap /swapfile
Setting up swapspace version 1, size = 500 MiB (524283904 bytes)
no label, UUID=b61dc323-e7d3-4480-bb3b-c48cf07a3002

6. Activate Swap file for usage.

[root@hostname ~]# swapon /swapfile

7. Check the revised size.

[root@hostname ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:          15802         139       12000           0        3662       15397
Swap:           499           0         499


8. To make this permanent, update below entry on /etc/fstab and save it.

/swapfile swap swap defaults 0 0

We have successfully increased swap size and made it permanent!
