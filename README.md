Network as Code Demo
====================

1. Open Powershell terminal

     git clone https://github.com/CumulusNetworks/cldemo-vagrant
    cd cldemo-vagrant
    vagrant up oob-mgmt-server oob-mgmt-switch spine01 spine02 leaf01 leaf02 server01 server02

2. Get the local port of the oob-mgmt-server

    vagrant port oob-mgmt-server

3. Using Putty/SecureCRT/Terminal ssh to oob-mgmt-server (open at least 4 windows)

4. On the oob-mgmt-server:

    git clone https://github.com/CumulusNetworks/cldemo-config-routing
    cd cldemo-config-routing

5. From the oob-mgmt-server, ssh to:

 * leaf01
 * leaf02
 * server01

6. On leaf01/02, validate no routing tables or OSPF adjacencies:

    net show route
    net show ospf neigh

7. On server01, validate no reachability to server02

    ping 172.16.2.101

8. Push an OSPF numbered configuration to the network (from the OOB mgmt server):

    ansible-playbook deploy-ospf-numbered.yml

9. On leaf01/02, validate routing tables, OSPF adjacencies, and configuration:

    net show route
    net show ospf neigh 
    net show conf

10. On server01, validate reachability to server02

    ping 172.16.2.101

Reconfigure the Network to use BGP
----------------------------------

(keep a window open with a continous ping from server01 to server02 to observe reconvergence)

11. Push a BGP numbered configuration to the network (from the OOB mgmt server):

    ansible-playbook deploy-bgp-numbered.yml -l network

12. On leaf01/02, validate routing tables, BGP neighbors, and configuration

    net show route
    net show bgp summ
    net show conf
