Configuration Templates

To deploy these configurations on a fresh Debian system:

    1. Copy suricata.conf to /etc/fail2ban/filter.d/.

    2. Append the [suricata] block to your /etc/fail2ban/jail.local.

    3. Ensure your log paths match /var/log/suricata/fast.log.

    4. Restart fail2ban: sudo systemctl restart fail2ban.
