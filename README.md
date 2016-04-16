Ansible ScyllaDB role
=====

This role installs and configures ScyllaDB on a server.

Requirements
------------

This role requires Ansible 1.4 or higher and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

```yaml
---

# Install repository
scylladb_deb_repo: 'deb  [arch=amd64] http://s3.amazonaws.com/downloads.scylladb.com/deb/ubuntu trusty scylladb/multiverse'
scylladb_version_state: present

# Configuration boot parameters
scylladb_server:
  NETWORK_MODE: "posix"
  TAP: "tap0"
  BRIDGE: "virbr0"
  IFNAME: "eth0"
  SET_NIC: no
  NR_HUGEPAGES: 64
  USER: "scylla"
  GROUP: "scylla"
  SCYLLA_HOME: "/var/lib/scylla"
  SCYLLA_CONF: "/etc/scylla"
  SCYLLA_ARGS: '"--log-to-syslog 1 --log-to-stdout 0 --default-log-level info --collectd-address=127.0.0.1:25826 --collectd=1 --collectd-poll-period 3000 --network-stack posix --options-file /etc/scylla/scylla.yaml --listen-address {{ansible_default_ipv4.address}} --rpc-address {{ansible_default_ipv4.address}}"'
  AMI: no

scylladb_jmx:
  SCYLLA_HOME: "/var/lib/scylla"
  SCYLLA_CONF: "/etc/scylla"

# ScyllaDB configuration
scylladb_config:
  cluster_name: 'Test Cluster'
  num_tokens: 256
  data_file_directories:
    - /var/lib/scylla/data
  commitlog_directory: /var/lib/scylla/commitlog
  commitlog_sync: periodic
  commitlog_sync_period_in_ms: 10000
  commitlog_segment_size_in_mb: 32
  seed_provider:
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
        - seeds: "127.0.0.1"
  listen_address: localhost
  native_transport_port: 9042
  read_request_timeout_in_ms: 5000
  write_request_timeout_in_ms: 2000
  endpoint_snitch: SimpleSnitch
  rpc_address: localhost
  rpc_port: 9160
  api_port: 10000
  api_address: 127.0.0.1
  batch_size_warn_threshold_in_kb: 5
  partitioner: org.apache.cassandra.dht.Murmur3Partitioner
  commitlog_total_space_in_mb: -1
  api_ui_dir: /usr/lib/scylla/swagger-ui/dist/
  api_doc_dir: /usr/lib/scylla/api/api-doc/

# Cassandra RackDC Properties
scylladb_rackdc:

# Services
manage_scylla_jmx_service: true
```

Examples
========

```yaml
# Roles
- name: scylladb
  hosts: scylladb
  user: root
  roles:
    - deimosfr.scylladb
```

Dependencies
------------

None

License
-------

GPL

Author Information
------------------

Pierre Mavro / deimosfr
