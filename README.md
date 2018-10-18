# Midori Coin

Shell script to install a `Midori Coin Masternode` on a Linux server running Ubuntu 16.04. 
This script will support installing 3 Midori masternodes on the same VPS.


## Installation

The installation script will do everything needed to setup your VPS with the latest updates and perform all steps required to install the Midori masternode without any interaction or programming knowledge. 

When you run the script it will tell you what it will do on your system. Once completed there is a summary of the information you need to be aware of regarding your node setup which you can copy/paste to your local PC. 
```
Make sure you keep the script output information safe for later.
```

You will need to run the script before setting up your node in your local wallet as it will generate a `private key` for you to use in your `masternode.conf` file.
To get started with the installation, login to your VPS as the root user and run the two lines below.

```
wget https://github.com/MidoriChainDev/MidoriChain/raw/master/install-midori.sh  
bash install-midori.sh
```

&nbsp;

## How to setup your masternode with this script and a cold wallet on your PC
```
Run the script first so you have the information when it finishes, to complete your cold wallet setup.
```
The script assumes you are running a cold wallet on your local PC and this script will execute on a Ubuntu Linux VPS (server). The steps involved are:

 1. Run the masternode installation script as per the [instructions above](https://github.com/MidoriChainDev/MidoriChain#installation).
 2. When you are finished this process you will get some information on what has been done as well as some important information you will need for your cold wallet setup.
 3. **Copy/paste the output of this script into a text file and keep it safe.**

You are now ready to configure your local wallet and finish the masternode setup

 1. Make sure you have downloaded the latest wallet from https://github.com/midorideveloper/MidoriCore/releases
 2. Install the wallet on your local PC
 3. Start the wallet and let if completely synchronize to the network - this will take some time
 4. Create a new `Receiving Address` from the wallets `File` menu and name it appropriately, e.g. MN-1
 5. Unlock your wallet and send **exactly 5,000 MDR** to the address created in step #4
 6. Wait for the transaction from step #5 to be fully confirmed. Look for a tick in the first column in your transactions tab
 7. Once confirmed, open your wallet console and type: `masternode outputs`
 8. Open your masternode configuration file from the wallets `Tools` menu item.
 9. In your masternodes.conf file add an entry that looks like: `[address-name from #4] [ip:port of your VPS from script output] [privkey from script output] [txid from from #7] [tx output index from #7]` - 
 10. Your masternodes.conf file entry should look like: `MN-1 127.0.0.2:48000 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0` and it must be all on one line in your masternodes config file
 11. Save and close your masternodes.conf file
 12. Close your wallet and restart
 13. Go to Masternodes
 14. Click the row for the masternode you just added
 15. Right click > Start Alias
 16. Your node should now be running successfully, wait for some time for it to connect with the network and the `Active` time to start counting up.

 &nbsp;

## Multiple master nodes on one server
The script allows for multiple nodes to be setup on the same server, using the same IP address. 

During the execution of the script a new user will be created to run each masternode. For stability of the Midori coin as well as keeping your VPS well resourced this script only supports installing up to 3 Midori masternodes on the same VPS. 

Once you have setup the 2nd and/or 3rd masternodes, use the output of the script for each masternode and follow the [steps above](https://github.com/MidoriChainDev/MidoriChain#how-to-setup-your-masternode-with-this-script-and-a-cold-wallet-on-your-pc) in your wallet, where each new masternode is a new line in your `masternode.conf` file. **NOTE:** All the port numbers in your masternode.conf file will be 48000 or the wallet will not re-start after you save the config file.

Note that multiple masternodes use only one instance of the `midorid` and `midori-cli` binary files located in `/usr/local/bin` and they each have their own configuration located in `/home/<username>/.midoricore` folder.

&nbsp;


## Masternode commands
Because the masternode runs under a user account, you cannot login as root to your server and run `midori-cli masternode status`, if you do you will get an error. You need to switch the to the user that you installed the masternode under when running the script. **So make sure you keep the output of the script after it runs.**

The masternode runs as a service, so if you do not stop the service and just try to stop the daemon process, it will restart again. You can start and stop the masternode when logged in as root or the user by running the following two commands:

#### To stop your masternode 
```
systemctl stop <username>.service
```

#### To start your masternode 
```
systemctl start <username>.service
```

#### To interactively switch from the root user to the masternode user, you can run:
```
 su - <username>
```
If you are asked for a password, it is in the script output you received when you installed the masternode, you can right click and paste the password. 
The following commands can then be run against the node that is running as the user you just switched to. Users for the Midori coin are created as `midori-mn1`, `midori-mn2` and `midori-mn3`.

Note: the commands below will only query the node that is running under the current user you just switched to. If you want to query another running node, you need to `exit` then switch to that user before running these commands.

#### To query your masternodes status
```
 midori-cli masternode status 
```

#### To query your masternode information
```
 midori-cli getinfo
```

#### To query your masternodes sync status
```
 midori-cli mnsync status
```

## Removing a masternode and user account
If something goes wrong with your installation or you want to remove a masternode, you can do so with the following command.
```
 userdel -r <username>
```
This will remove the user and its home directory. If you then re-run the installation script you can re-use that username.

&nbsp;

## Security
The script will set up the required firewall rules to only allow inbound node communications, whilst blocking all other inbound ports and all outbound ports.

The [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page) package is also used to mitigate DDoS attempts on your server.

Despite this script needing to run as `root` you should secure your Ubuntu server as normal with the following precautions:

 - disable password authentication
 - disable root login
 - enable SSH certificate login only

If the above precautions are taken you will need to `su root` before running the script. You can find a good tutorial for securing your VPS [here](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04).

&nbsp;

## Disclaimer
Whilst effort has been put into maintaining and testing this script, it will automatically modify settings on your Ubuntu server - use at your own risk. By downloading this script you are accepting all responsibility for any actions it performs on your server.

&nbsp;







