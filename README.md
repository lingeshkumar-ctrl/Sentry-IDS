# Sentry IDS | Real-Time Intrusion Detection & Prevention War Room

Sentry IDS is an advanced, standalone network security monitor (NSM) and active intrusion prevention system. It integrates directly with the **Suricata** engine, features multi-threaded asynchronous events handling, live rule-revocation databases, active dynamic firewall rules (`iptables` mitigation), and automated forensic snapshot capturing.

## Key Features
* ⚠️ **Dynamic Threat Intelligence:** Instantly parses network signatures, mapping them to High, Medium, and Low severity color-coded alerts.
* 🌐 **Geo-IP API Enrichment:** Real-time, keyless background queries resolving malicious attacker country & ISP data on-the-fly.
* ⚙️ **Active Mitigation (IPS Firewall Jail):** Dynamically drops high-severity attacker IPs at the Linux kernel level and allows real-time unblocking.
* 📝 **Forensic Snapshotting:** Automatically launches localized packet captures (`.pcap`) upon high-severity triggers for deep analysis.
* 📊 **Executive Forensic Reporting:** Generates audit-compliant corporate PDF reports of the session metrics under standard copyrights.

---

## System Operational Flow

```text
                     SENTRY IDS/IPS SYSTEM ARCHITECTURE
                     
     [ LAN / Network Interface (eth0) ] ---> Raw Packets Inbound
                    |
                    v
       [ Suricata Daemon (Engine) ] <--- [ Standalone Rules Engine (standalone.rules) ]
                    |
                    v
         Does Traffic Match Policy?
             /             \
       (YES)                (NO) ---> Quiet Logging (http / dns analytics logs)
         |
         v
  [ Multithreaded Async Event Broker ] ---> Funnels events cleanly to avoid UI thread lock
    /            |             \
   v             v              v
[Geo-IP Check]  [Automated IPS] [Forensic Snapshot Engine]
   |             |               |
   v             v               v
Query API for   Drop Attacker   Invoke TCPdump;
Country/ISP     IP via IPTables capture 100 packets
   |             |               |
   v             v               v
   =================================================
                    |
                    v
    [ Sentry UI Desktop Console (CustomTkinter) ]
         |
         v
    [ Export Forensic Audit Executive Report (.pdf) ]
