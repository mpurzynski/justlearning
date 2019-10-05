.. table::
   :align: left
   :widths: auto

   ========== ==========
   rx_packets rx_packets
   ========== ==========

The number of successfully received packets (processed by the interface) on the interface. Updated after a successful SKB construction & fetch by NAPI.

.. table::
   :align: left
   :widths: auto

   ======== ========
   rx_bytes rx_bytes
   ======== ========

The number of successfully received bytes (processed by the interface) on the interface. Updated after a successful SKB construction & fetch by NAPI.



rx_dropped - CPU is not servicing buffers fast enough (buffers are not cleaned fast enough) - not enough free buffers to DMA into
rx_alloc_fail - failed to construct an SKB from a packet
rx_pg_alloc_fail - failed to allocate a page in the DMA area to send packets to. Pages are mostly reused so it is a rare event if a new page has to be allocated


intel counters
rx_dropped: means CPU not servicing buffers fast enough.
port.rx_dropped means something is not fast enough in the slot/memory/system

mellanox counters
ch[i]_aff_change The number of times the NAPI poll function explicitly stopped execution on a CPU due to a change in affinity, on channel. Supported from kernel 4.19
rx_out_of_buffer - like rx_dropped for intel
rx_buffer_passed_thres_phy - The number of events where the port receive buffer was over 85% full. Supported from kernel 4.14
rx[i]_buff_alloc_err / rx_buff_alloc_err - Failed to allocate a buffer to received packet (or SKB) on port (or per ring)
outbound_pci_buffer_overflow - The number of packets dropped due to pci buffer overflow. If this counter is raising in high rate, it might indicate that the receive traffic rate for a host is larger than the PCIe bus and therefore a congestion occurs. Supported from kernel 4.14

GLV_RDPC[n] - per VSI - received dropped packet count - no place to DMA into - no host - no descriptors in the host queue

GLPRT_RDPC - port.rx_dropped - eth.rx_discards - received packets from the network that are dropped in the receive packet buffer due to possible lack of bandwidth of the PCIe or the internal data path

is any column except 1st in /proc/net/softnet_stat increasing?
net.core.netdev_budget x2

softnet_stat 2nd column = netdev_max_backlog (but that's rps/rfs only)


Output column definitions:
      cpu  # of the cpu

    total  # of packets (not including netpoll) received by the interrupt handler
             There might be some double counting going on:
                net/core/dev.c:1643: __get_cpu_var(netdev_rx_stat).total++;
                net/core/dev.c:1836: __get_cpu_var(netdev_rx_stat).total++;
             I think the intention was that these were originally on separate
             receive paths ...

  dropped  # of packets that were dropped because netdev_max_backlog was exceeded
  queue->input_pkt_queue.qlen <= netdev_max_backlog

 squeezed  # of times ksoftirq ran out of netdev_budget or time slice with work
             remaining
             budget <= 0 || time_after(jiffies, time_limit)
    netdev_budget


monitor port.fdir_miss and port.fdir_match <- match if the FD was able to match the traffic. In the Perfect Filter mode without any filters all traffic will result in fdir_miss as no FD filter is being matched
