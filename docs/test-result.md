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