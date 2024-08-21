# Sysctl

## Overview

This repository contains an optimized version of the `sysctl.conf` file designed to enhance the performance, security, and reliability of Linux servers. These optimizations are ideal for servers dedicated to web, mail, FTP services, among others. By implementing these configurations, you can ensure a more secure and efficient server environment.

## Features and Optimizations

### General System Security Options

- **kernel.panic = 10**

   Automatically reboots the system 10 seconds after a kernel panic, ensuring server availability.

- **kernel.sysrq = 0**

   Disables the System Request debugging functionality to enhance security.

- **kernel.core_uses_pid = 1**

   Appends the PID to core dump filenames, which is useful for debugging multi-threaded applications.

- **kernel.pid_max = 65535**

   Increases the maximum number of PIDs to support more concurrent processes.

- **kernel.maps_protect = 1**

   Restricts the visibility of `/proc/<pid>/maps` and `smaps` files to only processes allowed to `ptrace()`.

- **kernel.exec-shield = 1 and kernel.randomize_va_space = 2**

   Enables ExecShield protection and address space layout randomization (ASLR) for enhanced security.

- **fs.suid_dumpable = 0**

   Prevents core dumps from being created by suid processes, enhancing system security.

- **kernel.kptr_restrict = 1**

   Hides kernel pointers from non-privileged users, reducing the risk of information leaks.

### Improve System Memory Management

- **fs.file-max = 209708**

   Increases the maximum number of file descriptors and inode cache size for better performance.

- **vm.swappiness = 30**

   Reduces the system's tendency to swap, keeping more processes in physical memory.

- **vm.dirty_ratio = 30 and vm.dirty_background_ratio = 5**

   Controls the amount of memory that can be dirty before data is written to disk, optimizing I/O performance.

- **vm.mmap_min_addr = 4096**

   Sets the minimum virtual address that a process can mmap, protecting against certain types of exploits.

- **vm.overcommit_ratio = 50 and vm.overcommit_memory = 0**

   Manages memory overcommitment, ensuring that the system doesn't allocate more memory than available.

- **kernel.shmmax = 268435456 and kernel.shmall = 268435456**

   Sets the maximum shared memory segment size and total amount of shared memory available.

- **vm.min_free_kbytes = 65535**

   Ensures that at least 64MB of RAM is always available for critical operations.

### General Network Security Options

- **net.ipv4.tcp_syncookies = 1**

   Enables SYN cookies to protect against SYN flood attacks when the max SYN backlog is reached.

- **net.ipv4.ip_forward = 0**

   Disables IP packet forwarding to prevent the server from routing traffic.

- **net.ipv4.conf.all.accept_source_route = 0 and net.ipv6.conf.all.accept_source_route = 0**

   Disables IP source routing to prevent attackers from specifying the route a packet should take.

- **net.ipv4.conf.all.rp_filter = 1**

   Enables reverse path filtering to prevent IP spoofing.

- **net.ipv4.conf.all.accept_redirects = 0**

   Disables ICMP redirect acceptance to prevent man-in-the-middle attacks.

- **net.ipv4.conf.all.log_martians = 1**

   Logs packets with un-routable source addresses to help detect potential network issues or attacks.

- **net.ipv4.tcp_fin_timeout = 7**

   Reduces the time TCP connections remain in the FIN-WAIT-2 state, freeing up resources.

- **net.ipv4.tcp_keepalive_time = 300**

   Adjusts the frequency of TCP keepalive messages to maintain active connections.

- **net.ipv4.tcp_timestamps = 1**

   Enables TCP timestamps for better congestion control and round-trip time measurements.

- **net.ipv4.icmp_echo_ignore_broadcasts = 1**

   Ignores ICMP echo requests sent to broadcast addresses to prevent network disruptions.

- **net.ipv4.tcp_rfc1337 = 1**

   Applies RFC 1337 to mitigate time-wait assassination hazards in TCP connections.

- **net.ipv6.conf.all.autoconf = 0 and net.ipv6.conf.all.accept_ra = 0**

   Disables IPv6 auto-configuration and router advertisements to prevent unwanted address assignments.

### Tuning Network Performance

- **net.ipv4.tcp_congestion_control = bbr**

   Uses BBR congestion control to optimize network performance on high-speed networks.

- **net.core.default_qdisc = fq**

   Enables the `fq` queue management scheduler to handle TCP-heavy workloads efficiently.

- **net.ipv4.tcp_window_scaling = 1**

   Enables TCP window scaling to allow for larger TCP windows, improving throughput.

- **net.ipv4.tcp_rmem = 8192 87380 16777216 and net.ipv4.tcp_wmem = 8192 65536 16777216**

   Increases the buffer sizes for TCP read and write operations to handle larger amounts of data.

- **net.core.somaxconn = 32768**

   Increases the maximum number of incoming connections that can be queued.

- **net.core.netdev_max_backlog = 16384**

   Increases the maximum length of the queue for incoming packets, reducing packet loss under heavy load.

- **net.ipv4.tcp_max_tw_buckets = 1440000**

   Increases the size of the time-wait connection bucket to prevent simple DoS attacks.

- **net.ipv4.tcp_tw_reuse = 1**

   Allows reusing time-wait connections, improving resource utilization.

- **net.ipv4.tcp_no_metrics_save = 1**

   Prevents saving TCP metrics between connections, ensuring consistent connection performance.

- **net.ipv4.tcp_fastopen = 3**

   Enables TCP Fast Open to reduce the latency for establishing TCP connections.

- **net.ipv4.route.flush = 1**

   Ensures that route changes are applied immediately, improving network responsiveness."


## Usage

1. Clone this repository to your server:
   ```bash
   git clone https://github.com/weiseratel/sysctl.git
   ```
2. Backup your existing `sysctl.conf`:
   ```bash
   sudo cp /etc/sysctl.conf /etc/sysctl.conf.backup
   ```
3. Replace the existing `sysctl.conf` with the optimized version:
   ```bash
   sudo cp sysctl/sysctl.conf /etc/sysctl.conf
   ```
4. Apply the new configurations:
   ```bash
   sudo sysctl -p
   ```

## Important Notes

- Always review and understand each setting in the `sysctl.conf` file before applying it to a production environment.
- These settings are tailored for servers and might not be suitable for all environments, especially those requiring specific networking or performance configurations.

## Contributing

Contributions are welcome! If you have any improvements, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
