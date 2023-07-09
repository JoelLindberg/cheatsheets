# Networking

## nmcli

`RHEL`, `Ubuntu`

### Configure static IP

Show connections and devices:

~~~bash
nmcli con
~~~

*Pay attention the UUID column.*

Edit a connection interactively by specifying the UUID of the connection you want to edit:

~~~bash
nmcli con edit <uuid>
~~~

*Tab complete of the uuid works.*

In the interactive mode:

Show all settings: `print` \
Show a specific setting: `print <setting name>` \
Change a setting: `set <setting name> <new value>`

Change from DHCP to static: `set ipv4.method manual` \
Set a static IP: `set ipv4.addresses 10.0.2.71/24` \
Set the gateway: `set ipv4.gateway 10.0.2.1`

Save the settings: `save` \
Activate the device to apply the changes: `activate` \
Quit: `quit`

Show the current configuration of the network devices to verify the changes:

~~~bash
nmcli -p
~~~


Source: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_ip_networking_with_nmcli

<br />

### Configure wifi and ethernet to be mutually exclusive

`Ubuntu`

This should work with RHEL as well, but the scripts might be in a [different place](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-using_networkmanager_with_network_scripts): `/etc/init.d/network`

The below is the steps taken to get this working on Ubuntu desktop:

1. Insert the below script in a file placed here (the name of the file is just an example): `/etc/NetworkManager/dispatcher.d/70-wifi-wired-exclusive.sh`

~~~bash
#!/bin/bash
export LC_ALL=C

enable_disable_wifi ()
{
    result=$(nmcli dev | grep "ethernet" | grep -w "connected")
    if [ -n "$result" ]; then
        nmcli radio wifi off
    else
        nmcli radio wifi on
    fi
}

if [ "$2" = "up" ]; then
    enable_disable_wifi
fi

if [ "$2" = "down" ]; then
    enable_disable_wifi
fi
~~~

2. Apply permissions to allow the file to be executed: `chmod 744 /etc/NetworkManager/dispatcher.d/70-wifi-wired-exclusive.sh`

3. Restart the network to see it in action: `nmcli n off && nmcli n on`

*This script will be run every time you plug/unplug you ethernet cable or connect/disconnect your wifi. It will be triggered on network events basically.*

Source: https://manpages.ubuntu.com/manpages/focal/man7/nmcli-examples.7.html