Linux provides many tools that allows you to check and modify your network configurations.

# IP Interface

The new command that is being used to manage the network is `ip`. My favorite thing about this command, is that it lets you abbreviate the commands. To print the current configuration, run

```
ip address show
```

or simply just run
```
ip addr show
ip addr
ip a
```

I always use `ip a`. It is fast, and gets the job done. You can also select a certain interface instead of printing all the interface.

```
ip a show <interface_name>
ip a sh eth0
```

You can also print the routing table.

```
ip route show
```

## Modify Interface

If you want to modify the interface, maybe add a new IP or remove the current.

```
ip addr add/delete <IP/sub> dev <interface_name>
ip addr add 192.168.100.100/24 dev eth0
ip addr delete 192.168.100.100/24 dev eth0
```

## Interface down, Interface up

You can switch off an interface, instead of deleting it, and switch on later. This change will not survive a reboot.

```
ip link set <interface> down
ip link set <interface> up
```

# Network Manager

The network manager is a tool helps us configure our network settings.

It provides three great tools:

1- nmcli - a cli tool with various options
2- nmtui - a text based interface with basic options but powerful
3- network-connection-editor - a graphical interface

## nmcli

The general syntax for nmcli is 

```
nmcli [OPTIONS] OBJECT [COMMAND] [ARGUMENTS]
# Objects are: help | general | networking | radio | connection | device | agent | monitor
```

Our focus in this document will be the `connection` object.

These are the commands that are available for the `connection` object.

```
nmcli connection {show | up | down | modify | add | edit | clone | delete | monitor | reload | load | import | export | migrate} [ARGUMENTS...]
```

To print the current connections

```
nmcli connections show
```

To print all the settings for a certain connection

```
nmcli connection show <interface>
nmcli connection show eth0
```

To change any of these settings, we will use the `modify` object. For example, the `ipv4.routes` is responsible for static routes. Let us configure a permanent static route. `+ipv4.routes` means we will add, `-ipv4.routes` means we will delete.

```
nmcli connection modify <interface> +ipv4.routes 'ip/sub gw'  

nmcli connection modify +ipv4.routes '192.168.100.100/24 192.168.100.1' 
nmcli device reapply eth0 # To reload the configuration
```

We can also configure a static IP for an interface
```
nmcli connection modify <interface> ipv4.addresses <ip/sub> ipv4.gateway <GW> ipv4.dns <DNS> ipv4.method manual
nmcli connection modify eth0 ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.dns 8.8.8.8 ipv4.method manual
```

## nmtui

If this is too complicated, you can alternatively use the command `nmtui`, and perform all of the above by using the text interface.

