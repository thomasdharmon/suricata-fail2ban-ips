---

# Automated Network Defense: Integrating Suricata IDS with Fail2ban and UFW Firewall

## 1. Executive Summary

This project demonstrates the implementation of an automated Intrusion Prevention System (IPS) by creating a dynamic feedback loop between **Suricata** (IDS) and **UFW** (Firewall). By leveraging **Fail2ban** to parse real-time alert logs, malicious or non-compliant network actors are automatically blocked at the kernel layer upon triggering specific signature thresholds. This architecture minimizes the time-to-resolution for network threats and eliminates the need for manual administrator intervention.

## 2. Architectural Overview

* **Traffic Inspection:** Suricata IDS running in passive monitoring mode, generating alerts to `fast.log`.
* **Log Parsing:** Fail2ban configured with a custom filter to normalize Suricata’s irregular timestamp/bracket structure.
* **Enforcement:** UFW acting as the management interface for `nftables` to inject instant kernel-level `REJECT` chains.

## 3. Implementation Details

### Configuration Files

* `configs/suricata.conf`: A high-precision regex filter optimized to handle microsecond timestamps and ignore non-actionable Layer 2 hex-dump logs.
* `configs/jail.local`: A hardened jail configuration utilizing the native UFW action backend.

### Key Roadblocks & Resolutions

* **Regex Calibration:** Initial parsing failed due to microsecond-precise timestamps. This was resolved by defining an explicit `datepattern` within the Fail2ban filter.
* **Decoder Events:** 278 "missed" log lines were identified as Layer 2 Decoder Events (Ethertype unknown). Because these lacked IP headers, the filter correctly ignored them, confirming high efficiency.
* **Firewall Hook Visibility:** Standard `iptables` commands failed to show rules because modern Debian systems utilize `nftables`. Verification was achieved by querying native `nft` tables and performing live atomic ban tests.

## 4. Operational Verification

### Regex Validation

Validating the custom Fail2ban regular expression against raw Suricata network logs:
[INSERT IMAGE: images/regex_validation.png]

### Live Threat Response

Forcing a manual ban of a test IP (1.1.1.1) to verify the internal Fail2ban state:
[INSERT IMAGE: images/active_ban.png]

### Firewall Enforcement

Verifying that UFW ingested the token and rejected the traffic at the kernel layer:
[INSERT IMAGE: images/ufw_enforcement.png]

## 5. Key Takeaways

* **Defense-in-Depth:** Successfully converted a passive network monitoring tool into an active defensive system.
* **System Troubleshooting:** Gained hands-on experience debugging log parsing, service exceptions, and firewall backend interactions on a Debian Linux environment.
* **Automation:** Engineered a robust pipeline that scales defensive actions without manual overhead.

---

### How does this look to you?

Does this cover everything you want to document, or is there a specific technical detail you want to add to the "Roadblocks" section to make it feel more personal to your experience? Once you are happy with this text, the next step is simply creating your account on GitHub and creating the repository!
