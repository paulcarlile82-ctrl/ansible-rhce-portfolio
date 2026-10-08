# Enterprise Ansible Automation Architecture (RHCE / EX294)

Welcome to my Red Hat Certified Engineer (EX294) study and automation portfolio. This repository documents how I built, refactored, and mastered production-grade Ansible automation on Rocky Linux 9.

---

## Live Terminal Demonstration

[![asciicast](https://asciinema.org/a/439ed80f-ee4a-438c-8729-bb67397ea8ff.svg)](https://asciinema.org/a/439ed80f-ee4a-438c-8729-bb67397ea8ff)

*Automated execution of the `apache_web` role against `node1`, demonstrating tasks execution followed by an immediate second run verifying zero state drift (`changed=0`).*

---

## My Learning Journey & Architectural Evolution

### Building the Foundations
I started by locking down core mechanics: control node connectivity, key-based SSH authentication, and central defaults inside `ansible.cfg`. From there, I focused on writing simple, single-purpose playbooks using modern Red Hat standard module syntax—sticking strictly to Fully Qualified Collection Names like `ansible.builtin.dnf` and `ansible.builtin.service`.

### Moving Fast, Overloading, and Reining It Back In
Once standard playbooks clicked, I started moving fast. I felt confident, got excited about high-level automation, and jumped straight into writing custom Ansible roles, complex Jinja2 templates, nested loops, and Vault encryption all at once. 

That was where I hit a wall. 

Combining dynamic variable scopes, template logic, handler triggers, and multi-directory role structures before fully mastering each concept individually gave me serious information overload. I was spending my time debugging directory paths and tracing variable precedence across five different files instead of truly understanding the execution flow. I felt that friction immediately. 

I decided to take a step back, slow down, and process everything one piece at a time. I reined in the codebase, broke my learning back down to baseline concepts, and didn't move on until each individual topic clicked line by line in the terminal.

---

## Deep Dive: Technical Capabilities & Key Takeaways

### 1. Control Node Architecture & Inventory Management (`01_control_and_inventory/`)
* **What I Built:** Configured `ansible.cfg` defaults, key-based SSH authentication across `control`, `node1`, `node2`, and `node3`, and established structured inventory host groups (`webservers`, `database`).
* **Technical Challenge:** Ensuring predictable host resolution and security without relying on manual SSH password prompts during automated runs.
* **Key Takeaway:** Mastered `ansible.cfg` precedence order, strict inventory grouping, and verified host connectivity using ad-hoc modules (`ansible.builtin.ping`).

### 2. Idempotent Package & Service Management (`02_core_playbooks/`)
* **What I Built:** Automated `dnf` package installations, service state enforcement (`vsftpd`, `httpd`), and hostname configuration across multi-node target environments.
* **Technical Challenge:** Writing playbooks that enforce target state cleanly without causing state drift or unintended restarts on secondary runs.
* **Key Takeaway:** Deepened understanding of idempotency by combining state parameters (`present`, `started`, `enabled`) with strict Fully Qualified Collection Names (FQCN).

### 3. Dynamic Custom System Facts (`02_core_playbooks/setup_custom_facts.yml`)
* **What I Built:** Developed playbooks to generate and push local custom facts (`/etc/ansible/facts.d/custom.fact`) to managed nodes to dynamically drive playbook execution logic.
* **Technical Challenge:** Using local host parameters dynamically inside playbooks before standard fact gathering runs.
* **Key Takeaway:** Learned how Ansible gathers facts and parses custom INI/JSON fact files into the `ansible_facts.ansible_local` namespace to control execution conditionally.

### 4. Conditionals, Loops & Dynamic Audits (`03_conditionals_and_loops/`)
* **What I Built:** Automated user account creation using `loop` iterations, controlled system banner file updates (`/etc/issue`) via conditional checks, and automated dynamic web server auditing.
* **Technical Challenge:** Processing multi-variable user dictionaries and evaluating OS-specific variables dynamically across target hosts.
* **Key Takeaway:** Mastered loop control mechanics, variable filtering, and using `when` statements to ensure playbooks adapt safely across heterogeneous Linux nodes.

### 5. Jinja2 Templates & Ansible Vault Credential Security (`04_templates_and_vault/`)
* **What I Built:** Rendered dynamic configuration files using Jinja2 templates (`.j2`) and secured sensitive credentials using Ansible Vault encryption (`vault.yml`).
* **Technical Challenge:** Securing sensitive administrative credentials while keeping template logic clean and maintainable.
* **Key Takeaway:** Gained hands-on experience with Ansible Vault encryption workflows, string/dictionary variable substitution, and enforcing proper file permissions on deployed templates.

### 6. Modular Custom Role Abstraction (`roles/`)
* **What I Built:** Refactored web server deployment into a clean, reusable `apache_web` custom role featuring decoupled tasks, default variables, templates, and handler triggers.
* **Technical Challenge:** Managing variable precedence and ensuring handlers trigger only on actual state changes (e.g., config updates requiring service reloads).
* **Key Takeaway:** Learned how role directory standards keep enterprise codebases modular, testable, and maintainable at scale.

---

## Lab Environment & Topology

* **Control Node:** `control` (`192.168.56.10`) — Rocky Linux 9 running `ansible-core` & `ansible-navigator`
* **Managed Nodes:** `node1` (`.11`), `node2` (`.12`), `node3` (`.13`)
* **Execution User:** `vagrant` with passwordless `sudo` escalation
* **Standards:** Strict FQCN module usage, strict YAML formatting, and full idempotency

---

## Repository Layout

```text
ansible-rhce-portfolio/
├── README.md                      # Narrative architecture showcase
├── JOURNAL.md                     # Engineering retrospective & detailed refactoring log
├── ansible.cfg                    # Central control node configuration
├── inventory                      # Multi-node host groups
├── 01_control_and_inventory/      # Connectivity validation & ad-hoc tests
├── 02_core_playbooks/             # System prep, package management, custom facts
├── 03_conditionals_and_loops/     # Dynamic user provisioning, issue control, web audits
├── 04_templates_and_vault/        # Encrypted variable vault & Jinja2 templating
└── roles/                         # Production-ready custom role abstractions
    ├── deploy_apache.yml          # Role execution entry point
    └── apache_web/                # Custom Apache web server role
