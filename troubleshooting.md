# Troubleshooting Log

Issues encountered while building the lab, with cause and fix.

## Phase 1 – Base config & SSH

### 1. `security passwords min-length` rejected on switches
- **Device:** HQ-SW1, BR-SW1 (Catalyst 2960)
- **Symptom:** `% Invalid input detected` when entering `security passwords min-length 10`.
- **Cause:** The 2960 IOS image in Packet Tracer does not support this command (it works on the ISR 4331 routers).
- **Fix:** Skipped the command on the switches; all switch passwords set to 10+ characters manually.

### 2. BR-R1 stuck on SSH version 1.99
- **Device:** BR-R1
- **Symptom:** `show ip ssh` showed `version 1.99` (SSHv1 and v2 both allowed) even after `ip ssh version 2`.
- **Cause:** The RSA key was too small. SSHv2 requires a key of at least 768 bits, so the router would not switch to v2 only.
- **Fix:**
crypto key zeroize rsa
crypto key generate rsa general-keys modulus 2048
ip ssh version 2

- **Verification:** `show ip ssh` now shows `SSH Enabled - version 2.0`.