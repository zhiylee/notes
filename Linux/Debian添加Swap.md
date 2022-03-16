# Debian添加Swap

[](https://linuxize.com/post/how-to-add-swap-space-on-debian-10/)

**Before You Begin**

Although possible, it is not common to have multiple swap spaces on a single machine. To check whether your Debian installation already has swap enabled, run the following command:

```
sudo swapon --showCopy
```

If the output is empty, it means that the system doesn’t have swap space.

Otherwise, if you get something like below, you already have swap enabled on your Debian system.

```
NAME      TYPE      SIZE USED PRIO
/dev/sda2 partition   4G   0B   -1
Copy
```

To activate swap, the user running the commands must have [sudo privileges](https://linuxize.com/post/how-to-create-a-sudo-user-on-debian/) .

## **Creating a Swap File**

In this example, we will create and activate `1G` of swap. To create a bigger swap, replace `1G` with the size of the desired swap space.

The steps below show how to add swap space on Debian 10.

1. First create a file which will be used for swap:
   
    ```
    sudo fallocate -l 1G /swapfileCopy
    ```
    
    If `fallocate` is not installed or you get an error message saying `fallocate failed: Operation not supported` you can use the following command to create the swap file:
    
    ```
    sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576Copy
    ```
    
2. Only the root user should be able to read and write to the swap file. Issue the command below to set the correct [permissions](https://linuxize.com/post/chmod-command-in-linux/) :
   
    ```
    sudo chmod 600 /swapfileCopy
    ```
    
3. Use the `mkswap` tool to set up a Linux swap area on the file:
   
    ```
    sudo mkswap /swapfileCopy
    ```
    
4. Activate the swap file:
   
    ```
    sudo swapon /swapfileCopy
    ```
    
    To make the change permanent open the `/etc/fstab` file:
    
    ```
    sudo nano /etc/fstabCopy
    ```
    
    and paste the following line:
    
    /etc/fstab
    
    `/swapfile swap swap defaults 0 0`Copy
    
5. Verify whether the swap is active using either the `swapon` or `[free](https://linuxize.com/post/free-command-in-linux/)` command as shown below:
   
    ```
    sudo swapon --showCopy
    ```
    
    ```
    NAME      TYPE  SIZE   USED PRIO
    /swapfile file 1024M 507.4M   -1Copy
    ```
    
    ```
    sudo free -hCopy
    ```
    
    ```
                  total        used        free      shared  buff/cache   available
    Mem:           488M        158M         83M        2.3M        246M        217M
    Swap:          1.0G        506M        517M
    ```