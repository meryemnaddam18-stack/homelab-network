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
  ```
  crypto key zeroize rsa
  crypto key generate rsa general-keys modulus 2048
  ip ssh version 2
  ```
- **Verification:** `show ip ssh` now shows `SSH Enabled - version 2.0`.

## Phase 2 – VLANs & trunking

### 3. Admin-PC could not ping HQ-SW1 (192.168.99.2)
- **Device:** HQ-SW1, Admin-PC
- **Symptom:** Pings from Admin-PC to the switch management IP timed out, even though `Vlan99` was up/up.
- **Cause:** Admin-PC was cabled to Fa0/4 instead of Fa0/21 (from the port map). Fa0/4 was still in VLAN 1, so the PC was not in the management VLAN. Guest-PC1 was also on Fa0/3 instead of Fa0/11.
- **Why `Vlan99` looked healthy:** the trunk to HQ-R1 carries VLAN 99, which keeps the SVI up/up even with no access port in that VLAN.
- **Fix:**
  ```
  interface fa0/4
   switchport mode access
   switchport access vlan 99
   spanning-tree portfast
  ```
  Port map in the README updated to match the real cabling (Guest on Fa0/3, Admin on Fa0/4).
- **Verification:** `show vlan brief` lists Fa0/4 under VLAN 99; Admin-PC pings and SSHs to 192.168.99.2.
- **Lesson:** `show interfaces status` shows which ports are actually connected. Check it against the port map before configuring.

### 4. Admin-PC default gateway set to the switch
- **Device:** Admin-PC
- **Symptom:** None yet, but the gateway was `192.168.99.2` (HQ-SW1).
- **Cause:** Typo in the static IP configuration. A Layer 2 switch does not route.
- **Fix:** Gateway changed to `192.168.99.1`, the HQ-R1 subinterface for VLAN 99.

### 5. `terminal length 0` rejected on switches
- **Device:** HQ-SW1, BR-SW1 (Catalyst 2960)
- **Symptom:** `% Invalid input detected`; `terminal ?` only lists `history` and `monitor`.
- **Cause:** Not supported by the 2960 image in Packet Tracer.
- **Workaround:** Configs exported with Packet Tracer's **Config → Settings → Running Config → Export** instead of copying `show running-config`.

## Phase 6 – NAT

### 6. Pings to 8.8.8.8 stopped at the ISP router
- **Device:** HQ-R1
- **Symptom:** In simulation mode, pings reached the ISP but no reply came back. `show ip nat translations` was empty.
- **Diagnosis:** `show ip nat statistics` showed **Outside Interfaces** empty, G0/0/2 listed as an inside interface, and no dynamic mappings.
- **Cause:** G0/0/2 (the ISP link) was set to `ip nat inside` instead of `outside`, and the PAT rule was missing. Packets left HQ with private source addresses, and the ISP has no route back to 192.168.0.0, so it dropped the replies.
- **Fix:**
  ```
  interface g0/0/2
   no ip nat inside
   ip nat outside
  ip nat inside source list NAT-INSIDE interface g0/0/2 overload
  ```
- **Verification:** `show ip nat statistics` lists G0/0/2 as outside; `show ip nat translations` shows inside hosts from HQ and the branch translated to 203.0.113.2.

## Repository workflow

### 7. Packet Tracer file not in the repository
- **Symptom:** `git status` showed nothing to commit after placing devices.
- **Cause:** The `.pkt` was saved outside the repo folder (Packet Tracer's default location).
- **Fix:** Saved with **File → Save As** into `topology/homelab.pkt`, then committed.

### 8. `git push` rejected
- **Symptom:** `! [rejected] main -> main (fetch first)`.
- **Cause:** A change made directly on GitHub was not in the local copy.
- **Fix:** `git pull --rebase`, then `git push`. Now pulling before starting work after any edit on the website.
