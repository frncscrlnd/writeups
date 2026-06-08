---
layout: default
title: SadServers Saint John: what is writing to this log file?
description: Walkthorugh/Writeup for the "Bata: Find in /proc" challenge, part of the easy section of the SadServers challenges.
set: Easy
order: 13
---

# ["Bata": Find in /proc](https://sadservers.com/scenario/bata)

This challenge tells us a spy has left a password in the `/proc/sys` directory. Let's get into this directory with `cd /proc/sys`. We only know that the contents of the file starts with this format: `secret:`. We'll have to put that secret into `/home/admin/secret.txt`. 

To search text for a line that matches a format we ca use [`grep`](https://www.gnu.org/software/grep/). However, using `grep secret .` (`.` stands for current directory) is not enough as grep will only search for the line in this directory. To go deeper into subdirectories, we have to use the `-r` flag, like this: `grep -r "secret" .`

This will return:

```
grep: ./fs/binfmt_misc/register: Permission denied
grep: ./fs/protected_fifos: Permission denied
grep: ./fs/protected_hardlinks: Permission denied
grep: ./fs/protected_regular: Permission denied
grep: ./fs/protected_symlinks: Permission denied
grep: ./kernel/cad_pid: Permission denied
./kernel/core_pattern:secret:excalibur
grep: ./kernel/unprivileged_userns_apparmor_policy: Permission denied
grep: ./kernel/usermodehelper/bset: Permission denied
grep: ./kernel/usermodehelper/inheritable: Permission denied
grep: ./net/core/bpf_jit_harden: Permission denied
grep: ./net/core/bpf_jit_kallsyms: Permission denied
grep: ./net/core/bpf_jit_limit: Permission denied
grep: ./net/ipv4/route/flush: Permission denied
grep: ./net/ipv4/tcp_fastopen_key: Permission denied
grep: ./net/ipv6/conf/all/stable_secret: Permission denied
grep: ./net/ipv6/conf/default/stable_secret: Permission denied
grep: ./net/ipv6/conf/docker0/stable_secret: Permission denied
grep: ./net/ipv6/conf/ens5/stable_secret: Permission denied
grep: ./net/ipv6/conf/lo/stable_secret: Permission denied
grep: ./net/ipv6/route/flush: Permission denied
grep: ./vm/compact_memory: Permission denied
grep: ./vm/drop_caches: Permission denied
grep: ./vm/mmap_rnd_bits: Permission denied
grep: ./vm/mmap_rnd_compat_bits: Permission denied
grep: ./vm/stat_refresh: Permission denied
```

This means that the password will be

>excalibur

We can now submit the password by using `echo "excalibur" > /home/admin/secret.txt`.