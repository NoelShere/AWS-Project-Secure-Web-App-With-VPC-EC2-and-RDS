# Your Complete AWS Infrastructure Map

## All Resources You Created

---

## VISUAL ARCHITECTURE

```
┌──────────────────────────────────────────────────────────────────┐
│                          INTERNET                                │
│                     (Your Laptop, Users)                         │
└─────────────────────────────┬──────────────────────────────────┘
                              │
                    ┌─────────▼────────────┐
                    │ Internet Gateway     │
                    │ (my-project-igw)     │
                    │ igw-0efd2227a05eb313a
                    └─────────┬────────────┘
                              │
        ┌─────────────────────┴────────────────────────┐
        │                                              │
        │   ROUTE TABLE (public-route-table)          │
        │   ┌──────────────────────────────────────┐  │
        │   │ 0.0.0.0/0 → IGW (internet traffic)  │  │
        │   │ 10.0.0.0/16 → local (internal)      │  │
        │   └──────────────────────────────────────┘  │
        │                                              │
        │                                              │
┌───────┴───────────────────────────────────────────────────────────┐
│                     VPC (my-project-vpc)                          │
│                    CIDR: 10.0.0.0/16                             │
│                                                                   │
│  ┌────────────────────────┐   ┌──────────────────────────────┐  │
│  │  PUBLIC SUBNET         │   │  PRIVATE SUBNET              │  │
│  │  (public-subnet)       │   │  (private-subnet)            │  │
│  │  CIDR: 10.0.1.0/24     │   │  CIDR: 10.0.2.0/24           │  │
│  │  AZ: ca-central-1a     │   │  AZ: ca-central-1b           │  │
│  │                        │   │                              │  │
│  │  ┌──────────────────┐  │   │  ┌────────────────────────┐  │  │
│  │  │  EC2 Instance    │  │   │  │  RDS Database          │  │  │
│  │  │ (my-web-server)  │  │   │  │ (my-project-db)        │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ Instance ID:     │  │   │  │ Endpoint:              │  │  │
│  │  │ i-01357215d36d8  │  │   │  │ my-project-db1.c9q...  │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ Private IP:      │  │   │  │ Private IP:            │  │  │
│  │  │ 10.0.1.199       │  │   │  │ 10.0.2.x (auto)        │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ Public IP:       │  │   │  │ Public IP:             │  │  │
│  │  │ 99.79.33.57      │  │   │  │ NONE (private) ✓       │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ Instance Type:   │  │   │  │ Engine:                │  │  │
│  │  │ t2.micro         │  │   │  │ MySQL 8.0              │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ OS: Amazon       │  │   │  │ Storage:               │  │  │
│  │  │ Linux 2          │  │   │  │ 100 GB (default)       │  │  │
│  │  │                  │  │   │  │                        │  │  │
│  │  │ Availability:    │  │   │  │ Availability:          │  │  │
│  │  │ ca-central-1a    │  │   │  │ Multi-AZ ✓             │  │  │
│  │  └──────────────────┘  │   │  └────────────────────────┘  │  │
│  │           │            │   │             │                │  │
│  │  ┌────────▼──────────┐ │   │  ┌──────────▼──────────────┐ │  │
│  │  │ Security Group    │ │   │  │ Security Group         │ │  │
│  │  │ (ec2-sg)          │ │   │  │ (rds-sg)               │ │  │
│  │  │                   │ │   │  │                        │ │  │
│  │  │ Inbound:          │ │   │  │ Inbound:               │ │  │
│  │  │ • Port 80 from    │ │   │  │ • Port 3306 from       │ │  │
│  │  │   0.0.0.0/0 ✓     │ │   │  │   ec2-sg ✓             │ │  │
│  │  │ • Port 443 from   │ │   │  │                        │ │  │
│  │  │   0.0.0.0/0 ✓     │ │   │  │ Outbound: All allowed  │ │  │
│  │  │ • Port 22 from    │ │   │  │                        │ │  │
│  │  │   0.0.0.0/0 ✓     │ │   │  │                        │ │  │
│  │  │                   │ │   │  │                        │ │  │
│  │  │ Outbound:         │ │   │  │                        │ │  │
│  │  │ All allowed ✓     │ │   │  │                        │ │  │
│  │  └───────────────────┘ │   │  └────────────────────────┘ │  │
│  └────────────────────────┘   └──────────────────────────────┘  │
│                                                                   │
│           ┌──────────────────────────────────────────────┐       │
│           │  DB Subnet Group (my-project-subnet-group)   │       │
│           │  • Includes: public-subnet & private-subnet  │       │
│           │  • Purpose: Tells RDS which subnets it can   │       │
│           │    use for high availability                 │       │
│           └──────────────────────────────────────────────┘       │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘
```

---

## Resource List with Details

### 1. VPC (Virtual Private Cloud)

| Property | Value |
|---|---|
| **Name** | my-project-vpc |
| **CIDR Block** | 10.0.0.0/16 |
| **Region** | ca-central-1 |
| **State** | Available |
| **Purpose** | Your private network in AWS |
| **Connects To** | Internet Gateway, Subnets, Route Tables, Security Groups |

---

### 2. Internet Gateway

| Property | Value |
|---|---|
| **Name** | my-project-igw |
| **ID** | igw-0efd2227a05eb313a |
| **State** | Attached |
| **Attached To** | my-project-vpc |
| **Purpose** | Bridge between internet and your VPC |
| **Enables** | Internet access for public subnet resources |

---

### 3. Public Subnet

| Property | Value |
|---|---|
| **Name** | public-subnet |
| **CIDR Block** | 10.0.1.0/24 |
| **Availability Zone** | ca-central-1a |
| **VPC** | my-project-vpc |
| **Auto-assign Public IP** | Enabled |
| **Route Table** | public-route-table |
| **Route Table Contains** | 0.0.0.0/0 → IGW, 10.0.0.0/16 → local |
| **Resources** | EC2 instance |
| **Purpose** | Accessible from internet |

---

### 4. Private Subnet

| Property | Value |
|---|---|
| **Name** | private-subnet |
| **CIDR Block** | 10.0.2.0/24 |
| **Availability Zone** | ca-central-1b |
| **VPC** | my-project-vpc |
| **Auto-assign Public IP** | Disabled |
| **Route Table** | Default (local only) |
| **Resources** | RDS database |
| **Purpose** | Hidden from internet, internal only |

---

### 5. EC2 Instance (Web Server)

| Property | Value |
|---|---|
| **Name** | my-web-server |
| **Instance ID** | i-01357215d36d850ea |
| **Instance Type** | t2.micro |
| **AMI** | Amazon Linux 2023 |
| **State** | Running |
| **Subnet** | public-subnet (10.0.1.0/24) |
| **Private IP** | 10.0.1.199 |
| **Public IP** | 99.79.33.57 |
| **Availability Zone** | ca-central-1a |
| **Security Group** | ec2-security-group |
| **Key Pair** | my-project-key |
| **Status Checks** | 2/2 checks passed ✓ |
| **Installed Software** | Apache (httpd), PHP 8.x, php-mysqli |
| **Running Services** | Apache web server |
| **Connects To** | RDS (internally via 10.0.2.0/24) |
| **Accessible From** | Internet via port 80, 443, 22 |

---

### 6. RDS Database (MySQL)

| Property | Value |
|---|---|
| **Name** | my-project-db |
| **Identifier** | my-project-db1 |
| **Engine** | MySQL 8.0 |
| **Instance Class** | db.t3.micro |
| **Endpoint** | my-project-db1.c9qowme2sd80.ca-central-1.rds.amazonaws.com |
| **Port** | 3306 |
| **State** | Available |
| **Subnet** | private-subnet (10.0.2.0/24) |
| **Availability Zone** | ca-central-1b |
| **Security Group** | rds-security-group |
| **Public Access** | No ✓ |
| **Multi-AZ** | Yes |
| **Master Username** | admin |
| **Master Password** | noels123 |
| **Database Name** | myappdb |
| **DB Subnet Group** | my-project-subnet-group |
| **Connects From** | EC2 only (ec2-security-group) |
| **Accessible From Internet** | NO ✓ |

---

### 7. EC2 Security Group

| Property | Value |
|---|---|
| **Name** | ec2-security-group |
| **ID** | sg-0xxx |
| **VPC** | my-project-vpc |
| **Purpose** | Controls traffic to EC2 instance |

**Inbound Rules:**
```
Port 80 (HTTP)    from 0.0.0.0/0 ✓
Port 443 (HTTPS)  from 0.0.0.0/0 ✓
Port 22 (SSH)     from 0.0.0.0/0 ✓
```

**Outbound Rules:**
```
All traffic allowed ✓
```

---

### 8. RDS Security Group

| Property | Value |
|---|---|
| **Name** | rds-security-group |
| **ID** | sg-0xxx |
| **VPC** | my-project-vpc |
| **Purpose** | Controls traffic to RDS database |

**Inbound Rules:**
```
Port 3306 (MySQL) from ec2-security-group ✓
```

**Outbound Rules:**
```
All traffic allowed ✓
```

---

### 9. Route Tables

#### Public Route Table (public-route-table)

| Destination | Target | Status |
|---|---|---|
| 0.0.0.0/0 | igw-0efd2227a05eb313a | Active ✓ |
| 10.0.0.0/16 | local | Active ✓ |

**Associated Subnets:** public-subnet

---

#### Private Route Table (Default)

| Destination | Target | Status |
|---|---|---|
| 10.0.0.0/16 | local | Active ✓ |

**Associated Subnets:** private-subnet

---

### 10. DB Subnet Group

| Property | Value |
|---|---|
| **Name** | my-project-subnet-group |
| **VPC** | my-project-vpc |
| **Subnets Included** | public-subnet (ca-central-1a), private-subnet (ca-central-1b) |
| **Purpose** | Tells RDS which subnets it can use |
| **Status** | Active ✓ |

---

## HOW EVERYTHING CONNECTS

### Traffic Flow 1: SSH Login (Internet → EC2)

```
Your Laptop
    ↓ ssh -i key.pem ec2-user@99.79.33.57
    ↓ Port 22
Internet
    ↓
Internet Gateway (my-project-igw)
    ↓ Routes through IGW
    ↓
Route Table (public-route-table) checks:
    "Is 99.79.33.57 in 0.0.0.0/0?" YES → send through IGW
    ↓
Public Subnet (public-subnet)
    ↓
EC2 Security Group checks:
    "Is port 22 allowed from 0.0.0.0/0?" YES → allow
    ↓
EC2 Instance (my-web-server)
    ↓ Receives SSH connection
```

---

### Traffic Flow 2: Website Access (Internet → EC2 → Web Page)

```
User opens http://99.79.33.57
    ↓ Port 80
Internet
    ↓
Internet Gateway (my-project-igw)
    ↓
Route Table: 0.0.0.0/0 → IGW ✓
    ↓
Public Subnet
    ↓
EC2 Security Group: Port 80 allowed ✓
    ↓
EC2 Instance (Apache/PHP running)
    ↓
Apache serves /var/www/html/index.php
    ↓
PHP script runs
    ↓
Website displays in browser
```

---

### Traffic Flow 3: EC2 → RDS (Internal Database Query)

```
EC2 (10.0.1.199)
    ↓ Needs user data
    ↓ Connects to RDS on port 3306
    ↓
Route Table (public-route-table) checks:
    "Is 10.0.2.x in 10.0.0.0/16?" YES → local
    ↓ Stays inside VPC, never leaves
    ↓
Private Subnet (private-subnet)
    ↓
RDS Security Group checks:
    "Is port 3306 allowed from ec2-security-group?" YES → allow
    ↓
RDS Database (my-project-db)
    ↓ Returns user data to EC2
    ↓
EC2 displays data in login form
```

---

### Traffic Flow 4: EC2 → Internet (Software Updates)

```
EC2 (10.0.1.199)
    ↓ sudo yum update
    ↓ Needs to download packages from internet
    ↓
Route Table checks:
    "Is ubuntu.com in 10.0.0.0/16?" NO
    "Is it in 0.0.0.0/0?" YES → send to IGW
    ↓
Internet Gateway (my-project-igw)
    ↓
Internet
    ↓
Ubuntu package servers
    ↓ Send software
    ↓
Internet Gateway
    ↓
EC2 receives packages
```

---

## Security Summary

| Layer | Protection | Status |
|---|---|---|
| **Internet Gateway** | Controlled access to/from internet | ✓ Only allows defined traffic |
| **Route Tables** | Directs traffic correctly | ✓ Public subnet has internet route |
| **Security Groups - EC2** | Controls ports to EC2 | ✓ Only 22, 80, 443 allowed |
| **Security Groups - RDS** | Controls ports to database | ✓ Only 3306 from EC2 allowed |
| **Network Isolation** | RDS has no public IP | ✓ Cannot be reached from internet |
| **Subnet Isolation** | Public vs Private separation | ✓ EC2 public, RDS private |

---

## What Each Component Does

| Component | Job | Your Setup |
|---|---|---|
| **VPC** | Container for all resources | ✓ my-project-vpc (10.0.0.0/16) |
| **Internet Gateway** | Bridge to internet | ✓ my-project-igw attached |
| **Route Table** | GPS for traffic | ✓ Routes set up correctly |
| **Public Subnet** | Accessible from internet | ✓ public-subnet with EC2 |
| **Private Subnet** | Hidden from internet | ✓ private-subnet with RDS |
| **EC2** | Web server | ✓ Running Apache + PHP |
| **RDS** | Database | ✓ Running MySQL |
| **EC2 Security Group** | Firewall for EC2 | ✓ Allows web traffic |
| **RDS Security Group** | Firewall for RDS | ✓ Only allows EC2 |
| **DB Subnet Group** | Where RDS can live | ✓ Spans both subnets |

---

## Complete Dependency Chart

```
Internet
    └── Internet Gateway (my-project-igw)
            └── VPC (my-project-vpc)
                    ├── Route Table (public-route-table)
                    │   └── Public Subnet (public-subnet)
                    │       └── EC2 Security Group (ec2-sg)
                    │           └── EC2 Instance (my-web-server)
                    │               ├── Apache (port 80)
                    │               └── PHP (connects to RDS)
                    │
                    └── Route Table (default/private)
                        └── Private Subnet (private-subnet)
                            └── RDS Security Group (rds-sg)
                                └── DB Subnet Group (my-project-subnet-group)
                                    └── RDS Instance (my-project-db)
                                        └── MySQL Database (myappdb)
```

---

## Summary

**Total Resources Created: 10**

1. ✓ VPC (1)
2. ✓ Internet Gateway (1)
3. ✓ Subnets (2)
4. ✓ Route Tables (2)
5. ✓ EC2 Instance (1)
6. ✓ RDS Database (1)
7. ✓ Security Groups (2)
8. ✓ DB Subnet Group (1)
9. Key Pair (1)
10. Network Interfaces (auto-created)

**All working together to create a secure, scalable, production-like architecture.**
