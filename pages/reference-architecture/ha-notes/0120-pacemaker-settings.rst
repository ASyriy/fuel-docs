.. index:: Pacemaker Settings

Pacemaker Settings
------------------

Pacemaker is the cluster resource manager used by Fuel to manage Neutron
resources, HAProxy, virtual IP addresses and MySQL Galera cluster. It is done by
use of Open Cluster Framework (see http://linux-ha.org/wiki/OCF_Resource_Agents)
agent scripts which are deployed in order to start/stop/monitor Neutron services,
to manage HAProxy, virtual IP addresses and MySQL replication. These are located
at ``/usr/lib/ocf/resource.d/mirantis/neutron-agent-[ovs|dhcp|l3]``,
``/usr/lib/ocf/resource.d/mirantis/mysql``, ``/usr/lib/ocf/resource.d/ocf/haproxy``.
Firstly, MySQL agent is started, HAproxy and virtual IP addresses are set up.
Open vSwitch, metadata and L3 agents are started as Pacemaker clones on all
the nodes, then a single instance of the dhcp agent is started.

.. seealso:: `Using Rules to Determine Resource
   Location <http://clusterlabs.org/doc/en-US/Pacemaker/1.1/html/Pacemaker_Explained/_using_rules_to_determine_resource_location.html>`_

MySQL HA script primarily targets to the cluster rebuild after power failure or
equal type of disaster - it needs working Corosync in which it forms quorum of
an epochs of replication and then electing master from node with newest epoch.
Be aware of default five minute interval in which every cluster member should be
booted to participate in such election. Every node is a self-aware, that means
if nobody pushes higher epoch that it retrieved from Corosync (neither no one did),
it will just elect itself as a master.

