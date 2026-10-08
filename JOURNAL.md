# Engineering Journal & Refactoring Log

## Architectural Retrospective: From Course Labs to Enterprise Structure

### Early Exploration & Role Friction
During initial lab builds, I progressed rapidly and jumped directly into custom roles, Jinja2 templating, and complex variable structures.

* **Friction Point:** Combining role directory abstractions, variable scopes, and handler chains before consolidating foundational execution mechanics caused information overload and debugging complexity across multi-file paths.
* **Engineering Pivot:** Stepped back to consolidate flat playbooks, establish strict FQCN module syntax (`ansible.builtin`, `ansible.posix`), and enforce clear inventory parsing inside `ansible.cfg`.

### Codebase Refactoring & Capability Layout
As my playbook suite expanded across system facts, loop iterations, and Vault encryption, lesson-based directory layouts created workspace fragmentation.

* **Refactoring Strategy:** Reorganized scattered lesson directories into a unified `ansible-rhce-portfolio` structure organized by functional capability:
  1. `01_control_and_inventory`: Ad-hoc verification and SSH key setup.
  2. `02_core_playbooks`: Package management, custom facts, and host defaults.
  3. `03_conditionals_and_loops`: Dynamic user loops and automated audit playbooks.
  4. `04_templates_and_vault`: Secure variable management and Jinja2 templates.
  5. `roles`: Production-ready custom role abstractions (`apache_web`).
* **Result:** Isolated sensitive Vault credentials via `.gitignore` and established a clean baseline for automated testing and terminal demonstrations.
