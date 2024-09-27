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
Key Parameters:
policies: Comma-separated list of policies that need to be created/updated.
app: The name of the application (used as the role_id or entry_key in YAML).
action: Specifies the action to perform (create_approle, update_approle, create_group, update_group, grant).
ipa_key: (Optional) IPA key required for creating/updating groups.
Available Parameters
Parameter	Type	Description
policies	String	A comma-separated list of policies to apply to the entry. Example: policies=app/data/dfdarkfdaffdsde/ro,consul_tf_ro.
app	String	The name of the application (used as the role_id or entry_key). Example: app=zzllff.
action	String	The action to perform: create_approle, update_approle, create_group, update_group, grant. Example: action=create_group.
ipa_key	String	IPA key required for create_group or update_group actions. Example: ipa_key=1232cd. (Not required for approle-related actions.)
Script Description
This script facilitates the following tasks:

Manage Vault Policies: Add, update, or validate policies within YAML files (groups.yml, products.yml) and Consul (consul.yml).
HCL File Generation: Dynamically generate HCL files using Jinja2 templates, based on policies (e.g., admin or read-only).
YAML Data Management: Handles YAML reading and writing using ruamel.yaml, preserving formatting, comments, and multi-line strings.
Supported Actions
create_group: Create a new group entry in groups.yml.
update_group: Update an existing group entry's policies in groups.yml.
create_approle: Create a new approle entry in products.yml.
update_approle: Update an existing approle entry's policies in products.yml.
grant: Grants access to specific paths (currently placeholder functionality).
File Structure and Paths
The script is located in the /GIT/vault-policy-script directory, and the required YAML and policy files are located in /GIT/vault-policy-eq. The script dynamically resolves file paths based on the script's location using relative paths.

Directory Structure:

bash
Copy code
/GIT/
  ├── vault-policy-script/
  │    └── vault_policy_script.py   # Script Location
  └── vault-policy-eq/
       ├── consul/
       │    └── consul.yml          # Consul policy file
       ├── policies/                # Directory containing policy HCL files
       ├── groups/
       │    └── groups.yml          # YAML file for group management
       └── aproles/
            └── products.yml        # YAML file for approle management
File Paths Used in the Script:

consul_file_path: Path to the Consul policy YAML file, resolved to ../vault-policy-eq/consul/consul.yml.
base_policy_dir: Base directory for policies, resolved to ../vault-policy-eq/policies.
groups_file_path: Path to the group YAML file, resolved to ../vault-policy-eq/groups/groups.yml.
approles_file_path: Path to the approles YAML file, resolved to ../vault-policy-eq/aproles/products.yml.
These paths are dynamically determined at runtime based on the script's location.

Function Descriptions
1. read_yaml(file_path)
Description: Reads a YAML file using ruamel.yaml while preserving formatting, comments, and multi-line strings.
Parameters: file_path - The path to the YAML file.
Returns: The YAML data and the ruamel.yaml object for dumping.
2. write_yaml(file_path, data, yaml=None)
Description: Writes updated data back to the YAML file, maintaining its original format.
Parameters:
file_path: Path to the YAML file.
data: Data to write back.
yaml: Optional ruamel.yaml object.
3. validate_and_update_consul_policy(policy, consul_file_path)
Description: Validates and updates Consul policies in consul.yml. Adds missing policies.
Parameters:
policy: Policy name (e.g., consul_tf_ro).
consul_file_path: Path to consul.yml.
4. render_hcl_template(template_path, context)
Description: Renders an HCL file from a Jinja template with a given context.
Parameters:
template_path: Path to the Jinja template.
context: Context dictionary for rendering the template.
5. create_hcl_file(file_path, policy)
Description: Creates a new HCL file for a given policy, using a Jinja2 template.
Parameters:
file_path: Path where the HCL file will be created.
policy: The policy name, used to determine the app type and policy type.
6. create_or_validate_hcl_files(policy, base_policy_dir)
Description: Validates the existence of the HCL files for the specified policy. Creates the required directories and files if they don’t exist.
Parameters:
policy: Policy name to validate.
base_policy_dir: Base directory where policies are stored.
7. handle_approle(yaml_data, entry_key, policies, consul_file_path, base_policy_dir)
Description: Handles creation or updating of approle entries in products.yml.
Parameters:
yaml_data: YAML data.
entry_key: The role_id to be created/updated.
policies: List of policies.
consul_file_path: Path to Consul YAML file.
base_policy_dir: Base directory for policies.
8. handle_group(yaml_data, entry_key, ipa_key, policies, consul_file_path, base_policy_dir)
Description: Handles creation or updating of group entries in groups.yml.
Parameters:
yaml_data: YAML data.
entry_key: The group entry key.
ipa_key: IPA key associated with the group.
policies: List of policies.
consul_file_path: Path to Consul YAML file.
base_policy_dir: Base directory for policies.
9. create_entry(yaml_data, entry_key, ipa_key, policies, consul_file_path, base_policy_dir, is_group=False)
Description: Determines if a new entry needs to be created, and delegates creation to the relevant handler (handle_group or handle_approle).
Parameters:
yaml_data: YAML data.
entry_key: The entry key for the approle/group.
ipa_key: IPA key (only for group).
policies: List of policies.
consul_file_path: Path to Consul YAML file.
base_policy_dir: Base directory for policies.
is_group: Boolean flag to indicate if the entry is for a group.
10. update_entry(yaml_data, entry_key, ipa_key, policies, consul_file_path, base_policy_dir, is_group=False)
Description: Updates an existing entry by adding new policies.
Parameters:
yaml_data: YAML data.
entry_key: The entry key for the approle/group.
ipa_key: IPA key (only for group).
policies: List of policies.
consul_file_path: Path to Consul YAML file.
base_policy_dir: Base directory for policies.
is_group: Boolean flag to indicate if the entry is for a group.
YAML File Management
Groups (groups.yml)
Contains group entries where each group is associated with an ipa_key and a set of policies.
AppRoles (products.yml)
Manages approle entries, where each entry is tied to an application and its corresponding policies.
Consul Policies (consul.yml)
Contains policies specific to Consul, used to validate and update required policies.
sql
Copy code

This markdown file includes all the necessary information for using and understanding the script, from command-line parameters to function descriptions and file management
