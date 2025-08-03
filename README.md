# Yatesbury: A Benchmark for East-West Network Security

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This dataset serves as a benchmark for evaluting the performance and efficiency of anomaly detectors in east-west data center network traffic. Detailed information about the benchmark can be found in our [NetVigil paper](https://www.microsoft.com/en-us/research/publication/netvigil-robust-and-low-cost-anomaly-detection-for-east-west-data-center-security/).

The dataset includes 20 distinct scenarios, each designated as an attack or a normal operation. For each scenario, a web-based [e-commerce application](https://github.com/GoogleCloudPlatform/microservices-demo) is utilized to generate normal traffic patterns. Simultaneously, attacks are carried out using one or more compromised, malicious nodes. Traffic traces, sourced from [NSG flow logs](https://learn.microsoft.com/en-us/azure/network-watcher/nsg-flow-logs-overview), are processed and converted into CSV files containing only the relevant properties.  


## Dataset Description

The dataset is available for download from Azure Blob Storage via [this link](Dataset.txt). For efficient data transfer, we suggest using `wget -i Dataset.txt` to download the dataset. The dataset is compressed in a `.tar.gz` format and each file represents a distinct scenario.

After decompression, each folder corresponds to either a normal or an attack scenario and includes two files: `nsg.csv` and `label.csv`, as well as a `ztn_fe_graphs` folder. For attack scenarios, there is also a `netvigil_scores.csv` file. The schema for these files and folders is as follows:  

### nsg.csv

1. `time`: Time in UTC when the event was logged.
2. `Source IP`: Source IP address.
3. `Destination IP`: Destination IP address.
4. `Source port`: Source port.
5. `Destination port`: Destination port.
6. `Protocol`: Protocol of the flow. Valid values are `T` for TCP and `U` for UDP.
7. `Traffic flow`: Direction of the traffic flow. Valid values are `I` for inbound and `O` for outbound.
8. `Traffic decision`: Whether traffic was allowed or denied. Valid values are `A` for allowed and `D` for denied.
9. `Flow State`: State of the flow. Possible states are:
    * `B`: Begin, when a flow is created. Statistics aren't provided.
    * `C`: Continuing for an ongoing flow. Statistics are provided at 5-minute intervals.
    * `E`: End, when a flow is terminated. Statistics are provided.
10. `Packets sent`: Total number of TCP packets sent from source to destination since the last update.
11. `Bytes sent`: Total number of TCP packet bytes sent from source to destination since the last update. Packet bytes include the packet header and payload.
12. `Packets received`: Total number of TCP packets sent from destination to source since the last update.
13. `Bytes received`: Total number of TCP packet bytes sent from destination to source since the last update. Packet bytes include packet header and payload.


### label.csv

1. `Source IP`: Source IP address.
2. `Destination IP`: Destination IP address.
3. `time`: The starting time of each 2-minute window.  
4. `label`: A `0` indicates normal operation for this IP pair during the 2-minute window, while a `1` denotes the presence of an attack.  

### ztn_fe_graphs

For each time window (e.g., 2 mins), there will be a PKL and CSV file. The PKL file represents the featurized graph and can be loaded with `pickle.load()`. The CSV file contains the edge list and the edge features. These files represent the output of the NetVigil feature extractor.

### netvigil_scores.csv

1. `MinT`: The starting time of each 2-minute window
2. `LocalIP`: Source IP address
3. `RemoteIP`: Destination IP address
4. `Label`: A `0` indicates normal operation for this IP pair during the 2-minute window, while a `1` denotes the presence of an attack.
5. `Score`: The NetVigil score (reconstruction loss) for this IP pair in the 2-minute window


## Attack Scenarios

| Attack Description                  | Description                                                                                      | # flows | Ratio malicious |  
|-------------------------------------|--------------------------------------------------------------------------------------------------|---------|-----------------|  
| Vertical Port Scan                  | Run an exhaustive scan of open ports                                                             | 1429    | 0.0265          |  
| SYN Flood DoS attack                | DoS attack where connections are rapidly initialized but not completed                           | 2817    | 0.0184          |  
| SYN Flood DDoS                      | DoS attack where connections are rapidly initialized but not completed (multiple attackers)      | 2437    | 0.0439          |  
| UDP DDoS                            | DoS attack with UDP packets (multiple attackers)                                                 | 1473    | 0.0081          |  
| Distributed Stealth Port Scan       | Run a targeted stealth scan of several key ports across many nodes with SYN packets              | 4069    | 0.0058          |  
| Distributed Port Scan               | Run a targeted scan of several key ports across many nodes                                       | 4054    | 0.0051          |  
| Distributed UDP Port Scan           | Run a targeted stealth scan of several keys across many nodes with UDP packets                    | 4319    | 0.0050          |  
| Infection Monkey 1                  | Scans key ports and launches network exploits                                                    | 2768    | 0.0122          |  
| Infection Monkey 2                  | Scans key ports and launches network exploits (target limited number of hosts)                   | 1490    | 0.0107          |  
| Infection Monkey 3                  | Scans key ports and launches network exploits (mount limited number of exploits)                 | 4677    | 0.0027          |  
| C&C communication                   | Compromised nodes receive commands, heartbeats, and file updates from C&C server                 | 2163    | 0.0254          |  
| DNS amplification                   | Attackers send DNS requests and direct responses to victim                                       | 4410    | 0.0825          |  
| SQL Injection                   | Attackers access the database with SQL injections                                       | --    | --          |  
| Unauthorized Database Access                   | Attackers access the database without  authorization                                       | --    | --          |  

## Reference Paper

If you use our benchmark in your work, we would appreciate a reference to the following paper:

Kevin Hsieh*, Mike Wong*, Santiago Segarra, Sathiya Kumaran Mani, Trevor Eberl, Anatoliy Panasyuk, Ravi Netravali, Ranveer Chandra, and Srikanth Kandula. [NetVigil: Robust and Low-Cost Anomaly Detection for East-West Data Center Security](https://www.microsoft.com/en-us/research/publication/netvigil-robust-and-low-cost-anomaly-detection-for-east-west-data-center-security/). *USENIX Symposium on Networked Systems Design and Implementation (NSDI), 2024*.


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
