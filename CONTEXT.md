# Project & Framework Domain Context

This document serves as the unified Ubiquitous Language, containing both project-specific terminology and Agent framework management terms.
All new terms generated during interactions with skills like `grill-with-docs` will be recorded here.

---
## 1. Agent Framework Language (Agent Skills Management)

**Issue tracker**:
The tool that hosts a repo's issues — GitHub Issues, Linear, a local `.scratch/` markdown convention, or similar. Skills like `to-issues`, `to-prd`, `triage`, and `qa` read from and write to it.
_Avoid_: backlog manager, backlog backend, issue host

**Issue**:
A single tracked unit of work inside an **Issue tracker** — a bug, task, PRD, or slice produced by `to-issues`.
_Avoid_: ticket (use only when quoting external systems that call them tickets)

**Triage role**:
A canonical state-machine label applied to an **Issue** during triage (e.g. `needs-triage`, `ready-for-afk`). Each role maps to a real label string in the **Issue tracker** via `docs/agents/triage-labels.md`.

### Framework Relationships
- An **Issue tracker** holds many **Issues**
- An **Issue** carries one **Triage role** at a time

### Flagged ambiguities
- "backlog" was previously used to mean both the *tool* hosting issues and the *body of work* inside it — resolved: the tool is the **Issue tracker**; "backlog" is no longer used as a domain term.
- "backlog backend" / "backlog manager" — resolved: collapsed into **Issue tracker**.

---

## 2. Project Domain Language (Lab Instrument Automation)

**SOP Parser**:
A module responsible for extracting text, tables, and parameters from Standard Operating Procedure documents (e.g., `.docx` files) and structuring them into an executable format.

**Test Sequence**:
The structured data (e.g., JSON or Python Dictionary format) parsed from an SOP document, containing instrument settings and execution steps.

**SOP Runner**:
An interactive CLI menu module (e.g., `menus/sop_runner_menu.py`) responsible for loading a **Test Sequence**, mapping it to physical or virtual instruments, and executing the tests automatically.

**Virtual SMU**:
A composite hardware abstraction. Since physical SMUs are extremely expensive, this concept combines an MQTT-based Power Supply Unit (PSU, e.g., SK120) and an Ammeter (AFUIOT) to simulate and replace the functionality of a traditional Source Measure Unit (SMU) in software logic.

### Project Relationships
- An **SOP Parser** can generate one or more **Test Sequences** from a single SOP document.
- The **SOP Runner** reads a **Test Sequence** and executes actions through the instrument abstraction layer.
- The instrument abstraction layer maps "SMU" commands required by the test to a physical `VisaInstrument` or to a **Virtual SMU** (PSU + AFUIOT) composed of MQTT nodes.
