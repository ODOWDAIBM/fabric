# peer command

## Description

 The `peer` command has five different subcommands, each of which allows
 administrators to perform a specific set of tasks related to a peer.  For
 example, you can use the `peer channel` subcommand to join a peer to a channel,
 or the `peer  chaincode` command to deploy a smart contract chaincode to a
 peer.

## Syntax

The `peer` command has five different subcommands within it:

```
peer chaincode [option] [flags]
peer channel   [option] [flags]
peer logging   [option] [flags]
peer node      [option] [flags]
peer version   [option] [flags]
```

Each subcommand has different options available, and these are described in their own dedicated topic.

If a subcommand is specified without an option, then `peer` will return some high level help text as described in the `--help` flag below.

## Flags

Each `peer` subcommand has a set of flags associated with it, many of which are designated *global* because they can be used at any command level.

For example, the `--help` flag can be used as follows
```
peer --help
peer channel --help
peer channel list --help
```

* `--help`

  Use `help` to get brief help text for different `peer` subcommands. The `help`
  flag is global -- it can be used at different levels to get individual command
  help, or even a help on a command option. See individual commands for more
  detail.

* `--logging-level <string>`

  This flag sets the logging level for a single invocation of a peer subcommand.
  It enables users to get a diagnostic output of the subcommand operation. The
  logging level setting does not persist across command invocations.

  There are six possible logging levels specified by `<string>` : `debug`,
  `info`, `notice`, `warning`, `error`, and `critical`. You can find the current
  logging level for a specific component on the peer by running `peer logging
  getlevel <component-name>`.

  If `logging-level` is not specified, then the default logging values are used.
  These defaults are defined in sampleconfig/core.yaml. Note that it is not possible to set a single logging level for the peer.

  This command and defaults are both overridden by the `CORE_LOGGING_LEVEL`
  environment variable if it is set.  The possible values for this environment
  variable are the same as the logging level values described above.

* `--version`

  Use this flag to determine the build version for the peer.  This flag provides
  a set of detailed information on how the peer was built.

## Usage

Here's some examples using the different available flags on the `peer` command.

* `--help` flag

  ```
  peer --help

  Usage: peer [flags] peer [command]

  Available Commands:
    chaincode   Operate a chaincode: install|instantiate|invoke|package|query|signpackage|upgrade.
    channel     Operate a channel: create|fetch|join|list|update.
    logging     Log levels: getlevel|setlevel|revertlevels.
    node        Operate a peer node: start|status.
    version     Print fabric peer version.

  Flags:
        --logging-level string       Default logging level and overrides, see   core.yaml for full syntax
        -v, --version                Display current version of fabric peer server

  Use "peer [command] --help" for more information about a command.
  ```

* `--version` flag

  ```
  peer --version

  peer:
   Version: 1.0.4
   Go version: go1.7.5
   OS/Arch: linux/amd64
   Chaincode:
    Base Image Version: 0.3.2
    Base Docker Namespace: hyperledger
    Base Docker Label: org.hyperledger.fabric
    Docker Namespace: hyperledger
  ```
