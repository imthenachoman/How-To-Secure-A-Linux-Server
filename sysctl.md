# Linux Kernel `sysctl` Settings

## Overview

Documentation for **most** of these keys can be found at https://github.com/torvalds/linux/blob/master/Documentation.

## Table

|`key=value`|Note|[Documentation|
|--|--|--|
|`fs.file-max = 65535`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`fs.protected_hardlinks = 1`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`fs.protected_symlinks = 1`||[/sysctl/fs.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt)|
|`kernel.core_uses_pid = 1`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.ctrl-alt-del = 0`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.exec-shield = 1`|||
|`kernel.kptr_restrict = 2`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.pid_max = 65536`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.randomize_va_space = 2`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`kernel.sysrq = 0`||[/sysctl/kernel.txt](https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt)|
|`net.ipv4.conf.all.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.log_martians = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.rp_filter = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.secure_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.all.send_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.conf.default.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
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
|`net.ipv4.tcp_fin_timeout = 15`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_keepalive_time = 1800`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_max_syn_backlog = 2048`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_max_tw_buckets = 1440000`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_rfc1337 = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_sack = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_syn_retries = 5`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_synack_retries = 2`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_syncookies = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_timestamps = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv4.tcp_window_scaling = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.all.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_defrtr = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_pinfo = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_ra_rtr_pref = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_redirects = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.accept_source_route = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.autoconf = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.dad_transmits = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.max_addresses = 1`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|
|`net.ipv6.conf.default.router_solicitations = 0`||[/networking/ip-sysctl.txt](https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt)|

## Flat File

    ###
    # start from https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/
    ###
    
    ###
    # start fs
    # https://github.com/torvalds/linux/blob/master/Documentation/sysctl/fs.txt
    ###

    fs.file-max = 65535
    fs.protected_hardlinks = 1
    fs.protected_symlinks = 1

    ###
    # end fs
    ###

    ###
    # start kernel
    # https://github.com/torvalds/linux/blob/master/Documentation/sysctl/kernel.txt
    ###

    kernel.core_uses_pid = 1
    kernel.ctrl-alt-del = 0
    kernel.exec-shield = 1
    kernel.kptr_restrict = 2
    kernel.pid_max = 65536
    kernel.randomize_va_space = 2
    kernel.sysrq = 0

    ###
    # end kernel
    ###

    ###
    # start net
    # https://github.com/torvalds/linux/blob/master/Documentation/networking/ip-sysctl.txt
    ###

    net.ipv4.conf.all.accept_redirects = 0
    net.ipv4.conf.all.accept_source_route = 0
    net.ipv4.conf.all.log_martians = 1
    net.ipv4.conf.all.rp_filter = 1
    net.ipv4.conf.all.secure_redirects = 0
    net.ipv4.conf.all.send_redirects = 0
    net.ipv4.conf.default.accept_redirects = 0
    net.ipv4.conf.default.accept_source_route = 0
    net.ipv4.conf.default.log_martians = 1
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.conf.default.secure_redirects = 0
    net.ipv4.conf.default.send_redirects = 0

    # change eth0 to your network interface
    net.ipv4.conf.eth0.accept_redirects = 0
    net.ipv4.conf.eth0.accept_source_route = 0
    net.ipv4.conf.eth0.log_martians = 0
    net.ipv4.conf.eth0.rp_filter = 1

    net.ipv4.conf.lo.accept_redirects = 0
    net.ipv4.conf.lo.accept_source_route = 0
    net.ipv4.conf.lo.log_martians = 0
    net.ipv4.conf.lo.rp_filter = 1
    net.ipv4.icmp_echo_ignore_all = 1
    net.ipv4.icmp_echo_ignore_broadcasts = 1
    net.ipv4.icmp_ignore_bogus_error_responses = 1
    net.ipv4.ip_forward = 0
    net.ipv4.ip_local_port_range = 2000 65000
    net.ipv4.tcp_fin_timeout = 15
    net.ipv4.tcp_keepalive_time = 1800
    net.ipv4.tcp_max_syn_backlog = 2048
    net.ipv4.tcp_max_tw_buckets = 1440000
    net.ipv4.tcp_rfc1337 = 1
    net.ipv4.tcp_sack = 0
    net.ipv4.tcp_syn_retries = 5
    net.ipv4.tcp_synack_retries = 2
    net.ipv4.tcp_syncookies = 1
    net.ipv4.tcp_timestamps = 0
    net.ipv4.tcp_window_scaling = 0
    net.ipv6.conf.all.accept_redirects = 0
    net.ipv6.conf.all.accept_source_route = 0
    net.ipv6.conf.default.accept_ra_defrtr = 0
    net.ipv6.conf.default.accept_ra_pinfo = 0
    net.ipv6.conf.default.accept_ra_rtr_pref = 0
    net.ipv6.conf.default.accept_redirects = 0
    net.ipv6.conf.default.accept_source_route = 0
    net.ipv6.conf.default.autoconf = 0
    net.ipv6.conf.default.dad_transmits = 0
    net.ipv6.conf.default.max_addresses = 1
    net.ipv6.conf.default.router_solicitations = 0

    ###
    # end kernel
    ###
    
    ###
    # end from https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/
    ###
