# Enterprise Ansible Automation Architecture (RHCE / EX294)

Welcome to my Red Hat Certified Engineer (EX294) study and automation portfolio. This repository isn't just a collection of static playbooks—it's a living log of how I built, refactored, and mastered production-grade Ansible automation on Rocky Linux 9.

---

## My Learning Journey & Architectural Evolution

### Building the Foundations
I started by getting the core mechanics locked down: host connectivity, key-based SSH authentication, and central defaults inside `ansible.cfg`. From there, I focused on writing simple, single-purpose playbooks using modern Red Hat standard module syntax—sticking strictly to Fully Qualified Collection Names like `ansible.builtin.dnf` and `ansible.builtin.service`.

### Moving Fast, Overloading, and Reining It Back In
Once standard playbooks clicked, I started moving fast. I felt confident, got excited about high-level automation, and jumped straight into writing custom Ansible roles, complex Jinja2 templates, nested loops, and Vault encryption all at once. 

That was where I hit a wall. 

Combining dynamic variable scopes, template logic, handler triggers, and multi-directory role structures before fully mastering each concept individually gave me serious information overload. I was spending my time debugging directory paths and tracing variable precedence across five different files instead of truly understanding the execution flow. I felt that friction immediately. 

I decided to take a step back, slow down, and process everything one piece at a time. I reined in the codebase, broke my learning back down to baseline concepts, and didn't move on until each individual topic clicked line by line in the terminal.

---

## What I Mastered Step-by-Step

* **Control Node & Inventory Setup:** Configured central `ansible.cfg` defaults, key-based SSH access across nodes (`control`, `node1`, `node2`, `node3`), and verified inventory group targeting using ad-hoc commands.
* **Package Management & System Services:** Automated idempotent package installations (`dnf`), service state enforcement (`vsftpd`), and automated system hostname alignment across multi-node environments (`02_core_playbooks/`).
* **Custom System Facts:** Learned how Ansible gathers facts and developed playbooks to push local custom facts (`/etc/ansible/facts.d/custom.fact`) to managed nodes to dynamically drive playbook behavior (`setup_custom_facts.yml`).
* **Conditionals, Loops & Dynamic Audits:** Mastered user account creation using `loop` iterations, controlled system file updates (`/etc/issue`) via conditional checks, and automated dynamic web server auditing (`03_conditionals_and_loops/`).
* **Jinja2 Templates & Ansible Vault:** Built dynamic configuration files using Jinja2 templates and secured sensitive credentials using Ansible Vault encryption, isolating secrets safely outside of version control (`04_templates_and_vault/`).
* **Custom Role Abstraction:** Re-integrated my `apache_web` custom role with total clarity—decoupling tasks, default variables, templates, and handler triggers into clean, maintainable modular structures (`roles/`).

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
