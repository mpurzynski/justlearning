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

.. table::
   :align: left
   :widths: auto

   ================ =================
   rx_pg_alloc_fail rx_buff_alloc_err
   ================ =================

Failed to allocate a page in the DMA area to send packets to. Pages are mostly reused so it is a rare event if a new page has to be allocated


.. table::
   :align: left
   :widths: auto

   ================ =================
   rx_dropped       rx_out_of_buffer
   ================ =================

Number of times receive queue had no software buffers allocated for the adapter's incoming traffic. That simply means there were not enough free buffers to DMA into.
::
   ethtool -g <int>

.. table::
   :align: left
   :widths: auto

   ================ =================
   port.rx_dropped  outbound_pci_buffer_overflow
   ================ =================

Received packets from the network that are dropped in the receive packet buffer due to possible lack of bandwidth of the PCIe or the internal data path. If this counter is raising in high rate, it might indicate that the receive traffic rate for a host is larger than the PCIe bus and therefore a congestion occurs. Make sure this card is installed in a PCIe x8 slot. Keep in mind some slots are x8 mechanically but x4 electrically.

.. table::
   :align: left
   :widths: auto

   ================ =================
   rx_alloc_fail
   ================ =================

Failed to construct an SKB from a packet - so also failed to clean the RX ring on time. Intel only.

rx_discards_phy - The number of received packets dropped due to lack of buffers on a physical port. If this counter is increasing, it implies that the adapter is congested and cannot absorb the traffic coming from the network.


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
