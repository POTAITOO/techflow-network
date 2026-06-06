# TechFlow Solutions – Network Test Results

## Connectivity Tests
| Test | Source | Destination | Expected | Result |
|------|--------|-------------|----------|--------|
| Same VLAN | PC3 (IT) | PC4 (IT) | Success | PASS |
| Inter-VLAN (allowed) | PC1 (Mgmt) | PC2 (Sales) | Success | PASS |
| Inter-VLAN (allowed) | PC3 (IT) | PC1 (Mgmt) | Success | PASS |
| ACL Block | PC2 (Sales) | PC1 (Mgmt) | Blocked | PASS |
| ACL Allow | PC2 (Sales) | PC3 (IT) | Success | PASS |

## ACL Explanation
ACL 100 is applied inbound on G0/0.20 (Sales subinterface). It denies all IP traffic from 192.168.20.0/24 to 192.168.10.0/24, then permits everything else. This prevents Sales from accessing Management while allowing all other inter-VLAN communication.

## Secret Mission - Server Segment Isolation Tests

This section documents the verification of the secure server segment (VLAN 40) access policies enforced by Access Control List 110 on R1.

### 1. IT Department (VLAN 30) to Server1 Ping Test
*   **Source**: PC3 (192.168.30.11)
*   **Destination**: Server1 (192.168.40.100)
*   **Expected Result**: Successful (Pings allowed by catch-all permit rule)
*   **Actual Result**: Success (0% packet loss)

### 2. IT Department (VLAN 30) to Server1 SSH Verification (Port 22)
*   **Source**: PC3 (192.168.30.11)
*   **Destination**: Server1 (192.168.40.100)
*   **Command**: `ssh -l admin 192.168.40.100`
*   **Expected Result**: Connection reached Server1 and was refused by the host itself (indicating the ACL successfully permitted the transit traffic).
*   **Actual Result**: `% Connection refused by remote host` (Instant return)

### 3. Sales Department (VLAN 20) to Server1 SSH Block (Port 22)
*   **Source**: PC2 (192.168.20.11)
*   **Destination**: Server1 (192.168.40.100)
*   **Command**: `ssh -l admin 192.168.40.100`
*   **Expected Result**: Dropped at R1 outbound interface (Connection timeout)
*   **Actual Result**: Connection timed out (Fails cleanly as expected)