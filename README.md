# AWS VPC Architecture Setup - Production & Development Networks

This repository documents the step-by-step implementation of a custom AWS VPC setup for a fictional company, **XYZ Corporation**, which requires isolated environments for **Production** and **Development** teams.

---

## 🧩 Problem Statement

Create and set up distinct VPCs for Production and Development networks with specified subnet layouts, EC2 instances, NAT gateway, internet access rules, and peering between VPCs.

---

## 📍 Architecture Overview

- **Region Used**: `us-east-1`
- **Availability Zones Used**: `us-east-1a`, `us-east-1b`, `us-east-1c`

---

## 🏗️ VPC Configuration Summary

### 🟦 Production VPC (`10.0.0.0/16`)
- **4-Tier Architecture**
- **Subnets**:
  - `web` (public) - 10.0.1.0/24 - `us-east-1a`
  - `app1` (private) - 10.0.2.0/24 - `us-east-1a`
  - `app2` (private) - 10.0.3.0/24 - `us-east-1b`
  - `dbcache` (private) - 10.0.4.0/24 - `us-east-1b`
  - `db` (private) - 10.0.5.0/24 - `us-east-1c`

### 🟨 Development VPC (`10.1.0.0/16`)
- **2-Tier Architecture**
- **Subnets**:
  - `web` (public) - 10.1.1.0/24 - `us-east-1a`
  - `db` (private) - 10.1.2.0/24 - `us-east-1b`

---

## 🚀 Key Steps Performed

1. **Created VPCs and Subnets** in multiple AZs.
2. **Attached Internet Gateway** for public subnet access.
3. **Created NAT Gateway** for selective internet access (e.g., `app1`, `dbcache`).
4. **Launched EC2 Instances** in each subnet, named per subnet.
5. **Configured Security Groups and NACLs** to secure traffic between tiers.
6. **Established VPC Peering** between Production and Development VPCs.
7. **Set up DB-to-DB Communication** across VPCs using peering + SG rules.

---

## 📈 Routing & Connectivity

| Source        | Destination    | Connectivity Allowed |
|---------------|----------------|----------------------|
| Web (Prod)    | Internet        | ✅ via IGW          |
| App1 (Prod)   | Internet        | ✅ via NAT GW       |
| DBCache (Prod)| Internet        | ✅ via NAT GW       |
| App2 (Prod)   | Internet        | ❌                  |
| DB (Prod)     | Internet        | ❌                  |
| Dev Web       | Internet        | ✅ via IGW          |
| Dev DB        | Internet        | ❌                  |
| Prod DB       | Dev DB          | ✅ via Peering      |

---

## 📸 Screenshots / Diagrams

- Added screenshots of:
  - VPC
  - Subnet configurations
  - Route tables
  - NAT Gateway
  - Peering connections
  - EC2 instances
