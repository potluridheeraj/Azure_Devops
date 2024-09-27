# Vault Policy Management Script

This script is designed to manage YAML entries for Vault policies, groups, and approles, including the ability to create and update these entries dynamically. The script also handles HCL file generation using Jinja2 templates and validates policies in Consul.

## Table of Contents
- [Command-Line Usage](#command-line-usage)
- [Available Parameters](#available-parameters)
- [Script Description](#script-description)
- [File Structure and Paths](#file-structure-and-paths)
- [Function Descriptions](#function-descriptions)
- [YAML File Management](#yaml-file-management)

## Command-Line Usage

Run the script from the `/GIT/vault-policy-script` directory. The script accepts several command-line arguments that dictate the action to be performed and the entries to be updated or created.

**Example Command:**
```bash
python vault_policy_script.py policies=app/data/dfdarkfdaffdsde/ro,consul_tf_ro app=zzllff action=create_group ipa_key=1232cd
