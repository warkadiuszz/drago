
<h1 align="center"><br>
    <a"><img src="./misc/logo.png" alt="Drago" width="96"></a>
    <br>
    Drago
<br></h1>

<h5 align="center">
A flexible configuration manager for Wireguard networks
</h5>

------------------

<p align="center">
  <a href="https://goreportcard.com/report/github.com/seashell/drago"><img src="https://goreportcard.com/badge/github.com/seashell/drago" alt="Go report: A+"></a>
  <img alt="GitHub" src="https://img.shields.io/github/license/seashell/drago">
</p>

Drago is a flexible configuration manager for WireGuard networks which is designed to make it simple to configure network overlays spanning heterogeneous hosts distributed across different clouds and physical locations.

<p align="center"> 
<img src="misc/drago-demo.gif"/>
</p>

## Overview

[WireGuard®](https://www.wireguard.com/) is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform and widely deployable, being regarded as the most secure, easiest to use, and simplest VPN solution in the industry. 

Although Wireguard presents several advantages over other VPN solutions, it does not allow for the dynamic configuration of network parameters such as IP addresses and firewall rules.


## How it works

Drago is built on top of WireGuard, a performant and secure VPN that allows for the creation of secure network overlays across heterogeneous hosts, from containers to virtual machines, to IoT devices. It extends Wireguard's functionality by providing a unified control plane for dynamic configuration of the underlying network.

Drago follows a client-server paradigm, in which a centralized server provides multiple clients with their desired state, which is then used to derive changes that must be applied to each host.

This means that the Drago server works as a gateway for accessing network configurations safely stored in a database. It exposes a comprehensive API through which these configurations can be retrieved and modified, and also implements authentication mechanisms to prevent unauthorized access. For the sake of convenience, the Drago server also provides an easy-to-use web UI to facilitate the process of managing and visualizing network parameters and topology.

The Drago client, which runs on every host in the network, is responsible for directly interacting with the server through the API, and for retrieving the most up-to-date configurations. Through a simple reconciliation process, the Drago client then guarantees that the Wireguard configurations on each host match the desired state stored in the database. When running in client mode, Drago also takes care of automatically generating key pairs for Wireguard, and sharing the public key so that hosts can always connect to each other.

The only assumption made by Drago is that each host running the client is also running Wireguard, and that the host in which the configuration server is located is reachable through the network.

Drago does not enforce any specific network topology. Its sole responsibility is to distribute the desired configurations, and guarantee that they are correctly applied to Wireguard on every single registered host. This means that it is up to you to define how your hosts are connected to each other and how your network should look like.

Drago is meant to be simple, and provide a solid foundation for higher-level functionality. Need automatic IP assignment, dynamic firewall rules, or some kind of telemetry? You are free to implement on top of the already existing API.

## Build

System requirements:
- Golang 1.14+
- Node 10.17.0+
- yarn 1.12.3+

```
go generate
go build
```

## Usage

```
drago --help
drago agent --config=<config_file>
```

## Development

For development purposes, we suggest that you first start the Drago server:

```
go run main.go agent --config="./demo/server.yml"
```

Once the server is up and running, you can run a dev server for the web UI:

```
cd ui
yarn start
```

Finally, you can build and run the Drago client:

```
go build
sudo ./drago --config="./dist/client.yml"
```

## TODO
- Simple auth for the management API
- Certificate-based client authentication
- Automatic generation of client tokens (in addition to CLI command)
- Integration with a production-grade DB such as Postgres
- Collect metrics from links (e.g., upstream/downstream traffic, last handshake, etc)
- Collection of host metrics (e.g., last seen)
- Input validation (backend + frontend)
- Allow for the management of multiple overlay networks
- Improvements in the overall architecture
- Refactoring (project layout, variables name)
- Persistent connections (e.g., using Websockets) for enchanced responsiveness
- Filtering + Pagination
- Topology graph improvements (labels)
- Implement other storage backends (BoltDB, file, etc)
- Import / Export network topology from e.g., JSON file
- DB query optimization
