# Clustering with Pacemaker

This is a compacted version of [Red Hat's guide to setting up a Linux cluster using Pacemaker](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_high_availability_clusters/index).

I have enriched it with from for my lab using VirtualBox.

<br/>

## Network configuration in Virtualbox

Add a new NAT network in Virtualbox.

1. File &rarr; Tools &rarr; Network Manager &rarr; Click the **NAT Networks** tab
2. Click **Create**: Assign a name and set the IPv4 Prefix to: `10.0.2.0/24`
3. Assign the network to each RHEL node: Machine settings &rarr; Network &rarr; Adapter 1: \
    Attached to: `NAT Network` \
    Name: `<select your newly created network>`

<br/>

## Network configuration on each RHEL node

I found it intuitive to use the terminal ui tool provided for configuring the IP address while using the cli tool to restart the network device after to apply the configuration.

1. Configure static IP with "nmtui":
    ~~~bash
    nmtui
    ~~~
2. Edit a connection &rarr; Select `enp0s3` (your network device name) &rarr; `Edit`: \
    IPv4 Configuration: `Manual` \
    Addresses: \
    On node1 set `10.0.2.81/24` \
    On node2 set `10.0.2.82/24` \
    Gateway: `10.0.2.1` \
    DNS Servers: `<set your home/work dns address>`

*Specify the network mask in the addresses field, otherwise it will be set to something else automatically*

3. Apply configuration after configuration using nmcli;
    ~~~bash
    nmcli d reapply enp0s3
    ~~~

Source: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/getting_started_with_networkmanager

<br/>

## Hostname configuration on each RHEL node

While we could have configured the hostname while we where setting up the IPs in the nmtui utility I used hostnamectl out of old habit.

node1:
~~~bash
hostnamectl z1.example.com
~~~

node2:
~~~bash
hostnamectl z2.example.com
~~~

<br/>

## Install packages on each RHEL node

First we need [enable the High Availability channel](https://access.redhat.com/solutions/265523) so that we can install the packages using yum.

It can be done either using **subscription-manager** or **yum**.

List available channels:

~~~bash
yum repolist all
subscription-manager repos --list
~~~

We will use yum:

~~~bash
yum install -y yum-utils
yum-config-manager --enable rhel-9-for-x86_64-highavailability-rpms
~~~

~~~bash
dnf install pcs pacemaker fence-agents-all
~~~

Start and enable automatic start of the cluster service:

~~~bash
systemctl start pcsd.service
systemctl enable pcsd.service
~~~

Firewall configuration:

~~~bash
firewall-cmd --permanent --add-service=high-availability
firewall-cmd --reload
~~~

<br/>

## Snapshot in VirtualBox

I decided it was a good idea to take a snapshot of the nodes before proceeding with the configuration. Just in case we need to go back, because the installation process of each RHEL node was really long compared to other distros.

1. Shutdown both nodes
2. On each node: Machine &rarr; Tools &rarr; Snapshots &rarr; **Take**

*Don't forget to delete the snapshot when done with the lab. It consumes disk space.*

<br/>

## User configuration on each RHEL node

Verify that the hacluster user has been created:

~~~bash
id hacluster
~~~

Change the password for the haclsuter user:

~~~bash
passwd hacluster
~~~

<br/>

## Edit hosts file on each RHEL node

We do this to allow the nodes to be able to lookup each other since we don't have a DNS system in place.

We need to add `z1.example.com` and `z2.example.com` on both nodes because otherwise the ipv6 address gets precedence when starting the cluster. It will then fail to start because it will resolve to the IPv6 address and this causes a mismatch.

Open the following in vim:

~~~bash
/etc/hosts
~~~

Append these lines to add the hosts:
~~~
10.0.2.81    z1.example.com
10.0.2.82    z2.example.com
~~~

<br/>

## Configure the cluster

On each node:

~~~bash
pcs host auth z1.example.com z2.example.com
~~~

You will be prompted for a user. Enter `hacluster` and the password you set in one of the earlier steps.

Cluster setup is only required to be run on one of the nodes:

~~~bash
pcs cluster setup my_cluster --start z1.example.com z2.example.com
pcs cluster status
~~~

Disable fencing for this lab:

~~~bash
pcs property set stonith-enabled=false
~~~

*Fencing is required even in a virtual environment. For example it makes sure a service is started properly on any available node in case of failure on the node it's running on.*

*Note regarding resources that will be managed by the cluster: \
Do not use "systemctl enable" to enable any services that will be managed by the cluster to start at system boot.*

<br/>

## Install the apache service on each RHEL node

~~~bash
dnf install -y httpd wget
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
~~~

~~~bash
cat <<-END >/var/www/html/index.html
<html>
<body>My Test Site - $(hostname)</body>
</html>
END
~~~

We want to allow from any IP becase we want to browse our service from our machine that is running VirtualBox.

~~~bash
cat <<-END > /etc/httpd/conf.d/status.conf
<Location /server-status>
SetHandler server-status
Order deny,allow
Allow from 0.0.0.0
</Location>
END
~~~


To list all cluster resource types available:

~~~bash
pcs resource list
~~~

To view a specific cluster  resouce:

~~~bash
pcs resource describe apache
~~~

<br/>

## Configure the cluster resources

Create the **IP resource** and the **apache (httpd) resource**. They are grouped 
together upon creation as `apachegroup` so that the cluster will make sure they are kept together. `10.0.2.80` is the floating IP.

Only required to be run on one of the nodes in the cluster:

~~~bash
pcs resource create ClusterIP ocf:heartbeat:IPaddr2 ip=10.0.2.80 --group apachegroup
~~~

*The IP is attached automatically to the network device of the active cluster node.*

Only required to be run on one of the nodes in the cluster:

~~~bash
pcs resource create WebSite ocf:heartbeat:apache configfile=/etc/httpd/conf/httpd.conf statusurl="http://localhost/server-status" --group apachegroup
~~~

Verify that the cluster resources are up:

~~~bash
pcs status
~~~

Display options for the resource:

~~~bash
pcs resource config WebSite
~~~

If you make changes to the apache service we need to restart it. Remember we should not manage the service using systemctl since it's managed by the cluster.

~~~bash
pcs resource restart WebSite
~~~

Simulate apache service crash:

~~~bash
killall -9 httpd
~~~

*Because we have disabled fencing the service will simply be restarted on the same node when we kill it.*

Cleanup error messages from the status view:

~~~bash
pcs resource cleanup WebSite
~~~

Force resources to be moved to the other node by disabling it on a node:

~~~bash
pcs node standby z1.example.com
~~~

Bring a disabled node online again. 

~~~bash
pcs node unstandby z1.example.com
~~~

*This will not move the service back to the previous node.*

Stop the cluster service on all nodes:
pcs cluster stop --all


<br/>
<br/>

# Setting up a NFS cluster

Shared disk in virtualbox:

1. File -> Tools -> Virtual Media Manager \
    Create -> VDI -> Pre-allocate (required for shareable) \
    Type: Shareable
2. Add the disk to each node in settings for each node.
