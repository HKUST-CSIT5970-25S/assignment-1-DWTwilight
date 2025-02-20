[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)

# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Tianci ZHAO

### Student Id: 21093775

### Email: tzhaoat@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose _any_ open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

> | Test Subject       | Tool     | Command & Params                                                                                                                    | Result                        |
> | ------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
> | CPU Performance    | sysbench | sysbench cpu run --threads=`num of vcores` (use default config to generate 10000 primes, with `threads` param to utilize all cores) | CPU speed: events per second  |
> | Memory Performance | sysbench | sysbench memory run --threads=`num of vcores` (use default config to test seq write, with `threads` param to utilize all cores)     | data transfer speed (MiB/sec) |

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

   In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

   | Size        | CPU performance (events/sec) | Memory performance (MiB/sec) |
   | ----------- | ---------------------------- | ---------------------------- |
   | `t2.micro`  | 2287.01                      | 534.13                       |
   | `t2.medium` | 4488.51                      | 904.99                       |
   | `c5d.large` | 1800.08                      | 7582.84                      |

   > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

> Yes, the performance of EC2 instances do increase with more resources. However, the `t2` instances seems to have better CPUs than `c5`, enabling them to have better CPU performance with same or less vCPU count.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

   | Type                      | TCP b/w (Mbps) | RTT (ms) |
   | ------------------------- | -------------- | -------- |
   | `t3.medium` - `t3.medium` | 4280.32        | 0.277    |
   | `m5.large` - `m5.large`   | 5068.8         | 0.219    |
   | `c5n.large` - `c5n.large` | 5058.56        | 0.164    |
   | `t3.medium` - `c5n.large` | 4771.84        | 0.528    |
   | `m5.large` - `c5n.large`  | 5048.32        | 0.602    |
   | `m5.large` - `t3.medium`  | 4741.12        | 0.230    |

   > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

> Within the same region, it is expected to have ~5Gbps and <1ms RTT between instances.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

   | Connection                | TCP b/w (Mbps) | RTT (ms) |
   | ------------------------- | -------------- | -------- |
   | N. Virginia - Oregon      | 493            | 62.736   |
   | N. Virginia - N. Virginia | 5068.8         | 0.296    |
   | Oregon - Oregon           | 5068.8         | 0.145    |

   > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

> Between regions VA and OR, the TCP bandwidth is ~500Mbps and RTT is ~60ms.
