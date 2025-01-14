---
title: Get Open Connections
platform: android
---

You can find system-wide networking information in `/proc/net` or just by inspecting the `/proc/<pid>/net` directories (for some reason not process specific). There are multiple files present in these directories, of which `tcp`, `tcp6` and `udp` might be considered relevant from the tester's perspective.

```bash
# cat /proc/7254/net/tcp
sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode
...
69: 1101A8C0:BB2F 9A447D4A:01BB 01 00000000:00000000 00:00000000 00000000 10093        0 75412 1 0000000000000000 20 3 19 10 -1
70: 1101A8C0:917C E3CB3AD8:01BB 01 00000000:00000000 00:00000000 00000000 10093        0 75553 1 0000000000000000 20 3 23 10 -1
71: 1101A8C0:C1E3 9C187D4A:01BB 01 00000000:00000000 00:00000000 00000000 10093        0 75458 1 0000000000000000 20 3 19 10 -1
...
```

In the output above, the most relevant fields for us are:

- `rem_address`: remote address and port number pair (in hexadecimal representation).
- `tx_queue` and `rx_queue`: the outgoing and incoming data queue in terms of kernel memory usage. These fields give an indication how actively the connection is being used.
- `uid`: containing the effective UID of the creator of the socket.

Another alternative is to use the `netstat` command, which also provides information about the network activity for the complete system in a more readable format, and can be easily filtered as per our requirements. For instance, we can easily filter it by PID:

```bash
# netstat -p | grep 24685
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program Name
tcp        0      0 192.168.1.17:47368      172.217.194.103:https   CLOSE_WAIT  24685/com.google.android.youtube
tcp        0      0 192.168.1.17:47233      172.217.194.94:https    CLOSE_WAIT  24685/com.google.android.youtube
tcp        0      0 192.168.1.17:38480      sc-in-f100.1e100.:https ESTABLISHED 24685/com.google.android.youtube
tcp        0      0 192.168.1.17:44833      74.125.24.91:https      ESTABLISHED 24685/com.google.android.youtube
tcp        0      0 192.168.1.17:38481      sc-in-f100.1e100.:https ESTABLISHED 24685/com.google.android.youtube
...
```

`netstat` output is clearly more user friendly than reading `/proc/<pid>/net`. The most relevant fields for us, similar to the previous output, are following:

- `Foreign Address`: remote address and port number pair (port number can be replaced with the well-known name of a protocol associated with the port).
- `Recv-Q` and `Send-Q`: Statistics related to receive and send queue. Gives an indication on how actively the connection is being used.
- `State`: the state of a socket, for example, if the socket is in active use (`ESTABLISHED`) or closed (`CLOSED`).
