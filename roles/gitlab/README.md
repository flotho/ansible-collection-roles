# [gitlab](#gitlab)

Install and configure GitLab on your system.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/robertdebock/ansible-role-gitlab/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-gitlab/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-gitlab/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-gitlab)|[![quality](https://img.shields.io/ansible/quality/57338)](https://galaxy.ansible.com/robertdebock/gitlab)|[![downloads](https://img.shields.io/ansible/role/d/57338)](https://galaxy.ansible.com/robertdebock/gitlab)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-gitlab.svg)](https://github.com/robertdebock/ansible-role-gitlab/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.

```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.roles.gitlab
      gitlab_letsencrypt: no
      gitlab_cleanup_ruby: no
      gitlab_trusted_certs:
        - isrgrootx1.pem  # A root certificate for letsencrypt.
```

The machine needs to be prepared. In CI this is done using `molecule/default/prepare.yml`:

```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.roles.bootstrap
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:

```yaml
---
# defaults file for gitlab

# Specify a specific version for GitLab to install.
# Please have a look at this repository for available package version:
# community: "https://packages.gitlab.com/gitlab/gitlab-ce"
# enterprise: "https://packages.gitlab.com/gitlab/gitlab-ee"
gitlab_version: "14.9.2"

# A part of the version is the "release", mostly "0". See repositories above.
gitlab_release: 0

# Choose to install "enterprise" or "community".
gitlab_distribution: community

# Would you like Ansible to show you the initial_root_password?
gitlab_show_initial_root_password: no

# Instead of defining the variables below, you can also simply copy a configuration file.
# Place this file into the `files` directory that hosts the playbooks.
# To configure GitLab using variables, comment this line
# gitlab_configuration_file: gitlab.rb

# The configuration below is only required when **not** placing a configuration file.

# The URL where the gitlab installation will be made available on.
# For "https", let's encrypt will be used.
gitlab_external_url: "http://localhost"

# Set the preferred timezone.
gitlab_rails_time_zone: UTC

# The syntax of `gitlab.rb` will be tested before applying this role. The
# package `ruby` is required for that. By default `ruby` is installed and
# uninstalled. This makes the role not idempotent, so CI ruby is not un-
# installed.
gitlab_cleanup_ruby: yes

# This role checks for outstanding database migrations. The role wil wait for
# migrations to finish. This value is in minutes.
gitlab_database_migrations_retries: 300

# You can install all roles but not specifying any role, or select a few roles.
# gitlab_roles:
#   - redis_sentinel_role
#   - redis_master_role
#   - redis_replica_role
#   - geo_primary_role
#   - geo_secondary_role
#   - postgres_role
#   - consul_role
#   - application_role
#   - monitoring_role

# You can set the default theme for the GitLab ui. Pick any one from:
# indigo, dark, light, blue, green, lightindigo, lightblue, lightgreen,
# red or lightred. (Nerds call "pink" "redlight")
gitlab_default_theme: dark

# You can disable features for newly created projects.
gitlab_default_projects_features_issues: yes
gitlab_default_projects_features_merge_requests: yes
gitlab_default_projects_features_wiki: yes
gitlab_default_projects_features_snippets: yes
gitlab_default_projects_features_builds: yes
gitlab_default_projects_features_container_registry: yes

# LDAP settings.
gitlab_rails_ldap_enabled: no
gitlab_rails_prevent_ldap_sign_in: no
# When `gitlab_rails_ldap_enabled` is set to `yes`, you need to define (at
# least on) `gitlab_rails_ldap_servers`.
# gitlab_rails_ldap_servers:
#   - name: main
#     label: LDAP
#     host: _your_ldap_server
#     port: 389
#     uid: sAMAccountName
#     bind_dn: _the_full_dn_of_the_user_you_will_bind_with
#     password: _the_password_of_the_bind_user
#     encryption: plain
#     verify_certificates: yes
#     smartcard_auth: yes
#     active_directory: yes
#     allow_username_or_email_login: no
#     lowercase_usernames: no
#     block_auto_created_users: no
#     base: ""
#     user_filter: ""
#     # These settings are only available when `gitlab_distribution` is set to
#     # the value `enterprise`.
#     # group_base: ""
#     # admin_group: ""
#     # sync_ssh_keys: no
#   - name: secondary
#     label: LDAP
#     host: _your_ldap_server
#     port: 389
#     uid: sAMAccountName
#     bind_dn: _the_full_dn_of_the_user_you_will_bind_with
#     password: _the_password_of_the_bind_user
#     encryption: plain
#     verify_certificates: yes
#     smartcard_auth: yes
#     active_directory: yes
#     allow_username_or_email_login: no
#     lowercase_usernames: no
#     block_auto_created_users: no
#     base: ""
#     user_filter: ""
#     # These settings are only available when `gitlab_distribution` is set to
#     # the value `enterprise`.
#     # group_base: ""
#     # admin_group: ""
#     # sync_ssh_keys: no

# Backup settings.
gitlab_rails_manage_backup_path: yes
gitlab_rails_backup_path: /var/opt/gitlab/backups
gitlab_rails_backup_gitaly_backup_path: /opt/gitlab/embedded/bin/gitaly-backup
gitlab_rails_backup_archive_permissions: "0644"
gitlab_rails_backup_pg_schema: public
gitlab_rails_backup_keep_time: 604800

# You can save backups on AWS S3.
# gitlab_rails_backup_upload_connection:
#   provider: AWS
#   region: eu-west-1
#   aws_access_key_id: AKIAKIAKI
#   aws_secret_access_key: secret123
#   use_iam_profile: no
# gitlab_rails_backup_upload_remote_directory: my.s3.bucket
# gitlab_rails_backup_multipart_chunk_size: 104857600
# gitlab_rails_backup_encryption: AES256
# gitlab_rails_backup_encryption_key: "base64-encoded encryption key"
# gitlab_rails_backup_upload_storage_options:
#   server_side_encryption: "aws:kms"
#   server_side_encryption_kms_key_id: "arn:aws:kms:YOUR-KEY-ID-HERE"
# gitlab_rails_backup_storage_class: STANDARD

# You can also save a backup on DigitalOcean Spaces.
# gitlab_rails_backup_upload_connection:
#   provider: AWS
#   region: ams3
#   aws_access_key_id: AKIAKIAKI
#   aws_secret_access_key: secret123
#   endpoint: "https://ams3.digitaloceanspaces.com"
# gitlab_rails_backup_upload_remote_directory: my.s3.bucket

# You can skip parts in a backup.
# gitlab_rails_env:
#   SKIP: db,uploads,repositories,builds,artifacts,lfs,registry,pages

# SMTP settings.
gitlab_rails_smtp_enable: yes
gitlab_rails_smtp_address: smtp.server
gitlab_rails_smtp_port: 465
gitlab_rails_smtp_user_name: smtp user
gitlab_rails_smtp_password: smtp password
gitlab_rails_smtp_domain: example.com
gitlab_rails_smtp_authentication: login
gitlab_rails_smtp_enable_starttls_auto: yes
gitlab_rails_smtp_tls: no
gitlab_rails_smtp_pool: no
gitlab_rails_smtp_openssl_verify_mode: none
gitlab_rails_smtp_ca_path: etc/ssl/certs
gitlab_rails_smtp_ca_file: /etc/ssl/certs/ca-certificates.crt

# E-mail settings.
gitlab_rails_gitlab_email_enabled: yes
gitlab_rails_gitlab_email_from: "example@example.com"
gitlab_rails_gitlab_email_display_name: Example
gitlab_rails_gitlab_email_reply_to: "noreply@example.com"
gitlab_rails_gitlab_email_subject_suffix: ""
gitlab_rails_gitlab_email_smime_enabled: no
gitlab_rails_gitlab_email_smime_key_file: /etc/gitlab/ssl/gitlab_smime.key
gitlab_rails_gitlab_email_smime_cert_file: /etc/gitlab/ssl/gitlab_smime.crt
gitlab_rails_gitlab_email_smime_ca_certs_file: /etc/gitlab/ssl/gitlab_smime_cas.crt

# User settings.
# gitlab_rails['gitlab_default_can_create_group'] = true
# gitlab_rails['gitlab_username_changing_enabled'] = true

# Gravater settings.
# gitlab_rails['gravatar_plain_url'] = 'http://www.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon'
# gitlab_rails['gravatar_ssl_url'] = 'https://secure.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon'

# Cron jobs settings, only when `gitlab_distribution` == `enterprise`.
# gitlab_rails['geo_file_download_dispatch_worker_cron'] = "*/10 * * * *"
# gitlab_rails['geo_repository_sync_worker_cron'] = "*/5 * * * *"
# gitlab_rails['geo_secondary_registry_consistency_worker'] = "* * * * *"
# gitlab_rails['geo_secondary_usage_data_cron_worker'] = "0 0 * * 0"
# gitlab_rails['geo_prune_event_log_worker_cron'] = "*/5 * * * *"
# gitlab_rails['geo_repository_verification_primary_batch_worker_cron'] = "*/5 * * * *"
# gitlab_rails['geo_repository_verification_secondary_scheduler_worker_cron'] = "*/5 * * * *"
# gitlab_rails['ldap_sync_worker_cron'] = "30 1 * * *"
# gitlab_rails['ldap_group_sync_worker_cron'] = "0 * * * *"
# gitlab_rails['historical_data_worker_cron'] = "0 12 * * *"
# gitlab_rails['pseudonymizer_worker_cron'] = "0 23 * * *"
# gitlab_rails['elastic_index_bulk_cron'] = "*/1 * * * *"
# gitlab_rails['analytics_devops_adoption_create_all_snapshots_worker_cron'] = "0 4 * * 0"

# Incoming email settings.
# gitlab_rails['incoming_email_address'] = "gitlab-incoming+%{key}@gmail.com"
# gitlab_rails['incoming_email_email'] = "gitlab-incoming@gmail.com"
# gitlab_rails['incoming_email_password'] = "[REDACTED]"
# gitlab_rails['incoming_email_host'] = "imap.gmail.com"
# gitlab_rails['incoming_email_port'] = 993
# gitlab_rails['incoming_email_ssl'] = true
# gitlab_rails['incoming_email_start_tls'] = false
# gitlab_rails['incoming_email_mailbox_name'] = "inbox"
# gitlab_rails['incoming_email_idle_timeout'] = 60
# gitlab_rails['incoming_email_log_file'] = "/var/log/gitlab/mailroom/mail_room_json.log"
# gitlab_rails['incoming_email_expunge_deleted'] = false
# gitlab_rails['incoming_email_inbox_method'] = 'microsoft_graph'
# gitlab_rails['incoming_email_inbox_options'] = {
#    'tenant_id': 'YOUR-TENANT-ID',
#    'client_id': 'YOUR-CLIENT-ID',
#    'client_secret': 'YOUR-CLIENT-SECRET',
#    'poll_interval': 60  # Optional
# }

# Settings for artifacts.
# gitlab_rails['artifacts_enabled'] = true
# gitlab_rails['artifacts_path'] = "/var/opt/gitlab/gitlab-rails/shared/artifacts"
# gitlab_rails['artifacts_object_store_enabled'] = false
# gitlab_rails['artifacts_object_store_direct_upload'] = false
# gitlab_rails['artifacts_object_store_background_upload'] = true
# gitlab_rails['artifacts_object_store_proxy_download'] = false
# gitlab_rails['artifacts_object_store_remote_directory'] = "artifacts"
# gitlab_rails['artifacts_object_store_connection'] = {
#   'provider' => 'AWS',
#   'region' => 'eu-west-1',
#   'aws_access_key_id' => 'AWS_ACCESS_KEY_ID',
#   'aws_secret_access_key' => 'AWS_SECRET_ACCESS_KEY',
#   # # The below options configure an S3 compatible host instead of AWS
#   # 'aws_signature_version' => 4, # For creation of signed URLs. Set to 2 if provider does not support v4.
#   # 'endpoint' => 'https://s3.amazonaws.com', # default: nil - Useful for S3 compliant services such as DigitalOcean Spaces
#   # 'host' => 's3.amazonaws.com',
#   # 'path_style' => false # Use 'host/bucket_name/object' instead of 'bucket_name.host/object'
# }

# Settings for OmniAuth
# gitlab_rails['omniauth_enabled'] = nil
# gitlab_rails['omniauth_allow_single_sign_on'] = ['saml']
# gitlab_rails['omniauth_sync_email_from_provider'] = 'saml'
# gitlab_rails['omniauth_sync_profile_from_provider'] = ['saml']
# gitlab_rails['omniauth_sync_profile_attributes'] = ['email']
# gitlab_rails['omniauth_auto_sign_in_with_provider'] = 'saml'
# gitlab_rails['omniauth_block_auto_created_users'] = true
# gitlab_rails['omniauth_auto_link_ldap_user'] = false
# gitlab_rails['omniauth_auto_link_saml_user'] = false
# gitlab_rails['omniauth_auto_link_user'] = ['saml']
# gitlab_rails['omniauth_external_providers'] = ['twitter', 'google_oauth2']
# gitlab_rails['omniauth_allow_bypass_two_factor'] = ['google_oauth2']
# gitlab_rails['omniauth_providers'] = [
#   {
#     "name" => "google_oauth2",
#     "app_id" => "YOUR APP ID",
#     "app_secret" => "YOUR APP SECRET",
#     "args" => { "access_type" => "offline", "approval_prompt" => "" }
#   }
# ]
# gitlab_rails['omniauth_cas3_session_duration'] = 28800
# gitlab_rails['omniauth_saml_message_max_byte_size'] = 250000

# Settings for git storage
# git_data_dirs({
#   "default" => {
#     "path" => "/mnt/nfs-01/git-data"
#    }
# })

# Settings for uploads
# gitlab_rails['uploads_directory'] = "/var/opt/gitlab/gitlab-rails/uploads"
# gitlab_rails['uploads_storage_path'] = "/opt/gitlab/embedded/service/gitlab-rails/public"
# gitlab_rails['uploads_base_dir'] = "uploads/-/system"
# gitlab_rails['uploads_object_store_enabled'] = false
# gitlab_rails['uploads_object_store_direct_upload'] = false
# gitlab_rails['uploads_object_store_background_upload'] = true
# gitlab_rails['uploads_object_store_proxy_download'] = false
# gitlab_rails['uploads_object_store_remote_directory'] = "uploads"
# gitlab_rails['uploads_object_store_connection'] = {
#   'provider' => 'AWS',
#   'region' => 'eu-west-1',
#   'aws_access_key_id' => 'AWS_ACCESS_KEY_ID',
#   'aws_secret_access_key' => 'AWS_SECRET_ACCESS_KEY',
#   # # The below options configure an S3 compatible host instead of AWS
#   # 'host' => 's3.amazonaws.com',
#   # 'aws_signature_version' => 4, # For creation of signed URLs. Set to 2 if provider does not support v4.
#   # 'endpoint' => 'https://s3.amazonaws.com', # default: nil - Useful for S3 compliant services such as DigitalOcean Spaces
#   # 'path_style' => false # Use 'host/bucket_name/object' instead of 'bucket_name.host/object'
# }

# Database settings
# If you are using the internal database, set `gitlab_rails_db_enabled` to "no".
gitlab_rails_db_enabled: no
gitlab_rails_db_adapter: postgresql
gitlab_rails_db_encoding: unicode
# gitlab_rails_db_collation:
gitlab_rails_db_database: gitlabhq_production
gitlab_rails_db_username: gitlab
gitlab_rails_db_password: "Som3P@s5w0rd"
gitlab_rails_db_host: localhost
gitlab_rails_db_port: 5432
# gitlab_rails_db_socket:
# gitlab_rails_db_sslmode:
gitlab_rails_db_sslcompression: 0
# gitlab_rails_db_sslrootcert:
# gitlab_rails_db_sslcert:
# gitlab_rails_db_sslkey:
gitlab_rails_db_prepared_statements: no
gitlab_rails_db_statements_limit: 1000
# gitlab_rails_db_connect_timeout:
# gitlab_rails_db_keepalives:
# gitlab_rails_db_keepalives_idle:
# gitlab_rails_db_keepalives_interval:
# gitlab_rails_db_keepalives_count:
# gitlab_rails_db_tcp_user_timeout:
# gitlab_rails_db_application_name:

# SSL settings
# # If you do not want to use SSL, use this structure.
gitlab_letsencrypt: yes
# gitlab_external_url: "http://gitlab.example.com" # (No `https` in the value.)
# # If you bring your own certificates, use this structure.
# gitlab_letsencrypt: no
# gitlab_ssl_directory: /etc/gitlab/ssl
# gitlab_ssl_key: some_file.key
# gitlab_ssl_crt: some_file.crt
# # If you'd like to use letsencrypt, use this scructure.
# gitlab_letsencrypt: yes
# gitlab_letsencrypt_contact_emails:
#   - robert@meinit.nl
# gitlab_letsencrypt_group: root
# gitlab_letsencrypt_key_size: 2048
# gitlab_letsencrypt_owner: root
# gitlab_letsencrypt_wwwroot: /var/opt/gitlab/nginx/www
# gitlab_letsencrypt_auto_renew: yes
# gitlab_letsencrypt_auto_renew_hour: 0
# gitlab_letsencrypt_auto_renew_minute: nil
# gitlab_letsencrypt_auto_renew_day_of_month: nil
# gitlab_letsencrypy_auto_renew_log_directory: /var/log/gitlab/lets-encrypt

# In case you need to trust a (CA) certificate to access remote resources,
# like an LDAP server, download the (CA) certificate, place it in the `files`
# directory and refer to it in the below list.
# gitlab_trusted_certs:
#   - my-ca-1.crt
#   - my-1.crt
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-gitlab/blob/master/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap)|[![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/robertdebock/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/ansible-role-gitlab/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|el|7, 8|
|ubuntu|focal|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-gitlab/issues)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[robertdebock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
