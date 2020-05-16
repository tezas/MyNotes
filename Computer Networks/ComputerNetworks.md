> PROTOCOL

* Standards that computers must follow in order to commiunicate properly.

> TCP/IP Five Layer Network Model

1. PHYSICAL LAYER: Represents physical devices that interconnects computers.
2. DATE LINK LAYER: 
3. Common way of interpreting these signals so devices on a network can communicate. It provides abstraction over physical layer.
   * Media Access Control (MAC) : Controls devices integration.
   * Logical Link Layer: Deals with addressing and Multiplexing. 
4. Network Layer (Internet Layer) : Responsible for transferring data across the collection of network. (IP : Internet Protocol).
5. Transport Layer: Deliver data to appropriate application process on host.
6. Application layer


### Data link layer (Frames)

* It works inside a local network.
* Hop to Hop delivery
* Flow control : Stop and wait | Go Back N. It is used so that capacity of a machine can be used wisely.
* Error control: To detect if any of the bits have been changed. In this layer hop-hop error has bbeen checked.
* Access Control: CSMACD. Collision detection.
* At data link layer, connection uses physical address.

### Network Layer (Packets)

* Source to Destination delivery (Machine to Machine delivery).
* It works with IP address.
* Routing : Decides the route for sending the message over a network using different routing algorithm.
* Fragmentation: If the message size is too large then intermediate node wouldn't be able to handle large message due its low capacity. So router breaks the message in multiple fragments.
* Congestion control.

### Transport layer

