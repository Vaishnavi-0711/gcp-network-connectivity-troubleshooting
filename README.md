# Google Cloud Network Intelligence Center: Connectivity Troubleshooting

**Project Domain:** Cloud Computing and Network Security  
**Platform:** Google Cloud Platform (GCP)  
**Tools:** Compute Engine, Virtual Private Cloud (VPC), Firewall Rules, Network Intelligence Center — Connectivity Tests

---

## 1. Project Overview

This project demonstrates network connectivity troubleshooting using Google Cloud Network Intelligence Center. It investigates communication between two virtual machine instances and demonstrates how firewall rules can affect network reachability.

A custom VPC network and subnet were configured, and a TCP connectivity test was performed between a source VM and a destination VM. A temporary deny firewall rule was introduced to simulate a network failure. The Connectivity Test results were then used to identify the firewall-related packet drop. After removing the temporary deny rule, connectivity was tested again.

## 2. Objectives

- Create and configure a custom VPC network and subnet.
- Deploy source and destination virtual machine instances.
- Configure ingress firewall rules for TCP port 80.
- Analyze network reachability using Connectivity Tests.
- Simulate a firewall-related connectivity failure.
- Diagnose the failure using connectivity analysis results.
- Restore connectivity and verify the final result.

## 3. Tools and Technologies

| Technology | Purpose |
|---|---|
| Google Cloud Platform | Cloud infrastructure |
| Compute Engine | Virtual machine instances |
| Virtual Private Cloud (VPC) | Network isolation and connectivity |
| VPC Firewall Rules | Control ingress traffic |
| Network Intelligence Center | Network connectivity analysis |
| TCP | Transport protocol |
| Nginx | Web server configured on the destination VM |

## 4. Network Configuration

| Component | Configuration |
|---|---|
| VPC network | `nic-troubleshooting-vpc` |
| Subnet | `nic-subnet` |
| Source VM | `source-vm` |
| Destination VM | `destination-vm` |
| Network tag | `web-server` |
| Protocol | TCP |
| Destination port | 80 |
| Connectivity Test | `test-source-to-destination` |
| Allow firewall rule | `allow-web-server` |
| Temporary deny rule | `deny-web-server-test` |

## 5. Implementation Procedure

### Step 1: Enable APIs

Enabled the required Google Cloud APIs for Compute Engine and Network Management.

### Step 2: Create the VPC Network

Created a custom VPC network named `nic-troubleshooting-vpc` and a subnet named `nic-subnet`.

### Step 3: Create Virtual Machines

Created two VM instances: `source-vm` and `destination-vm`. The destination VM was configured with the `web-server` network tag and a startup script to install and start Nginx.

### Step 4: Configure Firewall Rules

Created an ingress allow rule named `allow-web-server` to permit TCP traffic on port 80 from the configured source range to the tagged destination VM.

### Step 5: Create a Connectivity Test

Created a Connectivity Test named `test-source-to-destination` using TCP port 80, with `source-vm` as the source and `destination-vm` as the destination.

### Step 6: Simulate a Firewall Failure

Created a temporary deny firewall rule named `deny-web-server-test` with priority 900 to block matching ingress traffic on TCP port 80.

### Step 7: Analyze the Failure

Reran the Connectivity Test. The test reported the path as unreachable and showed 0 out of 50 packets delivered. The trace identified the temporary deny rule as a possible point of packet drop.

### Step 8: Restore Connectivity

Removed the temporary deny firewall rule while retaining the original allow rule. Reran the Connectivity Test and verified that connectivity was reachable again.

## 6. Results and Evidence

### Test 1: Initial Connectivity

The Connectivity Test reported the source-to-destination path as reachable.

![Initial Connectivity Test](screenshots/01-initial-connectivity.png)

### Test 2: Firewall Failure

The temporary deny firewall rule caused the test to report unreachable connectivity, with 0/50 packets delivered. The trace showed the firewall rule associated with the packet drop.

![Firewall Blocked Test](screenshots/02-firewall-blocked.png)

### Test 3: Connectivity Restored

After the temporary deny rule was removed, the Connectivity Test was rerun and reported successful reachability.

![Connectivity Restored](screenshots/03-connectivity-restored.png)

## 7. Key Learnings

- Understanding VPC networks, subnets, and VM network interfaces.
- Configuring firewall rules and network tags.
- Using Connectivity Tests to analyze network paths.
- Interpreting reachability and packet-delivery results.
- Diagnosing firewall-related network failures.
- Validating remediation by rerunning connectivity tests.
- Cleaning up cloud resources after completing an experiment.

## 8. Resource Cleanup

After the experiment, the temporary deny firewall rule and cloud resources that were no longer needed were deleted to reduce unnecessary resource usage.

## 9. Conclusion

This project demonstrates a structured approach to troubleshooting network connectivity in Google Cloud. By using Connectivity Tests, a firewall-related failure was simulated, analyzed, and resolved. The experiment provided practical experience with cloud networking, firewall configuration, connectivity analysis, and troubleshooting workflows.
