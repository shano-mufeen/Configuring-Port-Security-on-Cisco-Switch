# 🔐 Cisco Port Security Configuration | Cisco Packet Tracer

## 📌 Project Overview

This project demonstrates the configuration and testing of **Port Security** on a Cisco Layer 2 switch using **Cisco Packet Tracer**.

Port Security helps control which devices are allowed to access switch ports by limiting the number of MAC addresses that can be learned on a port and defining what happens when an unauthorized device is detected.

The lab covers three Port Security violation modes:

* **Shutdown**
* **Restrict**
* **Protect**

It also demonstrates two methods of learning MAC addresses:

* **Sticky MAC**
* **Static MAC Address**

---

# 🎯 Project Objectives

The main objectives of this lab are to:

* Understand Cisco Port Security.
* Limit the number of devices allowed on a switch port.
* Configure Port Security on access ports.
* Configure maximum MAC addresses per port.
* Configure Sticky MAC learning.
* Configure static MAC address assignment.
* Configure Port Security violation modes.
* Test unauthorized device access.
* Verify Port Security using Cisco IOS commands.

---

# 🖥️ Lab Topology

The project uses multiple PCs connected to a Cisco Layer 2 switch.

```text
                 +----------------+
                 |     Switch     |
                 +----------------+
                  |  |  |  |  |
                 PC PC PC PC PC
```

Different switch ports are configured with different Port Security policies to demonstrate the behavior of each violation mode.

---

# 🔐 Port Security Concepts

By default, switch interfaces can allow devices to connect and communicate through the network.

Port Security provides a method of restricting access to a switch port based on **MAC addresses**.

It can be used to:

* Limit the number of MAC addresses learned on a port.
* Specify which MAC addresses are allowed.
* Define the action taken when a violation occurs.

---

# ⚙️ Port Security Configuration

## 1️⃣ Configure the Interface as an Access Port

Port Security is configured on an access interface.

```cisco
interface fastEthernet 0/1
switchport mode access
```

---

## 2️⃣ Enable Port Security

```cisco
switchport port-security
```

This enables Port Security on the interface.

---

# 🔢 3️⃣ Configure Maximum MAC Addresses

The maximum number of devices allowed on a port can be configured.

### One Device

```cisco
switchport port-security maximum 1
```

This allows a maximum of **one MAC address** on the port.

### Two Devices

```cisco
switchport port-security maximum 2
```

This allows a maximum of **two MAC addresses** on the port.

---

# 🧲 4️⃣ Configure Sticky MAC Learning

Sticky MAC allows the switch to dynamically learn MAC addresses and associate them with the port.

```cisco
switchport port-security mac-address sticky
```

The learned MAC address becomes part of the Port Security configuration.

---

# 🛑 5️⃣ Configure Violation Modes

Cisco Port Security supports different actions when a violation occurs.

The lab demonstrates:

```text
Shutdown
Restrict
Protect
```

---

## 🔴 Shutdown Mode

Configuration:

```cisco
switchport port-security violation shutdown
```

When a Port Security violation occurs, the interface is placed into a shutdown/error-disabled state.

### Lab Configuration

```cisco
interface range fastEthernet 0/1 - 2
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
```

### Policy

```text
Maximum Devices: 1
MAC Learning: Sticky
Violation Mode: Shutdown
```

---

# 🟠 Restrict Mode

Restrict mode allows the port to remain operational while unauthorized traffic is restricted.

### Lab Configuration

```cisco
interface range fastEthernet 0/3 - 4
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
```

### Policy

```text
Maximum Devices: 2
MAC Learning: Sticky
Violation Mode: Restrict
```

---

# 🟢 Protect Mode

Protect mode silently drops traffic from unauthorized MAC addresses while the port remains operational.

For the third configuration, the lab uses:

```text
Maximum Devices: 1
MAC Learning: Static
Violation Mode: Protect
```

Example configuration:

```cisco
interface fastEthernet 0/5
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address <MAC_ADDRESS>
switchport port-security violation protect
```

Replace `<MAC_ADDRESS>` with the MAC address of the authorized PC.

---

# 📊 Port Security Configuration Summary

| Interface | Maximum Devices | MAC Learning | Violation Mode |
| --------- | --------------: | ------------ | -------------- |
| Fa0/1     |               1 | Sticky       | Shutdown       |
| Fa0/2     |               1 | Sticky       | Shutdown       |
| Fa0/3     |               2 | Sticky       | Restrict       |
| Fa0/4     |               2 | Sticky       | Restrict       |
| Fa0/5     |               1 | Static       | Protect        |

---

# 🧪 Testing the Configuration

Before testing Port Security, the switch needs to learn the MAC addresses of the connected devices.

The MAC address table can be checked using:

```cisco
show mac address-table
```

After the legitimate devices have communicated with each other, the switch learns their MAC addresses.

---

# 🔍 Verify Port Security

Use:

```cisco
show port-security
```

You can also verify a specific interface:

```cisco
show port-security interface fastEthernet 0/1
```

This allows you to check:

* Port Security status
* Maximum MAC addresses
* Current secure MAC addresses
* Violation mode
* Violation count

---

# 🧪 Unauthorized Device Test

The lab tests what happens when a different device is connected to a port that is configured to allow only a specific number of devices.

Example:

```text
Authorized PC
     │
     ▼
Fa0/5
     │
     ▼
  Switch
     │
     ▼
Traffic Allowed ✅
```

Then the authorized PC is disconnected and another device is connected to the same port.

```text
Unauthorized PC
      │
      ▼
   Fa0/5
      │
      ▼
    Switch
      │
      ▼
Port Security Violation
```

Because the port is configured with Port Security, the unauthorized device's traffic is restricted according to the configured violation mode.

---

# 🧠 Key Concepts Learned

### Maximum MAC Address

Defines how many secure MAC addresses can be associated with a switch port.

```cisco
switchport port-security maximum 1
```

### Sticky MAC

Allows the switch to dynamically learn the MAC address and associate it with the secure port.

```cisco
switchport port-security mac-address sticky
```

### Static MAC

Allows an administrator to manually specify the authorized MAC address.

```cisco
switchport port-security mac-address <MAC_ADDRESS>
```

### Violation Mode

Determines what the switch does when an unauthorized MAC address is detected.

```text
Shutdown → Port is disabled
Restrict → Unauthorized traffic is restricted
Protect  → Unauthorized traffic is dropped
```

---

# 🔍 Useful Verification Commands

### Display MAC Address Table

```cisco
show mac address-table
```

### Display Port Security Status

```cisco
show port-security
```

### Display Port Security for an Interface

```cisco
show port-security interface fastEthernet 0/1
```

### Display Running Configuration

```cisco
show running-config
```

---

# 💻 Complete Configuration Examples

## Shutdown + Sticky

```cisco
interface range fastEthernet 0/1 - 2
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
```

## Restrict + Sticky

```cisco
interface range fastEthernet 0/3 - 4
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
```

## Protect + Static MAC

```cisco
interface fastEthernet 0/5
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address <MAC_ADDRESS>
switchport port-security violation protect
```

---

# 🎯 Skills Demonstrated

* Cisco Packet Tracer
* Cisco IOS CLI
* Layer 2 Switch Security
* Port Security
* MAC Address Security
* Sticky MAC
* Static MAC Configuration
* MAC Address Table Verification
* Shutdown Violation Mode
* Restrict Violation Mode
* Protect Violation Mode
* Access Port Configuration
* Network Security Testing
* Troubleshooting

---

# 🚀 Project Outcome

Successfully configured and tested **Cisco Port Security** using different maximum-device limits, MAC learning methods, and violation modes.

The lab demonstrated how switch ports can be controlled using:

```text
Port Security
     │
     ├── Maximum MAC Addresses
     │
     ├── Sticky MAC
     │
     ├── Static MAC
     │
     └── Violation Modes
           ├── Shutdown
           ├── Restrict
           └── Protect
```

This project strengthened practical understanding of **Layer 2 access control and switch security** using Cisco IOS.

---

## 📁 Project Type

**Cisco Packet Tracer — Layer 2 Network Security Lab**

### 🔐 Focus

**Port Security | MAC Address Security | Sticky MAC | Static MAC | Shutdown | Restrict | Protect**
