# peer channel

## Description

The `peer channel` subcommand allows administrators to perform channel related
operations on a peer, such as joining a channel or instantiating smart contract
chaincode on a channel.

## Syntax

The `peer channel` subcommand has the following syntax:

```
peer channel create [flags]
peer channel fetch  [flags]
peer channel join   [flags]
peer channel list   [flags]
peer channel update [flags]
```

The different subcommand options (create, fetch...) relate to the different
channel operations that are relevant to a peer. For example, use the `peer
channel join` subcommand option to join a peer to a channel, or the `peer
channel list` subcommand option to show the channels to which a peer is joined.

Each peer channel subcommand is described together with its options in its own
section in this topic.

## Flags

Each `peer channel` subcommand has both a set of flags specific to an individual
subcommand, as well as a set of global flags that relate to all `peer channel`
subcommands.

The individual flags are described with the relevant subcommand. The global flags are as follows:

```
peer channel --cafile <string>
peer channel --orderer <string>
peer channel --tls
```

* `--cafile <string>`

  a fully qualified path to a file containing PEM-encoded certificates for the
  orderer being communicated with. Use in conjunction with the `--tls` flag.

* `--orderer <string>`

  the fully qualified IP address and port of the orderer being communicated with
  for this channel operation.  If the port is not specified, it will default to
  port 7050. An IP address must be specified if the `--orderer` flag is used.

* `--tls`

  Use this flag to enable TLS communications for the `peer channel` command. The
  certificates specified with `--cafile` will be used for TLS communications to
  authenticate the orderer identified by `--orderer`.

## Usage

Here's some examples using the different available flags on the `peer channel`
command.

* Using the `--orderer` flag to list the channels to which a peer is joined.

  ```
  peer channel list --orderer orderer.example.com:7050

  2017-11-30 12:07:51.317 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
  2017-11-30 12:07:51.317 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
  2017-11-30 12:07:51.321 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
  2017-11-30 12:07:51.323 UTC [msp/identity] Sign -> DEBU 004 Sign: plaintext: 0A8A070A5C08031A0C0897E9FFD00510...631A0D0A0B4765744368616E6E656C73
  2017-11-30 12:07:51.323 UTC [msp/identity] Sign -> DEBU 005 Sign: digest: D170CD2D6FEB04E49033B54B0AC53744991ADAA320C5733074BC5227BD19E863
  2017-11-30 12:07:51.335 UTC [channelCmd] list -> INFO 006 Channels peers has joined to:
  2017-11-30 12:07:51.335 UTC [channelCmd] list -> INFO 007 drivenet.channel.001
  2017-11-30 12:07:51.335 UTC [main] main -> INFO 008 Exiting.....
  ```

  You can see that the peer joined to a channel called `drivenet.channel.001`.

## peer channel create

(TBD)

### Create Description

### Create Syntax

### Create Flags

### Create Usage

## peer channel fetch

### Fetch Description

The `peer channel fetch` command allows administrators to fetch channel
transaction blocks from the orderer. The retrieved blocks will typically contain
user transactions but they can also contain configuration transactions such as
the initial genesis block for the channel or any subsequent channel
configuration update.

The peer must have joined the channel, and have read access to it, in order for
the command to complete successfully.

### Fetch Syntax

The `peer channel fetch` command has the following syntax:

```
peer channel fetch [newest|oldest|config|(block number)] [flags]
```

  where

  * `newest`

    returns the most recent channel block available to the network orderer. This
    may be a user transaction block or a configuration transaction.

    This option will also return the block number of the most recent
    transaction.

  * `oldest`

    returns the oldest channel block available to the network orderer. This may
    be a user transaction block or a configuration transaction.

    This option will also return the block number of the oldest available
    transaction.

  * `config`

    returns the most recent channel configuration block available to the network
    orderer. This can only be a configuration transaction.

    This option will also return the block number of the most recent
    configuration transaction.

  * `(block number)`

    returns the specified channel block. This may be a user transaction block or
    a configuration transaction.

    Specifying 0 will result in the genesis block for this channel being
    returned (if it is still available to the network orderer).

Output from the `peer channel fetch` command is written to a file named according to the fetch options. It will be one of the following:

  * `<channelID>_newest.block`
  * `<channelID>_oldest.block`
  * `<channelID>_config.block`
  * `<channelID>_(block number).block`

### Fetch Flags

The `peer channel fetch` command has the following command specific flags:

  * `--channelID <string>`

    where `<string>` is the name of the channel for which the blocks are to be
    fetched from the orderer.

The global `peer` command flags also apply:

  * `--cafile`
  * `--orderer`
  * `--tls`

### Fetch Usage

  Here's some examples of the `peer channel fetch` command.

  * Using the `newest` option to retrieve the most recent channel block.

    ```
    peer channel fetch newest  -c drivenet.channel.001 --orderer orderer.example.com:7050

    2017-11-30 17:02:56.234 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
    2017-11-30 17:02:56.234 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
    2017-11-30 17:02:56.237 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
    2017-11-30 17:02:56.237 UTC [msp] GetLocalMSP -> DEBU 004 Returning existing local MSP
    2017-11-30 17:02:56.237 UTC [msp] GetDefaultSigningIdentity -> DEBU 005 Obtaining default signing identity
    2017-11-30 17:02:56.240 UTC [msp] GetLocalMSP -> DEBU 006 Returning existing local MSP
    2017-11-30 17:02:56.240 UTC [msp] GetDefaultSigningIdentity -> DEBU 007 Obtaining default signing identity
    2017-11-30 17:02:56.240 UTC [msp/identity] Sign -> DEBU 008 Sign: plaintext: 0AC9060A1B08021A0608C0F380D10522...DC7F80E9BEE612080A020A0012020A00
    2017-11-30 17:02:56.241 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: D3F6C959BCFCD78B5895A466276C181EEA3B54C1CF8E8707238FE3A3D358F769
    2017-11-30 17:02:56.245 UTC [channelCmd] readBlock -> DEBU 00a Received block: 32
    2017-11-30 17:02:56.246 UTC [main] main -> INFO 00b Exiting.....

    ls -alt

    total 276
    drwxr-xr-x 2 root root   4096 Nov 30 16:17 .
    -rw-r--r-- 1 root root  13307 Nov 30 17:02 drivenet.channel.001_newest.block
    drwxr-xr-x 3 root root   4096 Nov 21 13:38 ..

    ```

    You can see that the retrieved block is number 32, and that the information
    has been written to the file `drivenet.channel.001_newest.block`

  * Using the `(block number)` option to retrieve a specific block -- in this case, block number 16.

    ```
    peer channel fetch 16  -c drivenet.channel.001 --orderer orderer.example.com:7050

    2017-11-30 17:08:12.039 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
    2017-11-30 17:08:12.039 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
    2017-11-30 17:08:12.042 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
    2017-11-30 17:08:12.042 UTC [msp] GetLocalMSP -> DEBU 004 Returning existing local MSP
    2017-11-30 17:08:12.042 UTC [msp] GetDefaultSigningIdentity -> DEBU 005 Obtaining default signing identity
    2017-11-30 17:08:12.042 UTC [msp] GetLocalMSP -> DEBU 006 Returning existing local MSP
    2017-11-30 17:08:12.042 UTC [msp] GetDefaultSigningIdentity -> DEBU 007 Obtaining default signing identity
    2017-11-30 17:08:12.042 UTC [msp/identity] Sign -> DEBU 008 Sign: plaintext: 0AC9060A1B08021A0608FCF580D10522...B092120C0A041A02081012041A020810
    2017-11-30 17:08:12.042 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: CD6F4ADB7E00E79E4FADBE627CBE7CAA6F2A4471A9A0BE780CD4BE65AF8B96DE
    2017-11-30 17:08:12.046 UTC [channelCmd] readBlock -> DEBU 00a Received block: 16
    2017-11-30 17:08:12.046 UTC [main] main -> INFO 00b Exiting.....

    ls -alt

    total 276
    drwxr-xr-x 2 root root   4096 Nov 30 16:17 .
    -rw-r--r-- 1 root root  10474 Nov 30 17:08 drivenet.channel.001_16.block
    -rw-r--r-- 1 root root  13307 Nov 30 17:02 drivenet.channel.001_newest.block
    drwxr-xr-x 3 root root   4096 Nov 21 13:38 ..

    ```

    You can see that the retrieved block is number 16, and that the information
    has been written to the file `drivenet.channel.001_newest.block`

For configuration blocks, the file can be formatted using the `configtxlator`
[command](./configtxlator.html). See this command for an example of formatted output.

## peer channel join

(TBD)

### Join Description

### Join Syntax

### Join Flags

### Join Usage


## peer channel list

(TBD)

### List Description

### List Syntax

### List Flags

### List Usage


## peer channel update

(TBD)

### Update Description

### Update Syntax

### Update Flags

### Update Usage
