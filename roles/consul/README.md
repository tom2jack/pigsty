# Consul (ansible role)

This role will install consul on hosts

* if target server in dcs_servers, init it as a server
* otherwise, init a consul client and join dcs_servers
* if consul already exists, the action depends on variable `dcs_exists_action`, where:
    * `skip (default)` will skip this **play** on that host
    * `clean (dangerous)` will force remove existing consul instance
    * `abort` treat this as an error and will abort entire playbook for all hosts  
* variable `dcs_name` will be used as consul data center name
* key in `dcs_servers` will be used as consul server node name
* host variable `nodename` or node's `hostname` will be used as consul node name

### Tricks

```
./infra.yml --tags=dcs

```



### Tasks

[tasks/main.yml](tasks/main.yml)

```yaml
Check for existing consul		  TAGS: [consul_check]
Consul exists flag fact set		  TAGS: [consul_check]
Abort due to consul exists		  TAGS: [consul_check]
Clean existing consul instance	  TAGS: [consul_check]
Purge existing consul instance	  TAGS: [consul_check]
Make sure consul is installed	  TAGS: [consul_install]
Get dcs server node names		  TAGS: [consul_config]
Get dcs node name from var		  TAGS: [consul_config]
Fetch hostname as dcs node name	  TAGS: [consul_config]
Get dcs name from hostname		  TAGS: [consul_config]
Copy /etc/consul.d/consul.json	  TAGS: [consul_config]
Get dcs bootstrap expect quroum	  TAGS: [consul_server]
Copy consul server service unit	  TAGS: [consul_server]
Launch consul server service	  TAGS: [consul_server]
Wait for consul server online	  TAGS: [consul_server]
Copy consul agent service		  TAGS: [consul_agent]
Launch consul agent service		  TAGS: [consul_agent]
Wait for consul agent online	  TAGS: [consul_agent]
```

### Default variables

[defaults/main.yml](defaults/main.yml)

```yaml
---
dcs_type: consul                  # consul | etcd | both
dcs_name: pigsty                  # consul dc name | etcd initial cluster token
dcs_servers: {}                   # dcs name:ip dict (e.g: pg-meta-1: 10.10.10.10)
dcs_exists_action: skip           # skip|abort|clean if dcs server already exists
dcs_disable_purge: false          # set to true to disable purge functionality for good (force dcs_exists_action = abort)

consul_data_dir: /var/lib/consul  # default data directory
consul_exists: false              # default value for inner variable (DO NOT CHANGE!)
...
```