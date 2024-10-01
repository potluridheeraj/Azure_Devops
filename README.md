| **Action Type**         | **Required Parameters**                                     | **Description**                                                   |
|-------------------------|-------------------------------------------------------------|-------------------------------------------------------------------|
| **create_group**         | `app`, `ipa_key`, `policies`                                | Creates a new group with policies.                                |
| **update_group**         | `app`, `policies`                                           | Updates an existing group with new policies.                      |
| **create_approle**       | `app`, `policies`                                           | Creates a new approle with policies.                              |
| **update_approle**       | `app`, `policies`                                           | Updates an existing approle with new policies.                    |
| **grant_robot**          | `app`, `user`, `policies`, `ipa_key`                        | Grants access to a robot (user) by adding policies.               |
| **invalidate_and_update_consul_policy** | `policies`, `app`                                     | Invalidates and updates Consul policies.                          |
| **create_user**          | `user`, `policies`, `user_file_path`                        | Creates a new user entry with policies in the user file.           |
