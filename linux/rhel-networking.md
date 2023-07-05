# Red Hat Networking

## nmcli

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
