# subnet

Print addresses in CIDR range.

## Quickstart

Download the [subnet](https://github.com/eetami/subnet/releases/latest) shell
script to your `~/.local/bin` directory.

```bash
SUBNET_VERSION='v0.4.0'
wget -O ~/.local/bin/subnet "https://github.com/eetami/subnet/releases/download/${SUBNET_VERSION}/subnet"
subnet -h
```

This will also install files related to [simpleargs](https://github.com/laurivan/simpleargs)
under your home directory in `~/.simpleargs.d/`.

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

### Print all addresses in `192.168.201.1/30` network as [IPv4-mapped IPv6 addresses](https://www.rfc-editor.org/rfc/rfc4291.html#section-2.5.5.2)

```console
$ subnet -m 192.168.201.1/30
0000:0000:0000:0000:0000:ffff:192.168.201.0 (network address)
0000:0000:0000:0000:0000:ffff:192.168.201.1 *
0000:0000:0000:0000:0000:ffff:192.168.201.2
0000:0000:0000:0000:0000:ffff:192.168.201.3 (broadcast address)
```

The `-m` option can also be used in combination with other outputting options, such as `-c`, `-q` and `-u`.

### Print only usable addresses from above network as canonical IPv4-mapped IPv6 addresses in standard canonical IPv6 format without additional details

```console
$ subnet -cqumstandard 192.168.201.1/30
::ffff:c0a8:c901
::ffff:c0a8:c902
```

Since the `-m` option accepts an optional argument, it must be used as the last flag in an option argument.
For example the following invocation will produce an error.

```console
$ subnet -mc 192.168.201.1/30
ERROR: -m/--mapped: invalid value 'c'
Valid values are: 'false', 'default' and 'standard'.
Usage: subnet [OPTION]... <CIDR>
```

But the following invocation will work.

```console
$ subnet -cm 192.168.201.1/30
::ffff:192.168.201.0 (network address)
::ffff:192.168.201.1 *
::ffff:192.168.201.2
::ffff:192.168.201.3 (broadcast address)
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

## Offline use

subnet requires [simpleargs](https://github.com/laurivan/simpleargs) argument parser for bash.
If it is not present in user system, subnet will automatically download and install it (using
`wget` or `curl`, whichever is present) in user home directory.

However if the target system does not have internet access, or access to GitHub is blocked for
some reason, then manual installation is required.

The simplest way is to copy the simpleargs-${version} script onto the target system and follow
the [installation instructions](https://github.com/laurivan/simpleargs/blob/main/docs/installation.md).
