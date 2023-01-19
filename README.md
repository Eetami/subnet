# subnet

Print addresses in CIDR range.

## Quickstart

Download the [subnet](https://github.com/eetami/subnet/releases/latest) shell
script to your `~/.local/bin` directory. This will also install files related to
[simpleargs](https://github.com/laurivan/simpleargs) under your home directory in `~/.simpleargs.d/`.

```bash
SUBNET_VERSION='v0.3.0'
wget -O ~/.local/bin/subnet https://github.com/eetami/subnet/releases/download/${SUBNET_VERSION}/subnet
subnet -h
```

## Examples

### Print all IPv4 addresses in `192.168.201.1/30` network

```console
$ subnet 192.168.201.1/30
192.168.201.0 (network address)
192.168.201.1 *
192.168.201.2
192.168.201.3 (broadcast address)
```

### Print all addresses in the above network without additional details

```console
$ subnet -q 192.168.201.1/30
192.168.201.0
192.168.201.1
192.168.201.2
192.168.201.3
```

### Print only usable addresses from above network

```console
$ subnet -u 192.168.201.1/30
192.168.201.1 *
192.168.201.2
```

### Print above addresses on a single line without additional details

```console
$ subnet -qud' ' 192.168.201.1/30
192.168.201.1 192.168.201.2
```

### Print the first and last address of `dead::beef/64` IPv6 network

```console
$ subnet -v6 dead::beef/64
dead:0000:0000:0000:0000:0000:0000:0000
dead:0000:0000:0000:ffff:ffff:ffff:ffff
```
