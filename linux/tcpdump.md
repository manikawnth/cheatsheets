# tcpdump

Used to capture and analyze packets on network

## Syntax

**tcpdump** <options> [protocols] [filters] [modifiers / logical operators] [filters]

## Examples

**sudo tcpdump -i any -n -v -X ip and host localhost and port 3030**

 - **sudo** - execute as super user
 - **tcpdump** - program to sniff packets
 - **i** - option: interface *any*
 - **n** - option: not name but address (127.0.0.1 instead of localhost)
 - **v** - option: verbose
 - **ip** - protocol: only capture ippackets
 - **host** - filter: host is either src (source) or dst (destination)
 - **and** - logical operator
 - **port** - filter: filter the port 3030
 
## Try it out

Shell 1: Start a netcat listener on port 3030

```shell
ubuntu@ubuntu-pc-master:~$ nc -l 3030
```

Shell 2: Start a netcat connection to port 3030

```shell
ubuntu@ubuntu-pc-master:~$ nc localhost 3030
```

Shell 3: Start tcpdump packet sniffer on localhost and 3030 and print the contents in both hex and ASCII

```shell
ubuntu@ubuntu-pc-master:~$ sudo tcpdump -i any -n -v -X ip and host localhost and port 3030
```

Input in shell 2: Send hello from the netcat connector to port 3030 (from step2)

```shell
ubuntu@ubuntu-pc-master:~$ nc localhost 3030
hello
```

Output on shell 3:

```
ubuntu@ubuntu-pc-master:~$ sudo tcpdump -i any -n -v -X ip and host localhost and port 3030

tcpdump: listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
13:35:16.353947 IP (tos 0x0, ttl 64, id 57583, offset 0, flags [DF], proto TCP (6), length 58)
    127.0.0.1.34480 > 127.0.0.1.3030: Flags [P.], cksum 0xfe2e (incorrect -> 0x3462), seq 1809892301:1809892307, ack 3106346124, win 342, options [nop,nop,TS val 1788889 ecr 1778465], length 6
	0x0000:  4500 003a e0ef 4000 4006 5bcc 7f00 0001  E..:..@.@.[.....
	0x0010:  7f00 0001 86b0 0bd6 6be0 c3cd b927 148c  ........k....'..
	0x0020:  8018 0156 fe2e 0000 0101 080a 001b 4bd9  ...V..........K.
	0x0030:  001b 2321 6865 6c6c 6f0a                 ..#!hello.
13:35:16.353957 IP (tos 0x0, ttl 64, id 14253, offset 0, flags [DF], proto TCP (6), length 52)
    127.0.0.1.3030 > 127.0.0.1.34480: Flags [.], cksum 0xfe28 (incorrect -> 0x4f8e), ack 6, win 342, options [nop,nop,TS val 1788889 ecr 1788889], length 0
	0x0000:  4500 0034 37ad 4000 4006 0515 7f00 0001  E..47.@.@.......
	0x0010:  7f00 0001 0bd6 86b0 b927 148c 6be0 c3d3  .........'..k...
	0x0020:  8010 0156 fe28 0000 0101 080a 001b 4bd9  ...V.(........K.
	0x0030:  001b 4bd9                                ..K.

```

## Other cases
For complete options, refer man page `man tcpdump` or `tcpdump -h`



