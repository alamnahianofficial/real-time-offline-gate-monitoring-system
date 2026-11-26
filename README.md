# Real-Time Offline Gate Monitoring System

<sub>Full case study: <a href="https://nahian.ami.bd/">nahian.ami.bd</a></sub>

<p align="center">
  <img src="Assets/00-cover.png" alt="Real-Time Offline Gate Monitoring System">
</p>

This is a fully offline, edge-deployed computer vision and facility security platform that automates aggregate pedestrian and vehicle crossing counts at controlled facility access gates. Designed with strict privacy-preserving principles, the system captures verified visual evidence snapshots, records every crossing in a cryptographically tamper-evident audit log, and delivers shift-by-shift compliance reporting. The platform operates without facial recognition, biometrics, or persistent personal identification.

---

## System Interface Gallery

<sub>Click any screenshot to open the full-length page.</sub>

### Live Operational Dashboard & Kiosk View
<table><tr>
<td valign="top" width="50%"><a href="Assets/02-live-dashboard.png"><img src="Assets/preview/02-live-dashboard.png" alt="Operational Live Dashboard" width="100%"></a></td>
<td valign="top" width="50%"><img src="Assets/03-kiosk-view.png" alt="Guard Post Kiosk View" width="100%"></td>
</tr></table>

- **Operational Live Dashboard:** Real-time monitoring hub reporting current-shift and daily cumulative counts of humans, vehicles, and two-wheelers across individual gate boundaries (Main Gate, Side Gate) and aggregated facility totals. Features a low-latency live video feed with detection bounding box overlays, line-crossing vectors, and a streaming log of recent crossings with thumbnail evidence.
- **Guard Post Kiosk View:** A clean, distraction-free wall display interface optimized for security guard posts and gate control rooms. Maximizes visibility of real-time directional movement, active lane statuses, and system operational health without administrative controls.

### Event Logging & Supervisor Verification Queue
<table><tr>
<td valign="top" width="50%"><img src="Assets/04-event-log.png" alt="Audited Event Log" width="100%"></td>
<td valign="top" width="50%"><img src="Assets/05-review-queue.png" alt="Supervisor Review Queue" width="100%"></td>
</tr></table>

- **Audited Event Log:** Filterable historical registry of every crossing event containing timestamp, gate identifier, direction (In / Out), object classification (Person, Car, Two-Wheeler), detection confidence score, validation status, and visual evidence photo.
- **Supervisor Review Queue:** A dedicated human-in-the-loop verification workspace where ambiguous or low-confidence line crossings (such as occluded trajectories or incomplete transits) are queued for supervisor confirmation, rejection, or manual classification adjustment, with every decision captured in the audit trail.

### Traffic Analytics & Shift Reporting
<table><tr>
<td valign="top" width="50%"><img src="Assets/06-analytics.png" alt="Hourly Traffic Analytics" width="100%"></td>
<td valign="top" width="50%"><img src="Assets/07-shift-reports.png" alt="Automated Shift Reports" width="100%"></td>
</tr></table>

- **Flow Analytics:** Hourly breakdown of facility entries and exits by classification category across selectable time ranges (last 24 hours, week, month), accompanied by honest tracking coverage metrics indicating uptime versus downtime.
- **Automated Shift Reports:** Automated Excel and structured report generator compiling total traffic distributions, peak throughput hours, and gate utilization across morning, evening, and night operational shifts.

### Camera Management, Gate Geometry & Advanced Calibration
<table><tr>
<td valign="top" width="50%"><img src="Assets/09-camera-setup.png" alt="Network Camera Setup" width="100%"></td>
<td valign="top" width="50%"><a href="Assets/10-gate-setup.png"><img src="Assets/preview/10-gate-setup.png" alt="Interactive Gate Configuration" width="100%"></a></td>
</tr></table>
<p align="center">
  <img src="Assets/11-calibration.png" alt="Advanced Versioned Calibration">
</p>

- **Camera Discovery & Configuration:** Scans local subnets for IP camera streams or connects directly via RTSP URLs. Camera authentication credentials are encrypted in local memory and are never written to plain-text logs or databases.
- **Interactive Gate Line Setup:** Visual canvas allowing operators to draw gate boundaries, directional counting lines, class-specific lanes, and restricted alert zones directly on the live camera picture. Provides dynamic controls for detection sensitivity, shift schedules, and GPU/CPU compute selection.
- **Advanced Calibration Engine:** Precision version-controlled geometry editor for engineering teams to tune crossing width zones, transit thresholds, and multi-zone pocket monitors with documented revision histories.

### Cryptographic Audit Log & Component Health
<table><tr>
<td valign="top" width="50%"><img src="Assets/12-audit-log.png" alt="Tamper-Evident Audit Log" width="100%"></td>
<td valign="top" width="50%"><img src="Assets/08-system-health.png" alt="Component Health Diagnostics" width="100%"></td>
</tr></table>
### Access Control & Administration
<table><tr>
<td valign="top" width="50%"><img src="Assets/01-sign-in.png" alt="Role-Based System Access" width="100%"></td>
<td valign="top" width="50%"><img src="Assets/14-account-sessions.png" alt="Account Session Security" width="100%"></td>
</tr></table>
<p align="center">
  <img src="Assets/13-administration.png" alt="System Administration">
</p>

- **Tamper-Evident Audit Log:** Cryptographically hash-chained audit trail capturing all system events including user sign-ins, evidence views, configuration modifications, and supervisor review decisions. Operators can verify the cryptographic chain integrity at any time.
- **System Health Monitor:** Real-time health diagnostic dashboard monitoring every system service (API, Database, Audit, Storage, Vision Pipeline, Camera RTSP Feed, Counting Engine, Evidence Store, and Host Metrics including CPU, RAM, and GPU temperature/utilization).
- **Role-Based Access Control:** Strict role segregation across five permission tiers (Viewer, Operator, Supervisor, Administrator, Auditor) with active session revocation controls.

---

## Key Capabilities

- **High-Performance Edge Inference:** Powered by an optimized YOLO26m neural detector executing on ONNX Runtime with hardware acceleration (NVIDIA TensorRT, CUDA, or CPU fallback).
- **Multi-Object Tracking & Line-Crossing Engine:** Integrates ByteTrack and BoT-SORT algorithms with intelligent track-stitching to maintain subject persistence across temporary occlusions.
- **Privacy-Preserving Architecture:** Strictly measures aggregate traffic volume without extracting biometric features, facial geometry, or persistent identifiers.
- **Cryptographic Auditability:** Every administrative modification and evidence access is immutably committed into a hash-chained Merkle-style audit log.
- **Zero Cloud Dependence:** Designed for air-gapped facility environments with local database storage, offline package installers, and local camera ingestion.
- **Honest Coverage Accounting:** Explicitly measures and reports video ingestion uptime percentages, ensuring that shift reports accurately reflect any periods where camera feeds were offline.

---

## Technical Stack

- **Neural Detection:** YOLO26m on ONNX Runtime (TensorRT / CUDA / DirectML / CPU)
- **Tracking & Geometry:** ByteTrack, BoT-SORT Multi-Object Tracking, Line-Crossing Track Stitching
- **Backend API:** Python 3.12, FastAPI, WebSocket Live Video Streaming, Pydantic
- **Database & Migrations:** SQLite with Alembic Migration Engine, Hash-Chained Audit Ledger
- **Frontend Dashboard:** React, TypeScript, Tailwind CSS, Vite
- **Testing & Verification:** Pytest, Playwright (End-to-End browser suite), Axe-Core (WCAG 2.1 AA Accessibility)
- **Packaging & Delivery:** Offline Standalone Installer, systemd Service Supervision

---

## Vision Pipeline Architecture

```text
+-------------------------------------------------------------+
|                  RTSP Video Stream Source                   |
|  - IP Security Cameras / On-Premises RTSP Video Feed        |
+------------------------------+------------------------------+
                               | Real-Time Frames (15-30 FPS)
+------------------------------v------------------------------+
|                  Inference & Tracking Engine                |
|  - Frame Ingestion & Hardware Pre-processing                |
|  - YOLO26m ONNX Runtime Inference (TensorRT / CUDA)         |
|  - ByteTrack Association & Trajectory Smoothing             |
+------------------------------+------------------------------+
                               | Tracked Object Bounding Boxes
+------------------------------v------------------------------+
|                 Line-Crossing Analysis Engine               |
|  - Lane-Aware Polygon Intersection & Direction Vector       |
|  - Temporal Dwell Time & Transit Validation                 |
|  - High-Confidence Crossing vs Supervisor Review Decision   |
+---------------+------------------------------+---------------+
                |                              |
+---------------v--+                    +------v---------------+
| Evidence Capture |                    | Tamper-Evident Audit |
|  - Crop Snapshot |                    |  - SHA-256 Chained   |
|  - Timestamped   |                    |    Event Ledger      |
|    Local Storage |                    |  - Verified Records  |
+---------------+--+                    +------+---------------+
                |                              |
+---------------v------------------------------v--------------+
|               FastAPI WebSocket & REST Service              |
|  - Broadcasts Live Detections to Operator Dashboards        |
|  - Provides Role-Based Access Control and Shift Exports     |
+-------------------------------------------------------------+
```

---

## Core Engineering Principles

- **Edge Independence:** Built for mission-critical industrial installations where cloud connectivity is unavailable, latency-sensitive, or prohibited by security policy.
- **Privacy by Design:** Focuses exclusively on count validation and flow analytics; does not capture personal identity, facial embeddings, or demographic profiles.
- **Audit Verifiability:** Provides undeniable evidentiary integrity for physical security audits without reliance on external timestamping authorities.

---

## Notice

Commercial system I built while working at ANTS Drone Hub (Apr 2026 – Nov 2026). Source code, internal branding, and internal visuals are omitted under NDA. Names, branding, people, and camera images in the screenshots are removed or blurred.
