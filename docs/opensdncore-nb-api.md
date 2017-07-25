# JSON-RPC-OpenFlow


## Notations

In addition to the standard JSON data types, several special types are defined to improve the expressiveness of parameters in a RPC call. The following table shows the detail of each type.

### hex

a hex number given as a JSON string. The "0x" prefix is optional here.

 Example


            "hex":"0xabcd1234"

### hybrid

could be either a hex or a string. Must be given as a JSON string.
 Example


            "hybrid":"0xabcd1234"
            "hybrid":"max"

### enum

a JSON string whose value can just be in a limit range. The range will be defined in an array with each possible value and its meaning, and the name of the array will be given in upper case.
 Example


            "enum":"in_port"

            where the range is given in
            "OFPP": [
                {
                    "enum_value": "max",
                    "enum_meaning": "max port the switch can support"
                },
                {
                    "enum_value": "in_port",
                    "enum_meaning": "send the packet out the inpurt port. This reserved port must be explicitly used in order to send back out of the input port"
                },
                {
                    "enum_value": "table",
                    "enum_meaning": "submit the packet to the first flow table. Can used only in packet out message"
                }
                ...
            ]

### bitmap
a JSON array that consists of enum values
 Example


            "bitmap":[
                "port_down",
                "no_fwd",
                "no_packet_in"
            ]

            where the range is given in
            "OFPPC": [
                {
                    "enum_value": "port_down",
                    "enum_meaning": "port is administratively down"
                },
                {
                    "enum_value": "no_recv",
                    "enum_meaning": "drop all packets received by port"
                },
                {
                    "enum_value": "no_fwd",
                    "enum_meaning": "drop packets forwarded to port"
                },
                {
                    "enum_value": "no_packet_in",
                    "enum_meaning": "do not send packet-in message for port "
                }
                ...
            ]

### bytearray

a JSON array that consists of decimal numbers, with each number between [0, 255](i.e. a byte), or a hex byte string with the first byte being the first element of the array. Notice that by using hex representation, one byte consists of two hex digits.
 Example


            The following two fields represent the same byte array:
            "bytearray":[1,2,3,4,5,6]
            "bytearray":"0x010203040506"

-------------------------------------

## Data Structures

### ofp_match

an array consists of ofp_oxm objects

  Example


        [
            {
                "match_class": "openflow_basic",
                "field": "ipv4_src",
                "mask": "255.255.255.0",
                "value": "192.168.1.1"
            },
            {
                "match_class": "openflow_basic",
                "field": "tcp_src",
                "value": "5566"
            }
        ]
------------------------------------


### ofp_oxm

  ofp_oxm

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| match_class | oxm matching class | enum | one of OFPXMC enumeration |
| field | match field | enum | one of OFPXMT_OFB enumeration |
| value | match value | string | Unless specified, this value should be given in conventional form according to its type(e.g. TCP/UDP/SCTP ports should be given in decimal but as string |
| mask | match mask | string | the same length as value |

  OFPXMC

| value | description |
|---------|---------------|
| nxm0 | backward compatibility with NXM |
| nxm1 | backward compatibility with NXM |
| openflow_basic | basic class for OpenFlow |
| experimenter | experimenter class |

  OFPXMT_OFB

| value | description |
|---------|---------------|
| in_port | switch input port |
| in_phy_port | switch physical input port |
| metadata | metadata passed between tables |
| eth_dst | ethernet destination address |
| eth_src | ethernet source address |
| eth_type | ethernet frame type |
| vlan_id | VLAN id |
| vlan_pcp | VLAN priority |
| ip_dscp | IP DSCP(6 bits in ToS field) |
| ip_ecn | IP ECN(2 bits in ToS field) |
| ip_proto | IP protocol |
| ipv4_src | IPv4 source address |
| ipv4_dst | IPv4 destination address |
| tcp_src | tcp source port |
| tcp_dst | tcp destination port |
| udp_src | udp source port |
| udp_dst | udp destination port |
| sctp_src | sctp source port |
| sctp_dst | sctp destination port |
| icmpv4_type | ICMP type |
| icmpv4_code | ICMP code |
| arp_op | ARP opcode |
| arp_spa | ARP source IPv4 address |
| arp_tpa | ARP target IPv4 address |
| arp_sha | ARP source hardware address |
| arp_tha | ARP target hardware address |
| ipv6_src | IPv6 source address |
| ipv6_dst | IPv6 destination address |
| ipv6_flabel | IPv6 flow label |
| icmpv6_type | ICMP type |
| icmpv6_code | ICMP code |
| ipv6_nd_target | target address for ND |
| ipv6_nd_sll | source link-layer for ND |
| ipv6_nd_tll | target link-layer for ND |
| mpls_label | MPLS lable |
| mpls_tc | MPLS TC |
| mpls_bos | MPLS BoS bit |
| pbb_isid | PBB I-SID |
| tunnel_id | logical port metadata |
| ipv6_exthdr | IPv6 extension header pseudo-field |
| pbb_uca | PBB UCA header fields

  Example


            {
                "match_class": "openflow_basic",
                "field": "ipv4_src",
                "mask": "255.255.255.0",
                "value": "192.168.1.1"
            }
-------------------------------------


### ofp_instructions

consists of several objects which represent different kinds of instructions.
Notice that each instruction can appear only once in an instruction list.

  goto_table

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| table_id | set next table in the lookup pipeline | number | a number between [0, 255] |

  write_metadata

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| metadata | metadata value to write | hex | a 64-bit hex number |
| metadata_mask | metadata write bitmask | hex | a 64-bit hex number |

  meter

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| meter_id | meter instance | hybrid | a 32-bit hex number between [0,0xffff0000] or one of OFPM enumeration |

  experimenter

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| exp_id | experimenter id | hex | a 32-bit hex number |
| exp_data | experimenter-defined additional data | bytearray | a byte array |

  apply_actions

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ofp_actions | actions to apply | object | the same form as ofp_actions object |

  write_actions

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ofp_actions | actions to write | object | the same form as ofp_actions object |

  clear_actions

null


  OFPM

| value | description |
|---------|---------------|
| max | last usable meter |
| slowpath | meter for slow datapath |
| controller | meter for controller connection |
| all | represents all meters for stat requests commands |

  Example


        "ofp_instructions": {
            "write_actions": {
                "output": {
                    "port_no": "0x0f"
                }
            },
            "meter":{
                "meter_id":"0x01020304"
            }
        }

-------------------------------------


### ofp_actions

consists of several objects which represent different kinds of actions.
Notice that each action can appear only once in an action list.

  output

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| port_no | output port | hybrid | a 32-bit number between [0, 0xffffff00] or one of OFPP enumeration |
| max_len | max length to send to controller | hybrid | a 16-bit hex number between [0, 0xffe5] or one of OFPCML enumeration |

  group

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| group_id | group id | hybrid | a 32-bit hex number between [0, 0xffffff00] or one of OFPG enumeration |

  set_queue

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| queue_id | queue id for the packet | hex | a 32-bit hex number |

  set_mpls_ttl

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| mpls_ttl | the MPLS TTL to set | number | a number between [0, 255] |

  set_nw_ttl

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| nw_ttl | the NW TTL to set | number | a number between [0, 255] |

  push_vlan

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ethertype | the Ethertype of the new tag | hex | a 16-bit hex number |

  push_pbb

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ethertype | the Ethertype of the new tag | hex | a 16-bit hex number |

  push_mpls

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ethertype | the Ethertype of the new tag | hex | a 16-bit hex number |

  pop_mpls

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ethertype | the Ethertype of the MPLS payload | hex | a 16-bit hex number |

  set_field

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| ofp_oxm | the field to set described by a single oxm | object | a single object having the same form as ofp_oxm object |

  experimenter

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| exp_id | experimenter id | hex | a 32-bit hex number |
| exp_data | experimenter-defined additional data | bytearray | a byte array |

  copy_ttl_out

null

  copy_ttl_in

null

  dec_mpls_ttl

null

  dec_nw_ttl

null

  pop_pbb

null

  pop_vlan

null

  OFPCML

| value | description |
|---------|---------------|
| max | maximun max_len value which can be used to request a specific byte length |
| no_buffer | indicates that no buffering should be applied and the whole packet is to be sent to the controller |

  OFPG

| value | description |
|---------|---------------|
| max | last usable group |
| all | represents all groups for group delete commands |
| any | wildcard group used only for flow stats requests. selects all flows regardless of group (including flows with no group) |

  Example


                    "ofp_instructions": {
                        "write_actions": [
                            {
                                "set_field": {
                                    "ofp_oxm": {
                                        "match_class": "openflow_basic",
                                        "field": "ipv4_src",
                                        "value": "11.0.0.1"
                                    }
                                }
                            },
                            {
                                "set_field": {
                                    "ofp_oxm": {
                                        "match_class": "openflow_basic",
                                        "field": "ipv4_dst",
                                        "value": "11.0.0.2"
                                    }
                                }
                            },
                            {
                                "output": {
                                    "port_no": "0x02"
                                }
                            }
                        ]
                    }

-------------------------------------

## Switch Management

### list.channels

list datapath id of switches connecting to this controller
  Params

null

  Result

a JSON array consists of dpids(64-bit hex number given as string)

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "list.channels",
            "params": null
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": [
                "0xaabb000102030405",
                "0xccdd000102030405",
                "0x1234567890abcdef"
             ]
        }


-------------------------------------
## Controller-to-Switch Messaging

### ofc.send.get_config

get switch configuration

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |

  Result

a JSON object containing switch configuration information

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| flags | set of configuration flags | bitmap | possible values enumerated in OFPC_FRAG |
| miss_send_len | max bytes of packet that datapath should send to the controller. | hybrid | a 16-bit hex number between [0, 0xffe5] or one of OFPCML enumeration |

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.get_config",
            "params": {
                "dpid": "0xaabb000102030405"
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": {
                "flags": [
                    "normal",
                    "reasm"
                ],
                "miss_send_len": "no_buffer"
            }
        }

  OFPCML

| value | description |
|---------|---------------|
| max | max byte length |
| no_buffer | indicates that no buffering should be applied and the whole packet is to be sent to the controller |

  OFPC_FRAG

| value | description |
|---------|---------------|
| normal | no special handling for fragments |
| drop | drop fragments |
| reasm | reassemble |

------------------------------------

### ofc.send.flow_mod

send a flow_mod message

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |
| ofp_flow_mod | the flow_mod message | object | the same as described in ofp_flow_mod object | true |

  Result

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| msg_id | a unique ID identifying this request | hex | global unique to the controller |

  "ofp_flow_mod"::

a JSON object representing a flow_mod message

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| table_id | table id to be modified | hybrid | a 8-bit hex number between [0, 0xfe] or "all" for all tables | true |
| command | command to apply | enum | one of OFPFC enumeration | true |
| idle_timeout | idle time before discarding (seconds) | number |  | false |
| hard_timeout | max time before discarding (seconds) | number |  | false |
| priority | priority level of flow entry | number |  | false |
| cookie | require matching entries to contain this cookie value | hex | a 64-bit hex number | false |
| cookie_mask | cookie mask | hex | a 64-bit hex number | false |
| buffer_id | buffered packet to apply to. Not meaningful for delete commands | hybrid | a 32-bit hex number or "no_buffer" | false |
| out_port | for delete commands, require matching entries to include this as an output port | hybrid | a 32-bit hex number as described in ofp_port object, or "any" which indicates no restriction | false |
| out_group | for delete commands, require matching entries to include this as an output group | hybrid | a 32-bit hex number, or "any" which indicates no restriction | false |
| flags | flow entry configuration flags | bitmap | possible values enumerated in OFPFF | false |
| importance | eviction precedence | number | a number between [0, 2^ 16 ] | false |
| ofp_match | matching rules | array | consists of ofp_oxm objects | false |
| ofp_instructions | instructions to apply when matched | object | the same as  object | false |

  OFPFF

| value | description |
|---------|---------------|
| send_flow_rem | send flow removed message when flow expires or is deleted |
| check_overlap | check for overlapping entries first |
| reset_counts | reset flow packet and byte counts |
| no_pkt_counts | don't keep track of packet count |
| no_byt_counts | don't keep track of byte count |

  OFPFC

| value | description |
|---------|---------------|
| add | new flow |
| modify | modify all matching flows |
| modify_strict | modify entry strictly matching wildcards and priority |
| delete | delete all matching flows |
| delete_strict | delete entry strictly matching wildcars and priority |

 Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.flow_mod",
            "params": {
                "dpid": "0xaabb000102030405",
                "ofp_flow_mod": {
                    "command": "add",
                    "flags": [
                        "reset_counts",
                        "send_flow_rem"
                    ],
                    "idle_timeout": 14,
                    "ofp_instructions": {
                        "apply_actions": {
                            "output": {
                                "port_no": "0x0f"
                            }
                        }
                    },
                    "ofp_match": [
                        {
                            "match_class": "openflow_basic",
                            "field": "ipv4_src",
                            "mask": "255.255.255.0",
                            "value": "192.168.1.1"
                        }
                    ],
                    "priority": 18,
                    "table_id": "0x0f"
                }
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": {
                "msg_id": "0x01020304"
            }
        }

------------------------------------


### ofc.send.barrier

send barrier message, used to ensure message dependencies have been met or wants to receive notifications for completed operations.
The switch will continue to execute new commands only if all the messages before the barrier have been processed

 Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| "dpid" | switch datapath id | string | 64-bit hex number given as string | true |

 Result

a JSON String. "Barrier Reply" on success.

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.barrier",
            "params": {
                "dpid": "0xaabb000102030405"
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": "Barrier Reply"
        }
------------------------------------

### ofc.send.role_request

used to query or change the role of controller

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |
| role | new role that the controller wants to assume | enum | one of OFPCR enumeration | true |

  Result

| name | description | JSON type | restriction |
|--------|---------------|-------------|---------------|
| role | new role that accepted by switch. if the request is no_change, then the original role will be returned | enum | one of OFPCR enumeration. This field exists only if the new role is accepted by switch. |

  OFPCR

| value | description |
|---------|---------------|
| nochange | don't change current role |
| equal | default role, full access |
| master | full access, at most one master |
| slave | read-only access |

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.role_request",
            "params": {
                "dpid": "0xaabb000102030405",
                "role": "no_change"
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": {
                "role": "equal"
            }
        }

------------------------------------
### ofc.send.multipart.flow

get statistic information about individual flow entries

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |
| ofp_multipart_flow | a multipart_flow message | object | format described in  | true |

  Result

a JSON object consists of ofp_flow_stats object, whose format is introduced below


  Properties of "ofp_multipart_flow" object

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| table_id | table id | hybrid | a 8-bit hex number between [0, 0xfe] or "all" for all tables | true |
| out_port | Require matching entries to include this as an output port. | hybrid | a 32-bit hex number as described in ofp_port object, or "any" which indicates no restriction | false |
| out_group | Require matching entries to include this as an output group | hybrid | a 32-bit hex number, or "any" which indicates no restriction | false |
| cookie | Require matching entries to contain this cookie value | hex | a 64-bit hex number | false |
| cookie_mask | Require matching entries to contain this cookie value | hex | a 64-bit hex number,mask used to restrict the cookie bits that must match. | false |
| ofp_match | matching rules | array | consists of ofp_oxm objects | false |

 Properties of "ofp_flow_stats" object

| name | description | JSON type | note |
|--------|---------------|-------------|--------|
| table_id | table id the flow came from | hybrid | a 8-bit hex number between [0, 0xfe] or "all" for all tables |
| packet_count | number of packet in flow | number | |
| byte_count | number of byte in flow | number | |
| idle_timeout | idle time before discarding (seconds) | number |  |
| hard_timeout | max time before discarding (seconds) | number |  |
| duration_sec | time flow has been alive in seconds | number |  |
| duration_nsec | time flow has been alive in nanoseconds beyond duration_sec | number |  |
| priority | priority level of flow entry | number |  |
| flags | flow entry configuration flags | bitmap | possible values enumerated in OFPFF |
| cookie | require matching entries to contain this cookie value | hex | a 64-bit hex number | false |
| importance | eviction precedence | number | a number between [0, 2^ 16 ] | false |
| ofp_match | matching rules | array | consists of ofp_oxm objects |
| ofp_instructions | instructions to apply when matched | object | the same as ofp_instructions object |

  OFPFF

| value | description |
|---------|---------------|
| send_flow_rem | send flow removed message when flow expires or is deleted |
| check_overlap | check for overlapping entries first |
| reset_counts | reset flow packet and byte counts |
| no_pkt_counts | don't keep track of packet count |
| no_byt_counts | don't keep track of byte count |

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.multipart.flow",
            "params": {
                "dpid": "0xaabb000102030405",
                "ofp_multipart_flow": {
                    "ofp_match": [
                        {
                            "match_class": "openflow_basic",
                            "field": "ipv4_src",
                            "mask": "255.255.255.0",
                            "value": "192.168.1.1"
                        }
                    ],
                    "table_id": "0x0f"
                }
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": [
                {
                    "table_id": "0xe0",
                    "packet_count": 29348,
                    "byte_count": 7039684,
                    "idle_timeout": 3,
                    "hard_timeout": 15,
                    "duration_sec": 10,
                    "duration_nsec": 56,
                    "priority": 19,
                    "importance": 28,
                    "cookie": "0x1234567890abcdef",
                    "flags": [
                        "check_overlap",
                        "send_flow_rem"
                    ],
                    "ofp_match": [
                        {
                            "match_class": "openflow_basic",
                            "field": "ipv4_src",
                            "mask": "255.255.255.0",
                            "value": "192.168.1.1"
                        },
                        {
                            "match_class": "openflow_basic",
                            "field": "tcp_src",
                            "value": "5566"
                        }
                    ],
                    "ofp_instructions": {
                        "apply_actions": {
                            "output": {
                                "port_no": "0x0f"
                            }
                        }
                    }
                },
                {
                    "table_id": "0xe0",
                    "packet_count": 29348,
                    "byte_count": 7039684,
                    "idle_timeout": 7,
                    "hard_timeout": 9,
                    "duration_sec": 2,
                    "duration_nsec": 99,
                    "priority": 37,
                    "importance": 89,
                    "cookie": "0x000000000000000f",
                    "flags": [],
                    "ofp_match": [
                        {
                            "match_class": "openflow_basic",
                            "field": "ipv4_src",
                            "mask": "255.255.255.0",
                            "value": "192.168.1.1"
                        }
                    ],
                    "ofp_instructions": {
                        "goto_table": {
                            "table_id": "0x0f"
                        }
                    }
                }
            ]
        }

------------------------------------

### ofc.send.multipart.port_stats

get aggregate statistic information about ports

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |
| port_no | port that this queue attaching to, should refer to a valid physical port | hybrid | a 32-bit number between [0, 0xffffff00) or "all" for all ports | true |

  Result

| name | description | JSON type | note |
|--------|---------------|-------------|--------|
| port_no | port that this queue attaching to, should refer to a valid physical port | hybrid | a 32-bit number between [0, 0xffffff00) or "any" for all ports |
| tx_packets | number of transmitted packets | number | |
| rx_packets | number of received packets | number | |
| tx_bytes | number of transmitted bytes | number | |
| rx_bytes | number of received bytes | number | |
| tx_dropped | number of dropped packets | number | |
| rx_dropped | number of dropped packets | number | |
| tx_errors | Number of transmit errors. | number | a super-set of more specific transmit errors and should be greater than or equal to the sum of all tx_*_err values (none currently defined.) |
| rx_errors | Number of receive errors. | number | a super-set of more specific receive errors and should be greater than or equal to the sum of all rx_*_err values (none currently defined.) |
| duration_sec | time port has been alive in seconds | number |  |
| duration_nsec | time port has been alive in nanoseconds beyond duration_sec | number |  |
| properties | property list of port type specific statistics | array | consists of ofp_port_stats_prop objects |

 Properties of ofp_port_stats_prop object

| name | description | JSON type | note |
|--------|---------------|-------------|--------|
| type | port type | enum | one of OFPPSPT enumeration |
| rx_frame_err | number of frame alignment errors | number | only valid for type OFPPSPT_ETHERNET |
| rx_crc_err | number of CRC errors | number | only valid for type OFPPSPT_ETHERNET |
| rx_over_err | number of packets with RX overrun | number | only valid for type OFPPSPT_ETHERNET |
| collisions | number of collisions | number | only valid for type OFPPSPT_ETHERNET |

| flags | features enabled by the port | bitmap | possible values enumerated in OFPOSF |
|-------|------------------------------|--------|--------------------------------------|
| tx_freq_lmda | current TX frequency/Wavelength | number | only valid for type OFPPSPT_OPTICAL |
| tx_offset | TX offset | number | only valid for type OFPPSPT_OPTICAL |
| tx_grid_span | TX grid spacing | number | only valid for type OFPPSPT_OPTICAL |
| rx_freq_lmda | current RX frequency/Wavelength | number | only valid for type OFPPSPT_OPTICAL |
| rx_offset | RX offset | number | only valid for type OFPPSPT_OPTICAL |
| rx_grid_span | RX grid spacing | number | only valid for type OFPPSPT_OPTICAL |
| tx_pwr | current TX power | number | only valid for type OFPPSPT_OPTICAL |
| rx_pwr | current RX power | number | only valid for type OFPPSPT_OPTICAL |
| tx_grid_span | TX grid spacing | number | only valid for type OFPPSPT_OPTICAL |
| bias_current | TX Bias current | number | only valid for type OFPPSPT_OPTICAL |
| temperature | TX laser temperature | number | only valid for type OFPPSPT_OPTICAL |

| exp_id | id of the Experimenter | hex | a 32-bit hex number, only valid for type OFPPSPT_EXPERIMENTER |
|--------|------------------------|-----|---------------------------------------------------------------|
| exp_type | experimenter defined type | hex | a 32-bit hex number, only valid for type OFPPSPT_EXPERIMENTER |
| exp_data | experiment data | bytearray | only valid for type OFPPSPT_EXPERIMENTER |

  OFPPSPT


| value | description |
|---------|---------------|
| ethernet | ethernet property |
| optical | optical property |
| experimenter | experimenter property |

  OFPOSF

| value | description |
|---------|---------------|
| rx_tune | receiver tune info valid |
| tx_tune | transmit tune info valid |
| tx_pwr | TX power is valid |
| rx_pwr | RX power is valid |
| tx_bias | transmit bias is valid |
| tx_temp | TX temp is valid |

  Example


        -->
        {
            "id": 1,
            "jsonrpc": "2.0",
            "method": "ofc.send.multipart.port_stats",
            "params": {
                "dpid": "0xaabb000102030405",
                "port_no": "0xe0"
            }
        }

        <--
        {
            "id": 1,
            "jsonrpc": "2.0",
            "result": [
                {
                    "port_no": "0xe0",
                    "tx_packets": 28290,
                    "rx_packets": 29348,
                    "tx_bytes": 7039684,
                    "rx_bytes": 7200243,
                    "tx_dropped": 609,
                    "rx_dropped": 534,
                    "tx_errors": 97,
                    "rx_errors": 80,
                    "duration_sec": 2,
                    "duration_nsec": 56,
                    "properties": [
                        {
                            "type": "OFPPSPT_ETHERNET",
                            "rx_frame_err": 8,
                            "rx_crc_err": 15,
                            "rx_over_err": 57,
                            "collisions": 18
                        }
                    ]
                }
            ]
        }

------------------------------------

### ofc.send.multipart.port_description

 get a description of all the ports in the system that support OpenFlow

  Params

| name | description | JSON type | note | required |
|--------|---------------|-------------|--------|------------|
| dpid | switch datapath id | hex | 64-bit hex number given as string | true |

  Result

| name | description | JSON type | note |
|--------|---------------|-------------|--------|
| port_no | port that this queue attaching to, should refer to a valid physical port | hybrid | a 32-bit number between [0, 0xffffff00) or "any" for all ports |
| hw_addr |  Hardware address of the interface| number | |
| name | Name of the interface | number | |
| config | Bitmap of OFPPC_* flags| number | |
| state | Bitmap of OFPPS_* flags | number | |
| properties | property list of port type specific statistics | array | consists of ofp_port_desc_prop objects |

 Properties of ofp_port_desc_prop object

| name | description | JSON type | note |
|--------|---------------|-------------|--------|
| type | port type | enum |  |
| curr | number of frame alignment errors | number |  |
| advertised | Features being advertised by the port | number |  |
| supported |  Features supported by the port | number |  |
| peer |  Features advertised by peer| number |  |
| curr_speed |  Current port bitrate in kbps | number |  |
| max_speed | Max port bitrate in kbps | number |  |

  Example


        -->
       {
         "jsonrpc": "2.0",
         "method": "ofc.send.multipart.port_description",
         "params": {
            "dpid": "0x0000000000000001"
         },
         "id": 1
       }

        <--
       {
      "jsonrpc": "2.0",
      "result": [
        {
          "port_no": "1",
          "hw_addr": "ea:68:7d:44:9:75",
          "name": "s1-eth1",
          "config": [],
          "state": [],
          "properties": [
            {
              "type": "ETHERNET",
              "curr": [],
              "advertised": [],
              "supported": [],
              "peer": [],
              "curr_speed": 0,
              "max_speed": 102400
            }
          ]
        },
        {
          "port_no": "2",
          "hw_addr": "22:9d:cb:f9:97:cd",
          "name": "s1-eth2",
          "config": [],
          "state": [],
          "properties": [
            {
              "type": "ETHERNET",
              "curr": [],
              "advertised": [],
              "supported": [],
              "peer": [],
              "curr_speed": 0,
              "max_speed": 102400
            }
          ]
        }
      ],
      "id": 1
    }
------------------------------------


<!---
 Script for open external links in a new tab
-->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>
<script type="text/javascript" charset="utf-8">
      // Creating custom :external selector
      $.expr[':'].external = function(obj){
          return !obj.href.match(/^mailto\:/)
                  && (obj.hostname != location.hostname);
      };
      $(function(){
        $('a:external').addClass('external');
        $(".external").attr('target','_blank');
      })
</script>
