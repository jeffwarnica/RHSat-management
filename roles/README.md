Dependent roles, and configuration go in this directory.

For "production" use, roles should be configured in <rolename.yaml>, with that referenced in <requirements.yaml>. So configured, roles can be installed and updated with :

`ansible-galaxy install --roles-path=. -r requirements.yaml`.

For development, one may manipulate their Ansible role path, or symlink each role to a suitable role directory here. In practice one will have a mix of role code downladed with ansible-galaxy and symlinks.

Only configuration files (not the roles themselves) should be commited to Git.

