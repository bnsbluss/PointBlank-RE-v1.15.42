# PointBlank-RE-v1.15.42

![Point Blank](https://img.shields.io/badge/Point%20Blank-v1.15.42-red)
![Reverse Engineering](https://img.shields.io/badge/Research-Reverse%20Engineering-blue)
![Ghidra](https://img.shields.io/badge/Tool-Ghidra-orange)
![REA](https://img.shields.io/badge/Tool-REA-purple)
![Status](https://img.shields.io/badge/Status-Active%20Research-success)

> **Authorized Reverse Engineering & Research Project**

Repository penelitian dan dokumentasi **Reverse Engineering Point Blank client v1.15.42**, dengan fokus pada pemahaman arsitektur client, sistem gameplay, hubungan client/server, serta investigasi khusus terhadap **Convoy Mode**.

## 📌 About This Project

`PointBlank-RE-v1.15.42` merupakan workspace penelitian untuk menganalisis executable legacy **PointBlank.exe v1.15.42** secara sistematis.

Tujuan utama:
- memahami struktur executable;
- memetakan arsitektur client;
- memahami alur gameplay;
- menemukan fungsi dan state yang berkaitan dengan mode permainan;
- memahami korelasi client/server;
- mengidentifikasi penyebab suatu fitur mengalami kegagalan;
- mendokumentasikan setiap temuan berdasarkan evidence;
- melakukan eksperimen hanya setelah behavior berhasil dipahami.

Fokus investigasi saat ini:

> **Convoy Mode dapat masuk ke dalam game, tetapi mengalami kondisi stuck dan tidak melanjutkan gameplay sebagaimana mestinya.**

## 🎯 Main Objective

```text
Convoy Mode
     │
     ├── Match Creation
     ├── Match Join
     ├── Loading
     ├── Player Spawn
     ├── Objective Initialization
     ├── Convoy Object / Vehicle
     ├── Objective Update
     ├── Game State Transition
     ├── Victory / Failure Condition
     └── Match Completion
```

Pertanyaan utama:

1. Di tahap mana Convoy berhenti?
2. Kenapa state tersebut berhenti?
3. Masalah berasal dari Client, Server, Data/Resource, Network/Event, atau kombinasi beberapa komponen?

## 🔬 Research Methodology

```text
PointBlank.exe v1.15.42
        │
        ▼
01. Binary Recon / Triage
        │
        ▼
02. Ghidra Baseline
        │
        ▼
03. Gameplay Anchors
        │
        ▼
04. Convoy Investigation
        │
        ▼
05. Client / Server Correlation
        │
        ▼
06. Root Cause
        │
        ▼
07. Experiment / Fix
```

## 🧭 Research Stages

### Stage 1 — Binary Recon / Mapping

Periksa:
- PE architecture
- Image Base
- Entry Point
- Sections
- Section permissions
- Section entropy
- Imports
- Exports
- Resources
- Strings
- Compiler/runtime indicators
- Protection indicators
- Suspicious sections
- Possible packer/compression indicators
- Analysis anomalies

**Jangan langsung patch executable.** Tujuan tahap ini adalah memahami binary terlebih dahulu.

### Stage 2 — Ghidra Baseline

Catat:
- Function count
- Symbol information
- Imported functions
- Defined strings
- XREF quality
- Decompiled functions
- Code/data separation
- Analysis errors
- Unusual control flow
- Interesting constants
- Global structures

### Stage 3 — Gameplay Anchors

Cari anchor yang berhubungan dengan:

```text
Match
Room
Battle
GameState
Player
Spawn
Objective
Round
Mission
Timer
Vehicle
Convoy
Team
Score
Victory
Failure
Complete
```

Nama string bukan bukti final. Candidate harus dikonfirmasi dengan XREF, caller/callee, control flow, data usage, runtime observation, dan korelasi server.

### Stage 4 — Convoy Investigation

Alur yang ingin dipetakan:

```text
Room
 │
 ▼
Match Start
 │
 ▼
Loading
 │
 ▼
Player Spawn
 │
 ▼
Convoy Initialization
 │
 ▼
Objective Active
 │
 ▼
Objective Update
 │
 ▼
State Transition
 │
 ├──────────► Victory
 ├──────────► Failure
 └──────────► Next State
```

### Stage 5 — Client / Server Correlation

Komponen server yang relevan:

```text
PBAuthServer.exe
PBBattleServer.exe
PBGameServer.exe
```

Source seperti `BattleManager.cs`, `KickAllPlayers.cs`, `Program.cs`, dan class terkait dapat dibandingkan dengan behavior client.

### Stage 6 — Root Cause

Setiap hypothesis harus memiliki evidence.

### Stage 7 — Experiment / Fix

```text
Understand
   ↓
Confirm
   ↓
Experiment
   ↓
Observe
   ↓
Document
```

Bukan:

```text
Guess → Patch → Hope
```

## 🧪 Evidence Standard

Setiap finding penting mencatat:

- **Claim** — apa yang diyakini terjadi.
- **Evidence** — RVA/address, function, XREF, string, log, screenshot, atau runtime observation.
- **Method** — bagaimana evidence diperoleh.
- **Confidence** — LOW / MEDIUM / HIGH / CONFIRMED.
- **Next Check** — pemeriksaan berikutnya.

## 📁 Repository Structure

```text
PointBlank-RE-v1.15.42/
│
├── README.md
│
├── notes/
│   ├── 01-binary-triage.md
│   ├── 02-ghidra-baseline.md
│   ├── 03-gameplay-anchors.md
│   ├── 04-convoy-investigation.md
│   └── 05-client-server-correlation.md
│
├── scripts/
│   └── README.md
│
├── evidence/
│   └── README.md
│
└── docs/
    ├── methodology.md
    ├── state-machine.md
    └── findings.md
```

## 🤖 AI Assistance

Sebagian pekerjaan analisis dan pemecahan masalah sulit dibantu menggunakan **AI, termasuk Claude Code**.

AI digunakan sebagai:
- coding assistant;
- analysis assistant;
- brainstorming;
- membantu memahami fungsi kompleks;
- membantu membuat script;
- membantu menyusun dokumentasi;
- membantu mengevaluasi hypothesis.

> **AI-generated hypothesis bukan merupakan evidence.**

Kesimpulan penting harus diverifikasi melalui binary, Ghidra, REA, runtime behavior, server source, log, atau evidence teknis lainnya.

## 🛠️ Tools

| Tool | Purpose |
|---|---|
| REA | Runtime / reverse-engineering observation |
| Ghidra | Static analysis |
| Claude Code | Assistance untuk analisis kode dan problem sulit |
| GitHub | Dokumentasi dan version control |

## 🔐 Research Safety / Scope

Repository ini berfokus pada penelitian dan analisis terhadap software yang dimiliki atau telah mendapatkan izin untuk dianalisis.

**Jangan commit:**
- PointBlank.exe
- Memory dumps sensitif
- Private credentials
- Server keys
- Database credentials
- Account information
- Private certificates
- Private server assets
- Sensitive personal data

Binary asli tetap disimpan secara lokal. Repository berisi analysis, documentation, evidence, scripts, dan research notes.

## 📌 Important Research Rules

### Rule 1 — Preserve Original

```text
Original Binary
      │
      └──► Analysis Copy
```

### Rule 2 — Don't Patch Blindly

Jangan melakukan patch sebelum memahami execution path.

### Rule 3 — Evidence First

Setiap hypothesis harus memiliki evidence.

### Rule 4 — Record Addresses

```text
Module
RVA
VA
Function
Caller
Callee
XREF
Description
Confidence
```

### Rule 5 — Reproducibility

Temuan sebisa mungkin dapat diperiksa kembali.

## 🧠 Research Philosophy

> **Memahami bagaimana sistem bekerja sampai kita mengetahui dengan jelas mengapa behavior tersebut terjadi.**

```text
Observe
   ↓
Measure
   ↓
Map
   ↓
Hypothesize
   ↓
Verify
   ↓
Document
   ↓
Experiment
```

## 🚧 Current Status

**Stage 1 — Binary Recon / Mapping**

Target:

```text
PointBlank.exe v1.15.42
```

Checklist:

```text
[ ] PE information
[ ] SHA-256
[ ] Architecture
[ ] Entry Point
[ ] Image Base
[ ] Sections
[ ] Imports
[ ] Resources
[ ] Strings
[ ] Protection indicators
[ ] Ghidra baseline
```

## 🗺️ Long-Term Investigation Map

```text
                 PointBlank.exe
                       │
                       ▼
                Binary Mapping
                       │
                       ▼
                 Ghidra Base
                       │
                       ▼
               Gameplay Systems
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
          Client              Server
             │                   │
             └─────────┬─────────┘
                       ▼
                 Convoy System
                       │
                       ▼
                 State Machine
                       │
                       ▼
                  Root Cause
                       │
                       ▼
                  Experiment
                       │
                       ▼
                 Verified Fix
```

## 📜 Findings

Confirmed findings dicatat di:

```text
docs/findings.md
```

Format:

```text
Finding ID:
PBRE-001

Title:
...

Status:
Investigating / Confirmed / Rejected

Claim:
...

Evidence:
...

Affected Component:
...

Confidence:
...

Next Action:
...
```

## 📈 Project Status

| Area | Status |
|---|---|
| Repository | 🟢 Ready |
| Binary Recon | 🟡 In Progress |
| Ghidra Baseline | ⚪ Pending |
| Gameplay Mapping | ⚪ Pending |
| Convoy Investigation | ⚪ Pending |
| Client/Server Correlation | ⚪ Pending |
| Root Cause | ⚪ Pending |
| Fix Experiment | ⚪ Pending |

## 👤 Project

**PointBlank-RE-v1.15.42**

Research focus:

> **Understanding the legacy Point Blank client architecture and investigating the Convoy gameplay flow through static analysis, runtime observation, and client/server correlation.**

## Disclaimer

This repository is intended for authorized software research, interoperability, debugging, preservation, and educational purposes.

No proprietary executable or confidential credential is intended to be distributed through this repository.
