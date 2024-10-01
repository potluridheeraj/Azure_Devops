
# Vault Policy Script

This script manages YAML files for updating entries like approles, groups, and robot access. It validates and updates Consul policies, creates HCL files using Jinja2 templates, and ensures users are properly added or updated in a separate `user.yml` file. The script supports command-line arguments for different actions such as `create_group`, `update_group`, `create_approle`, `update_approle`, and `grant_robot` access.

## Features

- **Group Management**: Create or update group entries in `groups.yml`.
- **AppRole Management**: Create or update approle entries in `products.yml`.
- **Robot Access**: Grant or update robot access in `consul.yml`. This includes validating or adding users in `user.yml`.
- **HCL Policy Generation**: Automatically generates HCL files using Jinja2 templates for policies.
- **Consul Policy Validation**: Validate and update Consul policies.

## New Feature: Robot Access & User Validation

### Robot Access
The `grant_robot` action allows you to grant a robot (user) access to a specific app by adding the user to the `users` section and assigning the necessary policies. If the user does not exist in `user.yml`, the script will automatically add the user.

### User Validation
The script now includes a function to validate and update users in `user.yml`. This ensures that the robot (user) is properly tracked in the user management file.

## Usage

### Example Command

```bash
python vault-eq.py policies=app/product/daap/admin,consul_tf_dfdba app=avv action=grant_robot ipa_key=991324 user=abc_user
```

### Arguments

- `policies`: Comma-separated list of policies to apply (e.g., `app/product/daap/admin,consul_tf_dfdba`).
- `app`: The name of the app to which the policies will be applied.
- `action`: The action to be performed (e.g., `grant_robot`, `create_group`, `update_group`, etc.).
- `ipa_key`: IPA key for certain group-related actions (not used for robot access).
- `user`: The robot (user) to grant access to.

### Important Paths

- **Consul File**: `consul.yml` (manages policies and robot access).
- **Group File**: `groups.yml` (manages groups and related data).
- **AppRole File**: `products.yml` (manages approle entries).
- **User File**: `user.yml` (manages user records for robot access).

## Setup

1. Ensure you have Python 3 installed.
2. Clone or download this repository.
3. Install required dependencies (if applicable, e.g., `PyYAML`, `Jinja2`):
    ```bash
    pip install -r requirements.txt
    ```
4. Run the script using command-line arguments as described above.

## File Structure

- `vault-eq.py`: Main script that handles policy, group, approle, and robot access management.
- `consul.yml`: YAML file for storing policies and robot access.
- `groups.yml`: YAML file for group management.
- `products.yml`: YAML file for AppRole management.
- `user.yml`: YAML file for user management (robot access).
- `templates/`: Directory for Jinja2 templates used for HCL policy generation.

## License

This project is licensed under the MIT License.
