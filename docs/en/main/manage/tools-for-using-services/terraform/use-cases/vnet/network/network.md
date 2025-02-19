<warn>

First of all, make sure you [installed Terraform and created a main.tf file](../../../quick-start) with the required providers.

</warn>

To create a network or security group, create a `network.tf` file, which will describe the configuration of the created entities. Add the text from the examples below, and correct the settings for your networks and security groups.

1. To create a network and security groups, you will need the following objects:

    - Resources:

       - **vkcs_networking_network** — the network to which changes will be made.
       - **vkcs_networking_subnet** - subnet from the network. In the example: subnetwork.
       - **vkcs_networking_router** — a router for an external network and interaction with the outside world. In the example: router.
       - **vkcs_networking_router_interface** — connect the router to the internal network.
       - **vkcs_networking_secgroup** — security group to which access rules will be included.
       - **vkcs_networking_secgroup_rule** — rule for a security group. In the example, we open access to the network from any IP on ports 22 and 3389.
       - **vkcs_networking_port** — create a network port resource inside VK Cloud.
       - **vkcs_networking_port_secgroup_associate** — bind a port to a security group.

    - Data sources:

       - **vkcs_networking_network** — external network for obtaining public IP (Floating IP).

    ```hcl
    data "vkcs_networking_network" "extnet" {
       name="extnet"
    }

    resource "vkcs_networking_network" "network" {
       name="net"
    }

    resource "vkcs_networking_subnet" "subnetwork" {
       name="subnet_1"
       network_id = vkcs_networking_network.network.id
       cidr="192.168.199.0/24"
    }

    resource "vkcs_networking_router" "router" {
       name="router"
       admin_state_up = true
       external_network_id = data.vkcs_networking_network.extnet.id
    }

    resource "vkcs_networking_router_interface" "db" {
       router_id = vkcs_networking_router.router.id
       subnet_id = vkcs_networking_subnet.subnetwork.id
    }


    resource "vkcs_networking_secgroup" "secgroup" {
       name="security_group"
       description = "terraform security group"
    }

    resource "vkcs_networking_secgroup_rule" "secgroup_rule_1" {
       direction = "ingress"
       ethertype="IPv4"
       port_range_max = 22
       port_range_min = 22
       protocol="tcp"
       remote_ip_prefix = "0.0.0.0/0"
       security_group_id = vkcs_networking_secgroup.secgroup.id
       description = "secgroup_rule_1"
    }

    resource "vkcs_networking_secgroup_rule" "secgroup_rule_2" {
       direction = "ingress"
       ethertype="IPv4"
       port_range_max = 3389
       port_range_min = 3389
       remote_ip_prefix = "0.0.0.0/0"
       protocol="tcp"
       security_group_id = vkcs_networking_secgroup.secgroup.id
    }

    resource "vkcs_networking_port" "port" {
       name="port_1"
       admin_state_up = "true"
       network_id = vkcs_networking_network.network.id

       fixed_ip {
       subnet_id = vkcs_networking_subnet.subnetwork.id
       ip_address = "192.168.199.23"
       }
    }

    resource "vkcs_networking_port_secgroup_associate" "port" {
       port_id = vkcs_networking_port.port.id
       enforce = "false"
       security_group_ids = [
       vkcs_networking_secgroup.secgroup.id,
       ]
    }
    ```

1. Add an example to the `network.tf` file and run the following commands:

   ```bash
   terraform init
   ```
   ```bash
   terraform apply
   ```
