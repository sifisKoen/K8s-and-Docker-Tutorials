NETWORK NAMESPACES
===================


# Create network namespaces

```
ip netns add red
ip netns add blue
```

# Create veth pairs

```
ip link add veth-red type veth peer name veth-blue
```


# Create Add veth to respective namespaces

```
ip link set veth-red netns red
ip link set veth-blue netns blue
```

# Set IP Addresses

```
ip -n red addr add 192.168.1.1 dev veth-red
ip -n blue addr add 192.168.1.2 dev veth-blue
```

# Check IP Addresses

```
ip -n red addr
ip -n blue addr
```

# Bring up interfaces

```
ip -n red link set veth-red up
ip -n blue link set veth-blue up
```

# Bring Loopback devices up

```
ip -n red link set lo up
ip -n blue link set lo up
```

# Add default gateway

```
ip netns exec red ip route add default via 192.168.1.1 dev veth-red
ip netns exec blue ip route add default via 192.168.1.2 devip -n blue link del veth-blue
```

```
ip netns del red
ip netns del blue
```

```
ip link del v-net-0
```

```
iptables -t nat -D POSTROUTING 1
```

#

```
ip netns add red
ip netns add blue
```

```
ip link add veth-red type veth peer name veth-red-br
ip link add veth-blue type veth peer name veth-blue-br
```

```
ip link set veth-red netns red
ip link set veth-blue netns blue
```

```
ip -n red addr add 192.168.15.2/24 dev veth-red
ip -n blue addr add 192.168.15.3/24 dev veth-blue
```

```
brctl addbr v-net-0
```

```
ip link set dev v-net-0 up
ip link set veth-red-br up
```
