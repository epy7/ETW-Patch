# 🚀 ETW-Patcher: Trampoline Evasion Toolkit (AMSI/ETW/Sysmon)

Welcome to **ETW-Patch**! This is a PoC (Proof of Concept) in C++ that uses the powerful **Trampoline Hooking** technique to disable or check the most common detection mechanisms in Windows.

Specifically, it targets:
- **AMSI** (Antimalware Scan Interface)
- **ETW** (Event Tracing for Windows)
- **NtTraceEvent** (commonly used for monitoring, such as Sysmon)

> ⚠️ **Disclaimer:** This tool is strictly for **research purposes** in offensive and defensive security. Use in unauthorized environments is illegal and unethical. Use responsibly.

---

## 💡 About Trampoline Hooking

Trampoline Hooking is an *in-process hooking* technique where the execution of a function is redirected.

1. The **prologue** (the first 12 bytes) of the target function is copied to a **newly allocated memory area**, the **Trampoline**.
2. A return `JMP` is added to the Trampoline, pointing to the original instruction right after the patch.
3. The beginning of the target function is replaced with a **single 5-byte `JMP`**, redirecting execution flow to the **Trampoline**.

**Note on Evasion:** In its current state, the patch modifies the target to always jump to the Trampoline, which **interrupts the normal execution of the target function** (e.g., `AmsiScanBuffer`), resulting in detection being disabled.

---

## ✨ Execution Commands

Run `ETW-Patcher.exe` from the command line, passing the desired command number:

| Command | Description | Target Functions |
| :---: | :--- | :--- |
| **0** | **ALL** (Default) | Applies the patch to **AMSI**, **ETW**, and **NtTraceEvent** simultaneously. |
| **1** | **AMSI** | Applies the patch to `AmsiScanBuffer` (amsi.dll). |
| **2** | **ETW** | Applies the patch to `EtwEventWrite` (ntdll.dll). |
| **3** | **SYSMON** | Applies the patch to `NtTraceEvent` (ntdll.dll). |
| **4** | **CHECK** | Checks whether the above functions are patched (looks for `0xE9` — the relative JMP opcode — in the first byte). |

### Usage Example

```bash
# Full Evasion Patch
.\ETW-Patcher.exe 0

# Disable AMSI only
.\ETW-Patcher.exe 1

# Disable ETW only
.\ETW-Patcher.exe 2

# Disable NtTraceEvent only (Sysmon)
.\ETW-Patcher.exe 3

# Check status (before or after patching)
.\ETW-Patcher.exe 4
```
---

## 🖼️ Image Examples

![image](https://raw.githubusercontent.com/137f/ETW-Patcher/refs/heads/main/ETW-Patcher/Images/0.png)  
![image](https://raw.githubusercontent.com/137f/ETW-Patcher/refs/heads/main/ETW-Patcher/Images/1.png)  
![image](https://raw.githubusercontent.com/137f/ETW-Patcher/refs/heads/main/ETW-Patcher/Images/2.png)  
![image](https://raw.githubusercontent.com/137f/ETW-Patcher/refs/heads/main/ETW-Patcher/Images/3.png)  

---


