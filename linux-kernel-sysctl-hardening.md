# Linux Kernel `sysctl` Hardening

## Table of Contents

- [Overview](#overview)
- [Documentation](#documentation)
- [Disclaimer](#disclaimer)
- [Keys](#keys)

## Overview

This is a combined list of all the `sysctl` hardening recommendations I could find across multiple sites:

- https://www.cyberciti.biz/faq/linux-kernel-etcsysctl-conf-security-hardening/
- https://geektnt.com/sysctl-conf-hardening.html
- https://linoxide.com/how-tos/linux-server-protection/
- https://cloudpro.zone/index.php/2018/01/30/debian-9-3-server-setup-guide-part-5/
- https://github.com/klaver/sysctl/blob/master/sysctl.conf

## Documentation

Documentation for **most** of these keys can be found at https://github.com/torvalds/linux/blob/master/Documentation. However, the documentation seems to be for the 2.2 kernel. I cannot find any newer documentation. If you know where I can find newer documentation please submit a [new issue](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/issues/new).

## Disclaimer

I do not know what most of these settings do. This list is being provided just as reference material. I take no responsibility for anything.

## Keys

|`key=value`|Note|[Documentation|
|--|--|--|
|`fs.file-max = 65535`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`fs.protected_hardlinks = 1`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`fs.protected_symlinks = 1`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`fs.suid_dumpable = 0`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`kernel.core_uses_pid = 1`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.ctrl-alt-del = 0`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.kptr_restrict = 2`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.maps_protect = 1`|||
|`kernel.msgmax = 65535`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.msgmnb = 65535`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.pid_max = 65535`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.randomize_va_space = 2`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.shmall = 268435456`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.shmmax = 268435456`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.sysrq = 0`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`net.core.default_qdisc = fq`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.dev_weight = 64`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.netdev_max_backlog = 16384`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.optmem_max = 65535`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.rmem_default = 262144`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.rmem_max = 16777216`||[/sysctl/net.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/net.txt)|
|`net.core.somaxconn = 32768`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.core.wmem_default = 262144`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.core.wmem_max = 16777216`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.bootp_relay = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.forwarding = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.log_martians = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.proxy_arp = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.rp_filter = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.secure_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.send_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.forwarding = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.log_martians = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.rp_filter = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.secure_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.send_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.eth0.accept_redirects = 0`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.eth0.accept_source_route = 0`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.eth0.log_martians = 0`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.eth0.rp_filter = 1`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.lo.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.lo.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.lo.log_martians = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.lo.rp_filter = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.icmp_echo_ignore_all = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.icmp_echo_ignore_broadcasts = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.icmp_ignore_bogus_error_responses = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.ip_forward = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.ip_local_port_range = 2000 65000`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.ipfrag_high_thresh = 262144`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.ipfrag_low_thresh = 196608`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.neigh.default.gc_interval = 30`|||
|`net.ipv4.neigh.default.gc_thresh1 = 32`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.neigh.default.gc_thresh2 = 1024`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.neigh.default.gc_thresh3 = 2048`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.neigh.default.proxy_qlen = 96`|||
|`net.ipv4.neigh.default.unres_qlen = 6`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.route.flush = 1`|||
|`net.ipv4.tcp_congestion_control = htcp`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_ecn = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_fastopen = 3`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_fin_timeout = 15`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_keepalive_intvl = 15`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_keepalive_probes = 5`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_keepalive_time = 1800`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_max_orphans = 16384`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_max_syn_backlog = 2048`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_max_tw_buckets = 1440000`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_moderate_rcvbuf = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_no_metrics_save = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_orphan_retries = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_reordering = 3`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_retries1 = 3`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_retries2 = 15`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_rfc1337 = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_rmem = 8192 87380 16777216`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_sack = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_slow_start_after_idle = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_syn_retries = 5`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_synack_retries = 2`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_syncookies = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_timestamps = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_tw_recycle = 0`|||
|`net.ipv4.tcp_tw_reuse = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_window_scaling = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_wmem = 8192 65536 16777216`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.udp_rmem_min = 16384`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.udp_wmem_min = 16384`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.accept_ra=0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.autoconf = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.forwarding = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_defrtr = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_pinfo = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_rtr_pref = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra=0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.autoconf = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.dad_transmits = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.forwarding = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.max_addresses = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.router_solicitations = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.eth0.accept_ra=0`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.eth0.autoconf = 0`|change `eth0` to your network interface|[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.ip6frag_high_thresh = 262144`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.ip6frag_low_thresh = 196608`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.route.flush = 1`|||
|`net.unix.max_dgram_qlen = 50`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`vm.dirty_background_ratio = 5`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.dirty_ratio = 30`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.min_free_kbytes = 65535`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.mmap_min_addr = 4096`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.overcommit_memory = 0`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.overcommit_ratio = 50`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
|`vm.swappiness = 30`||[/sysctl/vm.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/vm.txt)|
