# Ansible Rulebooks

Ansible Rulebooks are an extension of Ansible that allow event-driven listening sources to trigger conditional plays, allowing for greater automation of tasks that would previously require manual intervention. Ansible Rulebook documentation can be found [here](https://ansible.readthedocs.io/projects/rulebook/en/latest).

Rulebooks consist of three components.

1. Sources: Sources are plugins that create listeners for events. Examples include webhooks, Kafka, file changes, and alertmanager.
2. Rules: Rules are conditions that are checked to see if they match with event sources.
3. Actions: When a rule condition is met, the event action is triggered. Example actions are run_playbook, run_module, set_fact, post_event, debug.

Rulebooks follow a yaml format that will make them familiar to anyone that writes or uses ansible playbooks.

## Dependencies

- `openjdk@17`
- `python3.11 or >`
- `ansible`, `ansible-runner`, `ansible-rulebook`
- `ansible-galaxy collection install ansible.eda`
