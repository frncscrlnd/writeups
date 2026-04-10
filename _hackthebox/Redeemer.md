---
layout: default
title: Hack The Box Redeemer
description: Walkthorugh/Writeup for Redeemer, a Starting Point Hack The Box machine about the the Redis Database Management System
set: Starting Point
subset: Tier 0 - Foundations
order: 4
---

# [Redeemer](https://app.hackthebox.com/machines/Redeemer)<!-- omit in toc -->

This Starting Point machine revolves entirely around [Redis](https://en.wikipedia.org/wiki/Redis) or Remote Dictionary Server, an in-memory [DBMS](https://en.wikipedia.org/wiki/Database). 

### Table of contents:
- [Task 1](#task-1)
- [Task 2](#task-2)
- [Task 3](#task-3)
- [Task 4](#task-4)
- [Task 5](#task-5)
- [Task 6](#task-6)
- [Task 7](#task-7)
- [Task 8](#task-8)
- [Task 9](#task-9)
- [Task 10](#task-10)
- [Submit flag](#submit-flag)

## Task 1

*Which TCP port is open on the machine?*

Just like we did in the last machines (such as [Meow](https://frncscrlnd.github.io/writeups/hackthebox/Meow), [Fawn](https://frncscrlnd.github.io/writeups/hackthebox/Fawn) and [Dancing](https://frncscrlnd.github.io/writeups/hackthebox/Dancing)), we'll need to use `nmap`: `nmap target-ip-here`. However, this will return:

```
Nmap scan report for target-ip-here
Host is up (0.10s latency).
All 1000 scanned ports on targt-ip-here are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 1 IP address (1 host up) scanned in 8.63 seconds
```

Are there no open ports? No, it just means that the [1000 most popular TCP ports](https://nmap.org/book/man-port-scanning-basics.html#:~:text=nmap%20%3Ctarget%3E%20scans%201%2C000%20TCP%20ports%20on%20the%20host%20%3Ctarget) are closed. Use `-p-` to scan all ports (this can take some so, as the challenge suggests, we'll set an [aggressive timing template](https://nmap.org/book/port-scanning-options.html#port-scanning-options-timing:~:text=%2DT0%20through%20%2DT5)): `nmap target-ip-here -p- -T 5`. This will return:

```
Nmap scan report target-ip-here
Host is up (0.090s latency).
Not shown: 65501 closed tcp ports (reset), 33 filtered tcp ports (no-response)
PORT     STATE SERVICE
6379/tcp open  redis

Nmap done: 1 IP address (1 host up) scanned in 309.19 seconds
```

this means that port 

>6379

is open

## Task 2

*Which service is running on the port that is open on the machine?*

As per the last question,

>Redis

is running on [port](https://en.wikipedia.org/wiki/Port_(computer_networking)) 6379 

## Task 3

*What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database*

As we said earlier, [Redis](https://en.wikipedia.org/wiki/Redis) is an extremely fast DB due to the fact that it runs directly inside the [RAM](https://en.wikipedia.org/wiki/Random-access_memory). That makes Redis an

>in-memory database

## Task 4

*Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.*

Just like what we've seen in [Sequel](https://frncscrlnd.github.io/writeups/hackthebox/Sequel), there's a way to interact with the Redis server just like MariaDB: 

>redis-cli

## Task 5

*Which flag is used with the Redis command-line utility to specify the hostname?*

The full way to access Redis when the server is on a different machine than ours is `redis-cli -h server-hostname-here` so the flag we are looking for is

>-h

## Task 6

*Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?*

This command is straight-forward: 

>info

will give you information about the Redis server.

## Task 7

*What is the version of the Redis server being used on the target machine?*

Running `info` will return:

```
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:747
run_id:7f5d85e6ef773fd8a2bde05ed06c0e856cae91c4
tcp_port:6379
uptime_in_seconds:9236
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:14047108
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf

# Clients
connected_clients:1
client_recent_max_input_buffer:2
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:859624
used_memory_human:839.48K
used_memory_rss:5836800
used_memory_rss_human:5.57M
used_memory_peak:859624
used_memory_peak_human:839.48K
used_memory_peak_perc:100.12%
used_memory_overhead:846142
used_memory_startup:796224
used_memory_dataset:13482
used_memory_dataset_perc:21.26%
allocator_allocated:1408448
allocator_active:1806336
allocator_resident:9052160
total_system_memory:2084024320
total_system_memory_human:1.94G
used_memory_lua:41984
used_memory_lua_human:41.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.28
allocator_frag_bytes:397888
allocator_rss_ratio:5.01
allocator_rss_bytes:7245824
rss_overhead_ratio:0.64
rss_overhead_bytes:-3215360
mem_fragmentation_ratio:7.14
mem_fragmentation_bytes:5019184
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:49694
mem_aof_buffer:0
mem_allocator:jemalloc-5.2.1
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1775646453
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:417792
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:5
total_commands_processed:7
instantaneous_ops_per_sec:0
total_net_input_bytes:332
total_net_output_bytes:12068
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:1079
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:c556c6091ada142c9a91299758d4e027a8d46afa
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:10.095295
used_cpu_user:8.357904
used_cpu_sys_children:0.000000
used_cpu_user_children:0.003401

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

so the redis version is

>5.0.7

## Task 8

*Which command is used to select the desired database in Redis?*

As we can see from the above command,

```
# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

there's only one database to select, and its' id is `0`. If we were in a different DB we could've moved to this one with 

>SELECT

like this `SELECT 0`

## Task 9

*How many keys are present inside the database with index 0?*

As per the abova nswer, there are 

>4 

keys inside the db with index 0

## Task 10

*Which command is used to obtain all the keys in a database?*

This is another straight-forward command: to select all the key, we can just use

>KEYS *

in our case it will return:

```
1) "numb"
2) "flag"
3) "stor"
4) "temp"
```

## Submit flag

*Submit root flag*

Let's read the value of the "flag" key with `GET`: `GET flag`. This will return:

>03e1d2b376c37ab3f5319922053953eb
