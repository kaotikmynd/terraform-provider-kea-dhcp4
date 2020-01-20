# go-terraform-kea-dhcp4

Kea - Dhcp4 plugin for Terraform
=================================


This is a plugin for https://www.isc.org/kea/ DHCP daemon to update its configuration, 
mostly it can be used if you need to control DHCP leases for any kind of Terraform infrastructure as a code.



Installation:
------------

1. "git clone" this repo
2. "go build" inside of it
3. Copy binary file into .terraform.d/plugins/
4. "terraform init" to initialize this plugin


Configuration and Usage:
------------------------

1. The plugin has been written for connection to Kea API with basic auth, so please set up your kea REST API with basic authorisation,
    (it can be done if you have hidden after apache instance), its strongly recommended using https protocol to keep credentials safety.
2. To set credentials to Kea API you can use export variables:
```
    export TF_VAR_kea_user=User
    export TF_VAR_kea_pass=Password
```
3. Now to use kea provider put something like this in your .tf files:
```
    # define kea-dhcp4 provider
    provider "kea-dhcp4" {
        kea_server_address    = "https://your.kea.server/kea"
        kea_server_username   = "${var.kea_user}"
        kea_server_password   = "${var.kea_pass}"
        kea_server_configfile = "/etc/kea/kea-dhcp4.conf"
    }
    # create resource lease in dhcp conf.
    resource "kea-dhcp4_host_lease" "vm3" {
        name = "vm3"
        mac_address = "52:54:00:06:19:25"
        ip_address = "192.168.122.10"
    }
```

Notice: you need to have already defined subnets in your kea server, 
this plugin allows you to define host lease only for host defined in terraform any kind of infrastructure which use Kea DHCPd. 


author nutcracker - michal.lis.1987@gmail.com

GNU Public License see LICENSE file.