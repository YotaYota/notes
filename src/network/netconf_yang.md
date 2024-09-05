# NETCONF YANG

- [NETCONF RFC 6241](https://datatracker.ietf.org/doc/html/rfc6241)
- [YANG RFC 7950](https://datatracker.ietf.org/doc/html/rfc7950)

## NETCONF

NETCONF is a protocol defined by IETF to install, manipulate and delete the configuration of network devices.

NETCONF operations are realized on top of a Remote Procedure Call (RPC) layer using an XML encoding and provides a basic set of operations to edit and query configuration on a network device.

NETCONF allows network operators to lock and unlock data stores for better control over device configuration with RPC6241.

### Operations

- `<get>`
- `<get-config>`
- `<edit-config>`

### Datastore

- Running [REQUIRED]
- Candidate
- Start-up

## YANG

YANG is a data modelling lanuguage used to model configuration data, state data RPCs, and notifications for network management protocols.

YANG was originally designed to model configuration and state data manipulated by the Network Configuration Protocol (NETCONF), NETCONF RPCs, and NETCONF notifications.

YANG structures data models into modules and submodules. YANG defines four main types of data nodes for data modeling:

- Leaf nodes
- Leaf-List Nodes
- Container Nodes
- List Nodes


When a node is tagged with "config false", its subhierarchy is flagged as state data.  If it is tagged with "config true", its subhierarchy is flagged as configuration data.
