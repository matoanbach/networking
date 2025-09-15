# CCNA 200-301 — Day 4  
## Introduction to the Cisco IOS CLI

---

## What is a CLI?
- **Command-line interface**
- The interface you use to configure Cisco devices
- Compared to GUI (Graphical User Interface)

---

## How to Connect to a Cisco Device (Console Port)
- **RJ45**
- **USB Mini-B**
- **Rollover cable** (pinouts shown)
- Terminal Emulator (e.g., PuTTY from putty.org)

---

## Cisco IOS CLI Modes

### User EXEC Mode
- Prompt: `Router>`
- Very limited access
- Can view some information, cannot change configuration
- Also called *user mode*

### Privileged EXEC Mode
- Command:  
```bash
Router> enable
Router#
```
- Full access to view configuration, restart device, save config
- Cannot fully change configuration

### Global Configuration Mode
- Prompt: `Router(config)#`
- Used to configure the device

---

## Passwords
- **enable password**
- Case-sensitive
- Does not display as you type
- **service password-encryption**
- Encrypts current & future passwords (weak encryption)
- Does not affect `enable secret`
- **enable secret**
- Uses MD5 encryption
- Stronger, recommended

---

## Running-config / Startup-config
- **Running-config**: current active config in RAM
- **Startup-config**: loaded from NVRAM at boot
- Commands:
- `show running-config`
- `show startup-config`
- Saving config:
- `write`
- `write memory`
- `copy running-config startup-config`

---

## Canceling Commands
- Use `no` in front of a command to remove it
```bash
Router(config)# no command
```

---

## Modes Review
- `Router>` → User EXEC mode
- `Router#` → Privileged EXEC mode
- `Router(config)#` → Global configuration mode

---

## Command Review
- `enable` → enter privileged EXEC mode
- `configure terminal` → enter global config mode
- `enable password password` → set password for privileged EXEC
- `service password-encryption` → encrypt passwords
- `enable secret password` → secure, encrypted password
- `do <command>` → run exec-level command from config mode
- `show running-config` / `show startup-config` → display configs
- `write` / `copy running-config startup-config` → save configs
