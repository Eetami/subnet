# subnet

Print addresses in CIDR range.

## Quickstart

Download the [subnet](https://github.com/eetami/subnet/releases/latest) shell
script to your `~/.local/bin` directory. This will also install files related to
[simpleargs](https://github.com/laurivan/simpleargs) under your home directory in
`~/.simpleargs.d/`.

```bash
SUBNET_VERSION='v0.4.0'
wget -O ~/.local/bin/subnet "https://github.com/eetami/subnet/releases/download/${SUBNET_VERSION}/subnet"
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

### Print above addresses as canonical IPv4-mapped IPv6 addresses

```console
$ subnet -cm 192.168.201.1/30
::ffff:192.168.201.0 (network address)
::ffff:192.168.201.1 *
::ffff:192.168.201.2
::ffff:192.168.201.3 (broadcast address)
```

### Print above addresses as canonical IPv4-mapped IPv6 addresses in standard canonical IPv6 format without additional details

```console
$ subnet -cqmstandard 192.168.201.1/30
::ffff:c0a8:c900
::ffff:c0a8:c901
::ffff:c0a8:c902
::ffff:c0a8:c903
```

### Print the first and last address of `dead::beef/64` IPv6 network

```console
$ subnet -v6 dead::beef/64
dead:0000:0000:0000:0000:0000:0000:0000
dead:0000:0000:0000:ffff:ffff:ffff:ffff
```

### Print all addresses in `dead::beef/126` IPv6 network in canonical form

```console
$ subnet -cv6 dead::beef/126
dead::beec
dead::beed
dead::beee
dead::beef *
```

### Print IPv4-mapped IPv6 network `::ffff:c0a8:beef/126` as IPv4 addresses

```console
$ subnet -v6 -m ::ffff:c0a8:beef/126
192.168.190.236
192.168.190.237
192.168.190.238
192.168.190.239 *
```

### Print IPv4-mapped IPv6 network `::ffff:c0a8:beef/126` as IPv4-mapped IPv6 addresses in canonical form

```console
$ subnet -v6 -cmstandard ::ffff:c0a8:beef/126
::ffff:192.168.190.236
::ffff:192.168.190.237
::ffff:192.168.190.238
::ffff:192.168.190.239 *
```
