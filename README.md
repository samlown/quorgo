# Quorgo

Automated monitoring, failure detection, and configuration. With similar objectives to [Redis Sentinel](http://redis.io/topics/sentinel), Quorgo is a generic solution to maintain and check on the status of a list of hosts that provide services. If a host fails, Quorgo will do its best to clients know something went wrong so they can update their configurations.

Configuration change notifications can be made as result of fault, or manually if required. When running in master-mode, a quorum algorithm decides which host should be the master before allowing a configuration detail to change. Communication is handled by ZeroMQ using PUB/SUB sockets to efficiently transmit heartbeats between multiple servers and publish new configurations in the case of failure.

There are several parts to Quorgo:

 * `quorgo-server` reads a configuration file and periodically sends out heartbeats to anyone subscribed. In turn, the process listens for incoming heartbeats and attempts to suggest an alternate host in the case of failure or when requested.
 * `quorgo-client` subscribes to servers and waits for events decided by a quorum. If a change occurs, the configuration file determines what actions should be taken.
 * Client libraries subscribe to quorumify servers using ZeroMQ SUB sockets and wait for incoming changes that prompt some kind of action specifically programmed in the library. The Quorumify protocol is sufficiently simple for your own libraries to be implemented easily.

## Configuration

Quorgo accepts a simple YAML configuration file the describes the environment.
