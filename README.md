# MYRMIDON Detection Lab: Atomic Red Team → ELK Pipeline

Production-grade threat simulation and detection pipeline using simulated attacks ingested into ELK Stack. Demonstrates end-to-end: Attack execution → Sigma-inspired detection → Kibana visualization.

Built for enterprise SOCs: Mirrors CrowdStrike/Expel triage workflows with live simulation of attacks from Red Canary's Atomic Red Team (ART) and an independent Kali attack box.

For a live demo, DM me on LinkedIn and we'll get you set up with credentials to access and see log generation in real time.

## Quick Demo Flow
1. **Execute Attack**: On Windows VM "Invoke-AtomicTest T(Insert MITRE TTP).
2. **Ingest Logs**: Beats to Logstash → Elasticsearch indexing.
3. **Detect & Visualize**: Kibana dashboard auto-updates with alerts, timelines, and MITRE ATT&CK mapping.

### Screenshots
![Lab Flow Topology](screenshots/01_lab_flow_topology.png)

![Atomic Red Team Firing](screenshots/02_art_execution_T1059_003.png)

![MITRE TTPs in T1059.003](screenshots/03_mitre_ttps.png)

![Logs Successfully Shipped](screenshots/04_logs_firing.png)

![Full Raw Log](screenshots/05_raw_log.png)

![Raw Log Process Creation Details](screenshots/06_raw_log_process_creation.png)

![EventID Dashboard—Line Chart](screenshots/07_eventid_dashboard.png)

![New and Parent Process Dashboard—Heatmap](screenshots/08_process_heatmap.png)

![CLI Executions Dashboard—Table](screenshots/09_cli_dashboard.png)

![Live Web Demo Login Portal](screenshots/10_web_demo_login_portal.png)

![Web Demo Successful Login](screenshots/11_web_demo_login_success.png)

## Import My Kibana Dashboard
1. Download: [malicious-executions-dashboard.ndjson](kibana/processes_and_executions_dashboard.ndjson)
2. In Kibana: Stack Management → Saved Objects → Import → Upload → Check "Include related objects" → Import.

## Secure Auth Setup (ES 9.0.1)
- Service token: `elasticsearch-service-tokens.bat create elastic/kibana kibana-token`.
- kibana.yml: `elasticsearch.serviceAccountToken: [token]` — No logins, direct to enhanced dashboard (multi-event lines, parent-process heat).
- Green cluster: Replicas=0 + single-node (`discovery.type: single-node`) for 100% health.
- Web Accessible: Cloudflare Tunnel (demo.myrmidon-security.dev) — Zero ports, HTTPS, WAF; view events from anywhere.
- Live Demo: Available upon request, please DM me on LinkedIn. We'll set up credentials so you can watch log spikes in real time as attacks are run.

## Tech Stack
- **Atomic Red Team**: Attack simulations based on the MITRE ATT&CK framework, ability to execute specific TTPs
- **ELK Stack**: ES 9.0.1 (green single-node), Kibana (token auth, enhanced dashboard: Timeline spikes, process table, etc), Sysmon for log enrichment, Winlogbeat for Windows Security log shipment.
- **Sigma Rules**: Query conversions for easy detection of TTPs in log search.
