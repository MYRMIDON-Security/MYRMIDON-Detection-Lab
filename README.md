# MYRMIDON Detection Lab: Atomic Red Team → ELK Pipeline

Production-grade threat simulation and detection pipeline using Atomic Red Team (ART) attacks ingested into ELK Stack. Demonstrates end-to-end: Attack execution → Sigma-inspired detection → Kibana visualization.

Built for enterprise SOCs: Mirrors CrowdStrike/Expel triage workflows with live T1059.003 (Command and Scripting Interpreter: PowerShell) simulation.

## Quick Demo Flow
1. **Execute Attack**: On Windows VM — `Invoke-AtomicTest T1059.003 -Verbose` (Execution --> Command and Scripting Interpreter --> PowerShell / Windows Command Shell).
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
3. Assumes index pattern: `logs-*` (tweak if needed).

## Secure Auth Setup (ES 9.0.1)
- Service token: `elasticsearch-service-tokens.bat create elastic/kibana kibana-token`.
- kibana.yml: `elasticsearch.serviceAccountToken: [token]` — No logins, direct to enhanced T1059 dashboard (multi-event lines, parent-process heat).
- Green cluster: Replicas=0 + single-node (`discovery.type: single-node`) for 100% health.
- Global: Cloudflare Tunnel (demo.myrmidon-security.dev) — Zero ports, HTTPS, WAF; view events from anywhere.
- Live Demo (LAN: 192.168.1.75): http://192.168.1.75:5601 — Token auto-auth; run ART, watch 4688 spikes vs baseline.

## Tech Stack
- **Atomic Red Team**: MITRE ATT&CK sims (T1059.003 tested — cmd.exe spawns, 4688 events).
- **ELK Stack**: ES 9.0.1 (green single-node), Kibana (token auth, enhanced dashboard: Timeline spikes, MITRE pie, process table, anomaly heat), Winlogbeat for Windows Security logs.
- **Detections**: (COMING SOON) 423 Sigma rules incoming (top Windows process_creation for execution tactics — auto-flag T1059 payloads).
- **Playbooks**: (COMING SOON) IR guides for top alerts (T1059.003: Triage to remediation in 20 mins, with SOAR scale).