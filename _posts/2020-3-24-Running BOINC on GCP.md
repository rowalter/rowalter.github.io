---
title: Running BOINC on free GCP instance
categories: Cloud
published: true
---
Spending compute ressources for calculation on various problems has recently become very popular again. The trigger was certainly the corona crisis.
Some colleagues at VMware have developed a Fling for this purpose, which is also becoming increasingly popular.

But not everyone has a Homelab with sufficient computing power available. Therefore I have been thinking about moving this workload into the cloud for quite some time. 

After a hint of a colleague to the entry offer of the Google Cloud Platform, I have prepared the "always free f1-micro" instance there for BOINC. Sure, a virtual CPU is not much, but sometimes it's the stamina and not the sprint that counts.

Here I want to show you how everyone can use the instance and control it from home.

First you have to create a Google Cloud Account, which is really not difficult and is explained e.g. in [this](https://medium.com/@hbmy289/how-to-set-up-a-free-micro-vps-on-google-cloud-platform-bddee893ac09) blog entry.

## Creating an instance with a client

Basically you only need to create a simple f1-micro instance. I chose an SDDC in Iowa and selected the smallest instance there. As OS I chose a current Debian with 10GB "non-volatile" memory. 

![](/images/2020-3-24-Running BOINC on GCP_1.png)

Then the instance must be created, which only takes a few seconds.

![](/images/2020-3-24-Running BOINC on GCP_2.png)

You can immediately connect to the VM via SSH. (Press the button SSH)

![](/images/2020-3-24-Running BOINC on GCP_3.png)

with
```
sudo apt-get install boinc-client
```
you can install the necessary packages for BOINC. The installation takes about 5 minutes and maybe you need to update the OS with apt-get upgrade.

When the installation is finished, a project can be attached via the command line. To do this, the appropriate key must first be read from the BOINC account. This is done with the command: 

```
boinccmd --loockup_account %project_URL% %User_eMail% %passwd%
```
so i used

```
boinccmd --loockup_account http://boinc.bakerlab.org %User_eMail% %passwd%
```
because the [Rosetta Projekt](http://boinc.bakerlab.org) is working on the COVID-19 problem.

With the account key which you get from the command you can add this "worker in the cloud" to your account.
```
boinccmd --project_attach http://boinc.bakerlab.org %account_key%
```
After you run this command the client will start downloading work units and start working on that.
But the access to the VM should be even more comfortable and of course it should also be possible to control it via the Boinc Manager on your own laptop.
The SSH access should be secured by a public/private key pair so that access via Putty or similar is also possible.
So you create a key pair using Puttygen, for example, and add correct key to the virtual machine using the "Edit" button (just scroll down).

![](/images/2020-3-24-Running BOINC on GCP_6.png)

After this you just need to create a new SSH session to the public IP adress of the instance and add the key file to it. When this is done, access is possible directly via Putty.

## Access to the client via Boinc Manager

Since the operation of the Boinc instance is quasi headless, an access of the local manager to this instance must be established. 
A firewall rule must be set up to allow RPC access to the corresponding port. Also the client must be configured in the VM to allow access (password protected).

You can configure the firewall rule through the network settings: (allow Port 31416)

![](/images/2020-3-24-Running BOINC on GCP_12.png)

Add this rule to the networking details of the VM:

![](/images/2020-3-24-Running BOINC on GCP_11.png)

To enable remote access to the Boinc Client in the VM, the corresponding configuration file **etc/boinc-client/cc_config.xml** needs to be edited. You need to add 
```
	<options>
          <allow_remote_gui_rpc>1</allow_remote_gui_rpc>
	</options>
```

The whole file should look like:

![](/images/2020-3-24-Running BOINC on GCP_8.png)

Also you need to add a password(string) to **/etc/boinc-client/gui_rpc_auth.cfg**

Finally, Boinc Manager can be used to connect to and control the actual client in the VM.

![](/images/2020-3-24-Running BOINC on GCP_9.png)

### Concluding words

After I initially connected to the client, the status was "Waiting for memory". This means that the memory of the VM is not sufficient to load the work package. The packages for the Rosetta project are about 350-370 MB in size. However, the available memory is only about 330 MB. For this reason I have added my other project [WorldCommunity Grid](https://www.worldcommunitygrid.org/). Most of the packages there are about 35MB in size, so they can easily be processed in the cloud. Maybe it helps to choose an OS other than Debian to have more memory of the f1-micro instance available for applications.

