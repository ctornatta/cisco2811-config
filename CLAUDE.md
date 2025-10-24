# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository captures and tracks the configuration of a Cisco 2811 router. The primary artifact is the router's IOS configuration stored in `cisco2811.txt`.

## Architecture Overview

### Configuration Structure

The `cisco2811.txt` file contains a Cisco IOS configuration (version 12.4) with the following key components:

- **Network Interfaces:**
  - `FastEthernet0/0`: WAN interface with DHCP client, configured for NAT outside
  - `FastEthernet0/1`: Currently shutdown
  - `FastEthernet1/0-1/15`: 16-port Fast Ethernet switch module (currently unconfigured)
  - `Loopback0`: Internal address space (172.16.0.0/24)
  - `Async0/1/0` and `Async0/2/0`: Dial-in modem interfaces with PPP encapsulation

- **Network Services:**
  - NAT overload on FastEthernet0/0 for internal 172.16.0.0/24 network
  - DHCP client on WAN interface
  - Dial-in pool: 172.16.0.10-172.16.0.12

- **Voice Configuration:**
  - 8 FXS voice ports (0/0/0-0/0/3 and 0/3/0-0/3/3)
  - Dial peers 100-103 and 200-203 mapped to respective ports

- **Security:**
  - Enable secret configured (hashed)
  - Username "dialup" for dial-in access
  - VTY lines accept telnet with password authentication

## Working with Cisco IOS Configurations

### Configuration Format

Cisco IOS configurations follow a hierarchical structure:
- Top-level commands (global configuration)
- Interface configurations (indented under `interface` keyword)
- Line configurations (indented under `line` keyword)
- Routing protocol configurations (indented under protocol keywords)

### Key Sections

- Lines starting with `!` are separators/comments
- `interface` blocks define network interface settings
- `line` blocks configure console, aux, and VTY access
- `access-list` entries define traffic filtering rules
- `dial-peer voice` blocks configure voice routing

### Branch Strategy

- `main`: Stable, tested configurations
- `feat/*`: Feature branches for adding/modifying configuration sections (e.g., `feat/ethernet` for configuring the FastEthernet1/0-15 switch module)

### Common Tasks

**Viewing configuration sections:**
```bash
grep -A 10 "^interface FastEthernet0/0" cisco2811.txt
grep -A 5 "^dial-peer voice" cisco2811.txt
```

**Validating configuration syntax:**
No automated syntax checker available in this repo. Configuration should be validated by loading onto a Cisco IOS device or simulator (e.g., GNS3, Packet Tracer, or physical hardware).

**Tracking changes:**
```bash
git diff main cisco2811.txt
```

## Important Configuration Details

- The FastEthernet1/0-15 interfaces (lines 85-116) are part of a 16-port switch module and currently have minimal configuration
- NAT is configured with overload (PAT) on the WAN interface
- Two async serial interfaces are configured for dial-in access using PPP and CHAP authentication
- Voice ports support analog phone connectivity with simple destination patterns (100-103, 200-203)
