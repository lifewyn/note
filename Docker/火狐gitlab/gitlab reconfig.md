
gitlab-ctl reconfigure
[2025-03-19T08:02:12+00:00] INFO: Started Cinc Zero at chefzero://localhost:1 with repository at /opt/gitlab/embedded (One version per cookbook)
Cinc Client, version 18.3.0
Patents: https://www.chef.io/patents
Infra Phase starting
[2025-03-19T08:02:12+00:00] INFO: *** Cinc Client 18.3.0 ***
[2025-03-19T08:02:12+00:00] INFO: Platform: x86_64-linux
[2025-03-19T08:02:12+00:00] INFO: Cinc-client pid: 4075
/opt/gitlab/embedded/lib/ruby/gems/3.2.0/gems/ffi-yajl-2.6.0/lib/ffi_yajl/encoder.rb:42: warning: undefining the allocator of T_DATA class FFI_Yajl::Ext::Encoder::YajlGen
[2025-03-19T08:02:12+00:00] INFO: Setting the run_list to ["recipe[gitlab-jh]"] from CLI options
[2025-03-19T08:02:12+00:00] INFO: Run List is [recipe[gitlab-jh]]
[2025-03-19T08:02:12+00:00] INFO: Run List expands to [gitlab-jh]
[2025-03-19T08:02:12+00:00] INFO: Starting Cinc Client Run for 192.168.30.21
[2025-03-19T08:02:12+00:00] INFO: Running start handlers
[2025-03-19T08:02:12+00:00] INFO: Start handlers complete.
Resolving cookbooks for run list: ["gitlab-jh"]
[2025-03-19T08:02:13+00:00] INFO: Loading cookbooks [gitlab-jh@0.0.1, gitlab-ee@0.0.1, package@0.1.0, gitlab@0.0.1, consul@0.1.0, patroni@0.1.0, pgbouncer@0.1.0, spamcheck@0.1.0, runit@5.1.7, logrotate@0.1.0, postgresql@0.1.0, redis@0.1.0, monitoring@0.1.0, registry@0.1.0, mattermost@0.1.0, gitaly@0.1.0, praefect@0.1.0, gitlab-kas@0.1.0, gitlab-pages@0.1.0, letsencrypt@0.1.0, nginx@0.1.0, acme@4.1.6, crond@0.1.0]
Synchronizing cookbooks:
  - gitlab-jh (0.0.1)
  - package (0.1.0)
  - gitlab-ee (0.0.1)
  - gitlab (0.0.1)
  - consul (0.1.0)
  - patroni (0.1.0)
  - pgbouncer (0.1.0)
  - logrotate (0.1.0)
  - runit (5.1.7)
  - spamcheck (0.1.0)
  - postgresql (0.1.0)
  - redis (0.1.0)
  - monitoring (0.1.0)
  - registry (0.1.0)
  - mattermost (0.1.0)
  - gitaly (0.1.0)
  - praefect (0.1.0)
  - gitlab-kas (0.1.0)
  - gitlab-pages (0.1.0)
  - letsencrypt (0.1.0)
  - nginx (0.1.0)
  - acme (4.1.6)
  - crond (0.1.0)
Installing cookbook gem dependencies:
Compiling cookbooks...
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/helpers/selinux_distro_helper.rb:2: warning: already initialized constant SELinuxDistroHelper::REDHAT_RELEASE_FILE
/opt/gitlab/embedded/cookbooks/package/libraries/helpers/selinux_distro_helper.rb:2: warning: previous definition of REDHAT_RELEASE_FILE was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/helpers/selinux_distro_helper.rb:3: warning: already initialized constant SELinuxDistroHelper::OS_RELEASE_FILE
/opt/gitlab/embedded/cookbooks/package/libraries/helpers/selinux_distro_helper.rb:3: warning: previous definition of OS_RELEASE_FILE was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/helpers/secrets_helper.rb:4: warning: already initialized constant SecretsHelper::SECRETS_FILE
/opt/gitlab/embedded/cookbooks/package/libraries/helpers/secrets_helper.rb:4: warning: previous definition of SECRETS_FILE was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/helpers/secrets_helper.rb:5: warning: already initialized constant SecretsHelper::SECRETS_FILE_CHEF_ATTR
/opt/gitlab/embedded/cookbooks/package/libraries/helpers/secrets_helper.rb:5: warning: previous definition of SECRETS_FILE_CHEF_ATTR was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/helpers/secrets_helper.rb:6: warning: already initialized constant SecretsHelper::SKIP_GENERATE_SECRETS_CHEF_ATTR
/opt/gitlab/embedded/cookbooks/package/libraries/helpers/secrets_helper.rb:6: warning: previous definition of SKIP_GENERATE_SECRETS_CHEF_ATTR was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/gitlab_cluster.rb:16: warning: already initialized constant GitlabCluster::CONFIG_PATH
/opt/gitlab/embedded/cookbooks/package/libraries/gitlab_cluster.rb:16: warning: previous definition of CONFIG_PATH was here
/opt/gitlab/embedded/cookbooks/cache/cookbooks/package/libraries/gitlab_cluster.rb:17: warning: already initialized constant GitlabCluster::JSON_FILE
/opt/gitlab/embedded/cookbooks/package/libraries/gitlab_cluster.rb:17: warning: previous definition of JSON_FILE was here
Loading Cinc Auditor profile files:
Loading Cinc Auditor input files:
Loading Cinc Auditor waiver files:
[2025-03-19T08:02:13+00:00] INFO: Generating default secrets
[2025-03-19T08:02:14+00:00] INFO: Generating /etc/gitlab/gitlab-secrets.json file
[2025-03-19T08:02:14+00:00] WARN: * git_data_dirs has been deprecated since 17.8 and will be removed in 18.0. See https://docs.gitlab.com/omnibus/settings/configuration.html#migrating-from-git_data_dirs for migration instructions.
Recipe: gitlab::default
  * directory[/etc/gitlab] action create (up to date)
[2025-03-19T08:02:14+00:00] WARN: gitlab-rails does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] INFO: Skipped selecting an init system because it was explicitly disabled
[2025-03-19T08:02:14+00:00] WARN: gitlab-shell does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-sshd does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: logrotate does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: logrotate does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: puma does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-rails does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-shell does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-workhorse does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-pages does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-kas does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitaly does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: mailroom does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitaly does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: postgresql does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: postgresql does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:14+00:00] WARN: gitlab-kas does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: puma does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: sidekiq does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: gitlab-workhorse does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: gitlab-exporter does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: redis-exporter does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: prometheus does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: alertmanager does not have a log_group or default logdir mode defined. Setting to 0700.
[2025-03-19T08:02:15+00:00] WARN: postgres-exporter does not have a log_group or default logdir mode defined. Setting to 0700.
  Converging 289 resources
  * directory[/etc/gitlab] action create (up to date)
  * directory[Create /var/opt/gitlab] action create (up to date)
  * directory[Create /var/log/gitlab] action create (up to date)
  * directory[/opt/gitlab/embedded/etc] action create (up to date)
  * template[/opt/gitlab/embedded/etc/gitconfig] action create (up to date)
Recipe: gitlab::web-server
  * account[Webserver user and group] action create (up to date)
Recipe: gitlab::users
  * directory[/var/opt/gitlab] action create (up to date)
  * account[GitLab user and group] action create (up to date)
  * template[/var/opt/gitlab/.gitconfig] action create (up to date)
  * directory[/var/opt/gitlab/.bundle] action create (up to date)
Recipe: gitaly::git_data_dirs
  * storage_directory[/var/opt/gitlab/git-data/repositories] action create
    * ruby_block[directory resource: /var/opt/gitlab/git-data/repositories] action run (skipped due to not_if)
     (up to date)
Recipe: gitlab::rails_pages_shared_path
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/pages] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/pages] action run (skipped due to not_if)
     (up to date)
Recipe: gitlab::gitlab-rails
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/artifacts] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/artifacts] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/external-diffs] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/external-diffs] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/lfs-objects] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/lfs-objects] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/packages] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/packages] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/dependency_proxy] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/dependency_proxy] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/terraform_state] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/terraform_state] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/ci_secure_files] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/ci_secure_files] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/encrypted_settings] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/encrypted_settings] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/uploads] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/uploads] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-ci/builds] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-ci/builds] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/cache] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/cache] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/var/opt/gitlab/gitlab-rails/shared/tmp] action create
    * ruby_block[directory resource: /var/opt/gitlab/gitlab-rails/shared/tmp] action run (skipped due to not_if)
     (up to date)
  * storage_directory[/opt/gitlab/embedded/service/gitlab-rails/public] action create (skipped due to only_if)
  * directory[create /var/opt/gitlab/gitlab-rails/etc] action create (up to date)
  * directory[create /opt/gitlab/etc/gitlab-rails] action create (up to date)
  * directory[create /var/opt/gitlab/gitlab-rails/working] action create (up to date)
  * directory[create /var/opt/gitlab/gitlab-rails/tmp] action create (up to date)
  * directory[create /var/opt/gitlab/gitlab-rails/upgrade-status] action create (up to date)
  * directory[/var/log/gitlab/gitlab-rails] action create (up to date)
  * storage_directory[/var/opt/gitlab/backups] action create
    * ruby_block[directory resource: /var/opt/gitlab/backups] action run (skipped due to not_if)
     (up to date)
  * directory[/var/opt/gitlab/gitlab-rails] action create (up to date)
  * directory[/var/opt/gitlab/gitlab-ci] action create (up to date)
  * file[/var/opt/gitlab/gitlab-rails/etc/gitlab-registry.key] action create (skipped due to only_if)
  * template[/opt/gitlab/etc/gitlab-rails-rc] action create (up to date)
  * file[/opt/gitlab/embedded/service/gitlab-rails/.secret] action delete (up to date)
  * file[/var/opt/gitlab/gitlab-rails/etc/secret] action delete (up to date)
  * templatesymlink[Create a database.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/database.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/database.yml to /var/opt/gitlab/gitlab-rails/etc/database.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a clickhouse.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/click_house.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/click_house.yml to /var/opt/gitlab/gitlab-rails/etc/click_house.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a secrets.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/secrets.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/secrets.yml to /var/opt/gitlab/gitlab-rails/etc/secrets.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a resque.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/resque.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/resque.yml to /var/opt/gitlab/gitlab-rails/etc/resque.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create an override redis.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/redis.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/redis.yml to /var/opt/gitlab/gitlab-rails/etc/redis.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a cable.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/cable.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/cable.yml to /var/opt/gitlab/gitlab-rails/etc/cable.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a redis.cache.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.cache.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.cache.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.queues.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.queues.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.queues.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.shared_state.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.shared_state.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.shared_state.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.trace_chunks.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.trace_chunks.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.trace_chunks.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.rate_limiting.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.rate_limiting.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.rate_limiting.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.sessions.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.sessions.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.sessions.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.repository_cache.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.repository_cache.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.repository_cache.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.cluster_rate_limiting.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.cluster_rate_limiting.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.cluster_rate_limiting.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a redis.workhorse.yml and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/redis.workhorse.yml] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/redis.workhorse.yml] action delete (up to date)
     (up to date)
  * templatesymlink[Create a session_store.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/session_store.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/session_store.yml to /var/opt/gitlab/gitlab-rails/etc/session_store.yml] action create (up to date)
     (up to date)
  * templatesymlink[Create a smtp_settings.rb and create a symlink to Rails root] action delete
    * file[/var/opt/gitlab/gitlab-rails/etc/smtp_settings.rb] action delete (up to date)
    * link[/opt/gitlab/embedded/service/gitlab-rails/config/initializers/smtp_settings.rb] action delete (up to date)
     (up to date)
  * templatesymlink[Create a gitlab.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab.yml] action create[2025-03-19T08:02:16+00:00] INFO: template[/var/opt/gitlab/gitlab-rails/etc/gitlab.yml] backed up to /opt/gitlab/embedded/cookbooks/cache/backup/var/opt/gitlab/gitlab-rails/etc/gitlab.yml.chef-20250319080216.005767
[2025-03-19T08:02:16+00:00] INFO: template[/var/opt/gitlab/gitlab-rails/etc/gitlab.yml] updated file contents /var/opt/gitlab/gitlab-rails/etc/gitlab.yml

      - update content in file /var/opt/gitlab/gitlab-rails/etc/gitlab.yml from 5ef6f4 to a82983
      - suppressed sensitive resource
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/gitlab.yml to /var/opt/gitlab/gitlab-rails/etc/gitlab.yml] action create (up to date)

  * templatesymlink[Create a gitlab_workhorse_secret and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab_workhorse_secret] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/.gitlab_workhorse_secret to /var/opt/gitlab/gitlab-rails/etc/gitlab_workhorse_secret] action create (up to date)
     (up to date)
  * templatesymlink[Create a gitlab_shell_secret and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab_shell_secret] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/.gitlab_shell_secret to /var/opt/gitlab/gitlab-rails/etc/gitlab_shell_secret] action create (up to date)
     (up to date)
  * templatesymlink[Create a gitlab_incoming_email_secret and create a symlink to Rails root] action create (skipped due to only_if)
  * templatesymlink[Create a gitlab_service_desk_email_secret and create a symlink to Rails root] action create (skipped due to only_if)
  * templatesymlink[Create a gitlab_pages_secret and create a symlink to Rails root] action create[2025-03-19T08:02:16+00:00] WARN: only_if block for templatesymlink[Create a gitlab_pages_secret and create a symlink to Rails root] returned a string, did you mean to run a command?

    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab_pages_secret] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/.gitlab_pages_secret to /var/opt/gitlab/gitlab-rails/etc/gitlab_pages_secret] action create (up to date)
     (up to date)
  * templatesymlink[Create a gitlab_kas_secret and create a symlink to Rails root] action create[2025-03-19T08:02:16+00:00] WARN: only_if block for templatesymlink[Create a gitlab_kas_secret and create a symlink to Rails root] returned a string, did you mean to run a command?

    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab_kas_secret] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/.gitlab_kas_secret to /var/opt/gitlab/gitlab-rails/etc/gitlab_kas_secret] action create (up to date)
     (up to date)
  * link[/opt/gitlab/embedded/service/gitlab-rails/config/initializers/relative_url.rb] action delete (up to date)
  * file[/var/opt/gitlab/gitlab-rails/etc/relative_url.rb] action delete (up to date)
  * env_dir[/opt/gitlab/etc/gitlab-rails/env] action create
    * directory[/opt/gitlab/etc/gitlab-rails/env] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/HOME] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/RAILS_ENV] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/BUNDLE_GEMFILE] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/PUMA_WORKER_MAX_MEMORY] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/SIDEKIQ_MEMORY_KILLER_MAX_RSS] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/PATH] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/ICU_DATA] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/PYTHONPATH] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/EXECJS_RUNTIME] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/TZ] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/SSL_CERT_DIR] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-rails/env/SSL_CERT_FILE] action create (up to date)
     (up to date)
  * link[/opt/gitlab/embedded/service/gitlab-rails/tmp] action create (up to date)
  * link[/opt/gitlab/embedded/service/gitlab-rails/public/uploads] action create (up to date)
  * link[/opt/gitlab/embedded/service/gitlab-rails/log] action create (up to date)
  * link[/var/log/gitlab/gitlab-rails/sidekiq.log] action delete (skipped due to only_if)
  * file[/opt/gitlab/embedded/service/gitlab-rails/db/structure.sql] action create (up to date)
  * remote_file[/var/opt/gitlab/gitlab-rails/VERSION] action create (up to date)
  * remote_file[/var/opt/gitlab/gitlab-rails/REVISION] action create (up to date)
  * version_file[Create version file for Rails] action create
    * file[/var/opt/gitlab/gitlab-rails/RUBY_VERSION] action create (up to date)
     (up to date)
  * execute[clear the gitlab-rails cache] action nothing (skipped due to action :nothing)
  * file[/var/opt/gitlab/gitlab-rails/config.ru] action delete (up to date)
Recipe: gitlab::selinux
  * bash[Set proper security context on ssh files for selinux] action nothing (skipped due to action :nothing)
Recipe: gitlab::add_trusted_certs
  * directory[/etc/gitlab/trusted-certs] action create (up to date)
  * directory[/opt/gitlab/embedded/ssl/certs] action create (up to date)
  * file[/opt/gitlab/embedded/ssl/certs/README] action create (up to date)
  * ruby_block[Move existing certs and link to /opt/gitlab/embedded/ssl/certs] action run (skipped due to only_if)
Recipe: gitlab::default
  * service[create a temporary puma service] action nothing (skipped due to action :nothing)
  * service[create a temporary sidekiq service] action nothing (skipped due to action :nothing)
  * service[create a temporary mailroom service] action nothing (skipped due to action :nothing)
Recipe: gitlab::gitlab-shell
  * storage_directory[/var/opt/gitlab/.ssh] action create
    * ruby_block[directory resource: /var/opt/gitlab/.ssh] action run (skipped due to not_if)
     (up to date)
  * directory[/var/opt/gitlab/gitlab-shell] action create (up to date)
  * directory[/var/log/gitlab/gitlab-shell] action create (up to date)
  * bash[generate gitlab-sshd host keys] action run (skipped due to only_if)
  * templatesymlink[Create a config.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-shell/config.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-shell/config.yml to /var/opt/gitlab/gitlab-shell/config.yml] action create (up to date)
     (up to date)
  * link[/opt/gitlab/embedded/service/gitlab-shell/.gitlab_shell_secret] action create (up to date)
  * file[/var/opt/gitlab/.ssh/authorized_keys] action create_if_missing (up to date)
  * env_dir[/opt/gitlab/etc/gitlab-sshd/env] action nothing (skipped due to action :nothing)
  * service[gitlab-sshd] action nothing (skipped due to action :nothing)
  * runit_service[gitlab-sshd] action disable
    * ruby_block[disable gitlab-sshd] action run (skipped due to only_if)
     (up to date)
Recipe: package::sysctl
  * execute[reload all sysctl conf] action nothing (skipped due to action :nothing)
Recipe: logrotate::folders_and_configs
  * directory[/var/opt/gitlab/logrotate] action create (up to date)
  * directory[/var/opt/gitlab/logrotate/logrotate.d] action create (up to date)
  * directory[/var/log/gitlab/logrotate] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.conf] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/nginx] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/puma] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitlab-rails] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitlab-shell] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitlab-workhorse] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitlab-pages] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitlab-kas] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/gitaly] action create (up to date)
  * template[/var/opt/gitlab/logrotate/logrotate.d/mailroom] action create (up to date)
Recipe: logrotate::enable
  * service[logrotate] action nothing (skipped due to action :nothing)
  * runit_service[logrotate] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/logrotate] action create (up to date)
    * template[/opt/gitlab/sv/logrotate/run] action create (up to date)
    * directory[/opt/gitlab/sv/logrotate/log] action create (up to date)
    * directory[/opt/gitlab/sv/logrotate/log/main] action create (up to date)
    * template[/opt/gitlab/sv/logrotate/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_logrotate] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/logrotate/config] action create (up to date)
    * template[/opt/gitlab/sv/logrotate/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/logrotate/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for logrotate service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/logrotate/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/logrotate/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/logrotate/control] action create (up to date)
    * template[/opt/gitlab/sv/logrotate/control/t] action create (up to date)
    * link[/opt/gitlab/init/logrotate] action create (up to date)
    * file[/opt/gitlab/sv/logrotate/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/logrotate] action create (up to date)
    * ruby_block[wait for logrotate service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/logrotate/current] action touch (skipped due to only_if)
     (up to date)
Recipe: redis::enable
  * redis_service[redis] action create[2025-03-19T08:02:16+00:00] WARN: redis does not have a log_group or default logdir mode defined. Setting to 0700.

    * account[user and group for redis] action create (up to date)
    * group[Socket group] action create (up to date)
    * directory[/var/opt/gitlab/redis] action create (up to date)
    * directory[/var/log/gitlab/redis] action create (up to date)
    * template[/var/opt/gitlab/redis/redis.conf] action create (up to date)
    * service[redis] action nothing (skipped due to action :nothing)
    * runit_service[redis] action enable
      * ruby_block[restart_service] action nothing (skipped due to action :nothing)
      * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
      * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
      * directory[/opt/gitlab/sv/redis] action create (up to date)
      * template[/opt/gitlab/sv/redis/run] action create (up to date)
      * directory[/opt/gitlab/sv/redis/log] action create (up to date)
      * directory[/opt/gitlab/sv/redis/log/main] action create (up to date)
      * template[/opt/gitlab/sv/redis/log/config] action create (up to date)
      * ruby_block[verify_chown_persisted_on_redis] action nothing (skipped due to action :nothing)
      * link[/var/log/gitlab/redis/config] action create (up to date)
      * template[/opt/gitlab/sv/redis/log/run] action create (up to date)
      * directory[/opt/gitlab/sv/redis/env] action create (up to date)
      * ruby_block[Delete unmanaged env files for redis service] action run (skipped due to only_if)
      * template[/opt/gitlab/sv/redis/check] action create (skipped due to only_if)
      * template[/opt/gitlab/sv/redis/finish] action create (skipped due to only_if)
      * directory[/opt/gitlab/sv/redis/control] action create (up to date)
      * link[/opt/gitlab/init/redis] action create (up to date)
      * file[/opt/gitlab/sv/redis/down] action nothing (skipped due to action :nothing)
      * directory[/opt/gitlab/service] action create (up to date)
      * link[/opt/gitlab/service/redis] action create (up to date)
      * ruby_block[wait for redis service socket] action run (skipped due to not_if)
      * file[/var/log/gitlab/redis/current] action touch (skipped due to only_if)
       (up to date)
    * ruby_block[warn pending redis restart] action run (skipped due to only_if)
     (up to date)
  * template[/opt/gitlab/etc/gitlab-redis-cli-rc] action create (up to date)
Recipe: gitaly::enable
  * directory[/var/opt/gitlab/gitaly] action create (up to date)
  * directory[/var/opt/gitlab/gitaly/run] action create (up to date)
  * directory[/var/log/gitlab/gitaly] action create (up to date)
  * directory[/var/opt/gitlab/gitaly/internal_sockets] action delete (up to date)
  * env_dir[/opt/gitlab/etc/gitaly/env] action create
    * directory[/opt/gitlab/etc/gitaly/env] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/HOME] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/PATH] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/TZ] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/PYTHONPATH] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/ICU_DATA] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/SSL_CERT_DIR] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/GITALY_PID_FILE] action create (up to date)
    * file[/opt/gitlab/etc/gitaly/env/WRAPPER_JSON_LOGGING] action create (up to date)
     (up to date)
  * file[/var/opt/gitlab/gitaly/.gitlab_secret] action create (up to date)
  * template[Create Gitaly config.toml] action create (up to date)
  * service[gitaly] action nothing (skipped due to action :nothing)
  * runit_service[gitaly] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/gitaly] action create (up to date)
    * template[/opt/gitlab/sv/gitaly/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitaly/log] action create (up to date)
    * directory[/opt/gitlab/sv/gitaly/log/main] action create (up to date)
    * template[/opt/gitlab/sv/gitaly/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_gitaly] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/gitaly/config] action create (up to date)
    * template[/opt/gitlab/sv/gitaly/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitaly/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for gitaly service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/gitaly/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/gitaly/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/gitaly/control] action create (up to date)
    * link[/opt/gitlab/init/gitaly] action create (up to date)
    * file[/opt/gitlab/sv/gitaly/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/gitaly] action create (up to date)
    * ruby_block[wait for gitaly service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/gitaly/current] action touch (skipped due to only_if)
     (up to date)
  * version_file[Create version file for Gitaly] action create
    * file[/var/opt/gitlab/gitaly/VERSION] action create (up to date)
     (up to date)
  * consul_service[gitaly] action delete
    * file[/var/opt/gitlab/consul/config.d/gitaly-service.json] action delete (up to date)
     (up to date)
Recipe: postgresql::bin
  * ruby_block[check_postgresql_version] action run (skipped due to not_if)
  * ruby_block[check_postgresql_version_is_deprecated] action run (skipped due to not_if)
  * ruby_block[Link postgresql bin files to the correct version] action run (skipped due to only_if)
  * template[/opt/gitlab/etc/gitlab-psql-rc] action create (up to date)
Recipe: postgresql::user
  * account[Postgresql user and group] action create (up to date)
  * directory[/var/opt/gitlab/postgresql] action create (up to date)
  * file[/var/opt/gitlab/postgresql/.profile] action create (up to date)
Recipe: postgresql::sysctl
  * gitlab_sysctl[kernel.shmmax] action create
    * directory[create /etc/sysctl.d for kernel.shmmax] action create (skipped due to only_if)
    * file[create /opt/gitlab/embedded/etc/90-omnibus-gitlab-kernel.shmmax.conf kernel.shmmax] action create (skipped due to only_if)
    * link[/etc/sysctl.d/90-omnibus-gitlab-kernel.shmmax.conf] action create (skipped due to only_if)
    * execute[load sysctl conf kernel.shmmax] action nothing (skipped due to action :nothing)
     (up to date)
  * gitlab_sysctl[kernel.shmall] action create
    * directory[create /etc/sysctl.d for kernel.shmall] action create (skipped due to only_if)
    * file[create /opt/gitlab/embedded/etc/90-omnibus-gitlab-kernel.shmall.conf kernel.shmall] action create (skipped due to only_if)
    * link[/etc/sysctl.d/90-omnibus-gitlab-kernel.shmall.conf] action create (skipped due to only_if)
    * execute[load sysctl conf kernel.shmall] action nothing (skipped due to action :nothing)
     (up to date)
  * gitlab_sysctl[kernel.sem] action create
    * directory[create /etc/sysctl.d for kernel.sem] action create (skipped due to only_if)
    * file[create /opt/gitlab/embedded/etc/90-omnibus-gitlab-kernel.sem.conf kernel.sem] action create (skipped due to only_if)
    * link[/etc/sysctl.d/90-omnibus-gitlab-kernel.sem.conf] action create (skipped due to only_if)
    * execute[load sysctl conf kernel.sem] action nothing (skipped due to action :nothing)
     (up to date)
Recipe: postgresql::enable
  * directory[/var/opt/gitlab/postgresql] action create (up to date)
  * directory[/var/opt/gitlab/postgresql/data] action create (up to date)
  * directory[/var/opt/gitlab/postgresql/data] action create (up to date)
  * directory[/var/log/gitlab/postgresql] action create (up to date)
  * execute[/opt/gitlab/embedded/bin/initdb -D /var/opt/gitlab/postgresql/data -E UTF8] action run (skipped due to not_if)
  * file[/var/opt/gitlab/postgresql/data/server.crt] action create (up to date)
  * file[/var/opt/gitlab/postgresql/data/server.key] action create (up to date)
  * postgresql_config[gitlab] action create
    * template[/var/opt/gitlab/postgresql/data/postgresql.conf] action create (up to date)
    * template[/var/opt/gitlab/postgresql/data/runtime.conf] action create (up to date)
    * template[/var/opt/gitlab/postgresql/data/pg_hba.conf] action create (up to date)
    * template[/var/opt/gitlab/postgresql/data/pg_ident.conf] action create (up to date)
     (up to date)
Recipe: postgresql::standalone
  * service[postgresql] action nothing (skipped due to action :nothing)
  * runit_service[postgresql] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/postgresql] action create (up to date)
    * template[/opt/gitlab/sv/postgresql/run] action create (up to date)
    * directory[/opt/gitlab/sv/postgresql/log] action create (up to date)
    * directory[/opt/gitlab/sv/postgresql/log/main] action create (up to date)
    * template[/opt/gitlab/sv/postgresql/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_postgresql] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/postgresql/config] action create (up to date)
    * template[/opt/gitlab/sv/postgresql/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/postgresql/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for postgresql service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/postgresql/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/postgresql/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/postgresql/control] action create (up to date)
    * template[/opt/gitlab/sv/postgresql/control/t] action create (up to date)
    * link[/opt/gitlab/init/postgresql] action create (up to date)
    * file[/opt/gitlab/sv/postgresql/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/postgresql] action create (up to date)
    * ruby_block[wait for postgresql service socket] action run (skipped due to not_if)
    * directory[/opt/gitlab/service/postgresql/supervise] action create (up to date)
    * directory[/opt/gitlab/service/postgresql/log/supervise] action create (up to date)
    * file[/opt/gitlab/service/postgresql/supervise/ok] action touch (skipped due to only_if)
    * file[/opt/gitlab/service/postgresql/log/supervise/ok] action touch (skipped due to only_if)
    * file[/opt/gitlab/service/postgresql/supervise/status] action touch[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/supervise/status] owner changed to 996
[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/supervise/status] group changed to 996

      - change owner from 'root' to 'gitlab-psql'
      - change group from 'root' to 'gitlab-psql'[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/supervise/status] updated atime and mtime to 2025-03-19 08:02:17 +0000

      - update utime on file /opt/gitlab/service/postgresql/supervise/status
    * file[/opt/gitlab/service/postgresql/log/supervise/status] action touch[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/log/supervise/status] owner changed to 996
[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/log/supervise/status] group changed to 996

      - change owner from 'root' to 'gitlab-psql'
      - change group from 'root' to 'gitlab-psql'[2025-03-19T08:02:17+00:00] INFO: file[/opt/gitlab/service/postgresql/log/supervise/status] updated atime and mtime to 2025-03-19 08:02:17 +0000

      - update utime on file /opt/gitlab/service/postgresql/log/supervise/status
    * file[/opt/gitlab/service/postgresql/supervise/control] action touch (skipped due to only_if)
    * file[/opt/gitlab/service/postgresql/log/supervise/control] action touch (skipped due to only_if)
    * file[/var/log/gitlab/postgresql/current] action touch (skipped due to only_if)

  * database_objects[postgresql] action create
    * postgresql_user[gitlab] action create
      * execute[create gitlab postgresql user] action run (skipped due to not_if)
       (up to date)
    * postgresql_user[gitlab_replicator] action create
      * execute[create gitlab_replicator postgresql user] action run (skipped due to not_if)
      * execute[set options for gitlab_replicator postgresql user] action run (skipped due to not_if)
       (up to date)
    * postgresql_database[gitlabhq_production] action create
      * execute[create database gitlabhq_production] action run (skipped due to not_if)
       (up to date)
    * postgresql_extension[pg_trgm] action enable
      * postgresql_query[enable pg_trgm extension] action run (skipped due to only_if)
       (up to date)
    * postgresql_extension[btree_gist] action enable
      * postgresql_query[enable btree_gist extension] action run (skipped due to only_if)
       (up to date)
    * postgresql_database[gitlabhq_production] action create
      * execute[create database gitlabhq_production] action run (skipped due to not_if)
       (up to date)
    * postgresql_extension[pg_trgm] action enable
      * postgresql_query[enable pg_trgm extension] action run (skipped due to only_if)
       (up to date)
    * postgresql_extension[btree_gist] action enable
      * postgresql_query[enable btree_gist extension] action run (skipped due to only_if)
       (up to date)
     (up to date)
  * version_file[Create version file for PostgreSQL] action create
    * file[/var/opt/gitlab/postgresql/VERSION] action create (up to date)
     (up to date)
  * ruby_block[warn pending postgresql restart] action run (skipped due to only_if)
  * execute[reload postgresql] action nothing (skipped due to action :nothing)
  * execute[start postgresql] action nothing (skipped due to action :nothing)
Recipe: praefect::disable
  * service[praefect] action nothing (skipped due to action :nothing)
  * runit_service[praefect] action disable
    * ruby_block[disable praefect] action run (skipped due to only_if)
     (up to date)
  * consul_service[praefect] action delete
    * file[/var/opt/gitlab/consul/config.d/praefect-service.json] action delete (up to date)
     (up to date)
Recipe: gitlab-kas::enable
  * directory[/var/opt/gitlab/gitlab-kas] action create (up to date)
  * directory[/opt/gitlab/etc/gitlab-kas] action create (up to date)
  * directory[/var/log/gitlab/gitlab-kas] action create (up to date)
  * version_file[Create version file for Gitlab KAS] action create
    * file[/var/opt/gitlab/gitlab-kas/VERSION] action create (up to date)
     (up to date)
  * file[/var/opt/gitlab/gitlab-kas/authentication_secret_file] action create (up to date)
  * file[/var/opt/gitlab/gitlab-kas/private_api_authentication_secret_file] action create (up to date)
  * file[/var/opt/gitlab/gitlab-kas/websocket_token_secret_file] action create (up to date)
  * file[/var/opt/gitlab/gitlab-kas/redis_password_file] action create (skipped due to only_if)
  * file[/var/opt/gitlab/gitlab-kas/redis_sentinels_password_file] action create (skipped due to only_if)
  * template[/var/opt/gitlab/gitlab-kas/gitlab-kas-config.yml] action create (up to date)
  * env_dir[/opt/gitlab/etc/gitlab-kas/env] action create
    * directory[/opt/gitlab/etc/gitlab-kas/env] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-kas/env/SSL_CERT_DIR] action create (up to date)
     (up to date)
  * service[gitlab-kas] action nothing (skipped due to action :nothing)
  * runit_service[gitlab-kas] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/gitlab-kas] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-kas/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-kas/log] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-kas/log/main] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-kas/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_gitlab-kas] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/gitlab-kas/config] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-kas/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-kas/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for gitlab-kas service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-kas/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-kas/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/gitlab-kas/control] action create (up to date)
    * link[/opt/gitlab/init/gitlab-kas] action create (up to date)
    * file[/opt/gitlab/sv/gitlab-kas/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/gitlab-kas] action create (up to date)
    * ruby_block[wait for gitlab-kas service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/gitlab-kas/current] action touch (skipped due to only_if)
     (up to date)
Recipe: gitlab::database_migrations
  * ruby_block[check remote PG version] action nothing (skipped due to action :nothing)
  * rails_migration[gitlab-rails] action run[2025-03-19T08:02:19+00:00] WARN: gitlab-rails does not have a log_group or default logdir mode defined. Setting to 0700.

    * bash_hide_env[migrate gitlab-rails database] action run (skipped due to not_if)
     (up to date)
Recipe: crond::disable
  * service[crond] action nothing (skipped due to action :nothing)
  * runit_service[crond] action disable
    * ruby_block[disable crond] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab::puma
  * directory[/opt/gitlab/var/puma] action create (up to date)
  * directory[/var/log/gitlab/puma] action create (up to date)
  * directory[/var/opt/gitlab/gitlab-rails/sockets] action create (up to date)
  * puma_config[/var/opt/gitlab/gitlab-rails/etc/puma.rb] action create
    * directory[/var/opt/gitlab/gitlab-rails/etc] action create (up to date)
    * template[/var/opt/gitlab/gitlab-rails/etc/puma.rb] action create (up to date)
     (up to date)
  * service[puma] action nothing (skipped due to action :nothing)
  * runit_service[puma] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/puma] action create (up to date)
    * template[/opt/gitlab/sv/puma/run] action create (up to date)
    * directory[/opt/gitlab/sv/puma/log] action create (up to date)
    * directory[/opt/gitlab/sv/puma/log/main] action create (up to date)
    * template[/opt/gitlab/sv/puma/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_puma] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/puma/config] action create (up to date)
    * template[/opt/gitlab/sv/puma/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/puma/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for puma service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/puma/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/puma/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/puma/control] action create (up to date)
    * template[/opt/gitlab/sv/puma/control/t] action create (up to date)
    * template[/opt/gitlab/sv/puma/control/h] action create (up to date)
    * link[/opt/gitlab/init/puma] action create (up to date)
    * file[/opt/gitlab/sv/puma/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/puma] action create (up to date)
    * ruby_block[wait for puma service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/puma/current] action touch (skipped due to only_if)
     (up to date)
  * consul_service[rails] action delete
    * file[/var/opt/gitlab/consul/config.d/rails-service.json] action delete (up to date)
     (up to date)
  * gitlab_sysctl[net.core.somaxconn] action create
    * directory[create /etc/sysctl.d for net.core.somaxconn] action create (skipped due to only_if)
    * file[create /opt/gitlab/embedded/etc/90-omnibus-gitlab-net.core.somaxconn.conf net.core.somaxconn] action create (skipped due to only_if)
    * link[/etc/sysctl.d/90-omnibus-gitlab-net.core.somaxconn.conf] action create (skipped due to only_if)
    * execute[load sysctl conf net.core.somaxconn] action nothing (skipped due to action :nothing)
     (up to date)
Recipe: gitlab::sidekiq
  * sidekiq_service[sidekiq] action enable[2025-03-19T08:02:19+00:00] WARN: sidekiq does not have a log_group or default logdir mode defined. Setting to 0700.

    * directory[/var/log/gitlab/sidekiq] action create (up to date)
    * service[sidekiq] action nothing (skipped due to action :nothing)
    * runit_service[sidekiq] action enable
      * ruby_block[restart_service] action nothing (skipped due to action :nothing)
      * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
      * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
      * directory[/opt/gitlab/sv/sidekiq] action create (up to date)
      * template[/opt/gitlab/sv/sidekiq/run] action create (up to date)
      * directory[/opt/gitlab/sv/sidekiq/log] action create (up to date)
      * directory[/opt/gitlab/sv/sidekiq/log/main] action create (up to date)
      * template[/opt/gitlab/sv/sidekiq/log/config] action create (up to date)
      * ruby_block[verify_chown_persisted_on_sidekiq] action nothing (skipped due to action :nothing)
      * link[/var/log/gitlab/sidekiq/config] action create (up to date)
      * template[/opt/gitlab/sv/sidekiq/log/run] action create (up to date)
      * directory[/opt/gitlab/sv/sidekiq/env] action create (up to date)
      * ruby_block[Delete unmanaged env files for sidekiq service] action run (skipped due to only_if)
      * template[/opt/gitlab/sv/sidekiq/check] action create (skipped due to only_if)
      * template[/opt/gitlab/sv/sidekiq/finish] action create (skipped due to only_if)
      * directory[/opt/gitlab/sv/sidekiq/control] action create (up to date)
      * link[/opt/gitlab/init/sidekiq] action create (up to date)
      * file[/opt/gitlab/sv/sidekiq/down] action nothing (skipped due to action :nothing)
      * directory[/opt/gitlab/service] action create (up to date)
      * link[/opt/gitlab/service/sidekiq] action create (up to date)
      * ruby_block[wait for sidekiq service socket] action run (skipped due to not_if)
      * file[/var/log/gitlab/sidekiq/current] action touch (skipped due to only_if)
       (up to date)
     (up to date)
  * consul_service[sidekiq] action delete
    * file[/var/opt/gitlab/consul/config.d/sidekiq-service.json] action delete (up to date)
     (up to date)
Recipe: gitlab::gitlab-workhorse
  * directory[/var/opt/gitlab/gitlab-workhorse] action create (up to date)
  * directory[/var/opt/gitlab/gitlab-workhorse/sockets] action create (up to date)
  * directory[/var/log/gitlab/gitlab-workhorse] action create (up to date)
  * directory[/opt/gitlab/etc/gitlab-workhorse] action create (up to date)
  * env_dir[/opt/gitlab/etc/gitlab-workhorse/env] action create
    * directory[/opt/gitlab/etc/gitlab-workhorse/env] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-workhorse/env/PATH] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-workhorse/env/HOME] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-workhorse/env/SSL_CERT_DIR] action create (up to date)
     (up to date)
  * service[gitlab-workhorse] action nothing (skipped due to action :nothing)
  * runit_service[gitlab-workhorse] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/gitlab-workhorse] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-workhorse/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-workhorse/log] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-workhorse/log/main] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-workhorse/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_gitlab-workhorse] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/gitlab-workhorse/config] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-workhorse/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-workhorse/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for gitlab-workhorse service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-workhorse/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-workhorse/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/gitlab-workhorse/control] action create (up to date)
    * link[/opt/gitlab/init/gitlab-workhorse] action create (up to date)
    * file[/opt/gitlab/sv/gitlab-workhorse/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/gitlab-workhorse] action create (up to date)
    * ruby_block[wait for gitlab-workhorse service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/gitlab-workhorse/current] action touch (skipped due to only_if)
     (up to date)
  * consul_service[workhorse] action delete
    * file[/var/opt/gitlab/consul/config.d/workhorse-service.json] action delete (up to date)
     (up to date)
  * version_file[Create version file for Workhorse] action create
    * file[/var/opt/gitlab/gitlab-workhorse/VERSION] action create (up to date)
     (up to date)
  * template[/var/opt/gitlab/gitlab-workhorse/config.toml] action create (up to date)
Recipe: gitlab::mailroom_disable
  * service[mailroom] action nothing (skipped due to action :nothing)
  * runit_service[mailroom] action disable
    * ruby_block[disable mailroom] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab::nginx
  * directory[/var/opt/gitlab/nginx] action create (up to date)
  * directory[/var/opt/gitlab/nginx/conf] action create (up to date)
  * directory[/var/log/gitlab/nginx] action create (up to date)
  * link[/var/opt/gitlab/nginx/logs] action create (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-http.conf] action create (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-smartcard-http.conf] action delete (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-health.conf] action create (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-pages.conf] action delete (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-registry.conf] action delete (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-mattermost-http.conf] action delete (up to date)
  * template[/var/opt/gitlab/nginx/conf/gitlab-kas.conf] action delete (up to date)
  * template[/var/opt/gitlab/nginx/conf/nginx-status.conf] action create (up to date)
  * consul_service[nginx] action delete
    * file[/var/opt/gitlab/consul/config.d/nginx-service.json] action delete (up to date)
     (up to date)
  * template[/var/opt/gitlab/nginx/conf/nginx.conf] action create (up to date)
Recipe: nginx::enable
  * service[nginx] action nothing (skipped due to action :nothing)
  * runit_service[nginx] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/nginx] action create (up to date)
    * template[/opt/gitlab/sv/nginx/run] action create (up to date)
    * directory[/opt/gitlab/sv/nginx/log] action create (up to date)
    * directory[/opt/gitlab/sv/nginx/log/main] action create (up to date)
    * template[/opt/gitlab/sv/nginx/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_nginx] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/nginx/config] action create (up to date)
    * template[/opt/gitlab/sv/nginx/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/nginx/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for nginx service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/nginx/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/nginx/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/nginx/control] action create (up to date)
    * link[/opt/gitlab/init/nginx] action create (up to date)
    * file[/opt/gitlab/sv/nginx/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/nginx] action create (up to date)
    * ruby_block[wait for nginx service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/nginx/current] action touch (skipped due to only_if)
     (up to date)
  * version_file[Create version file for NGINX] action create
    * file[/var/opt/gitlab/nginx/VERSION] action create (up to date)
     (up to date)
  * execute[reload nginx] action nothing (skipped due to action :nothing)
Recipe: gitlab::remote-syslog_disable
  * service[remote-syslog] action nothing (skipped due to action :nothing)
  * runit_service[remote-syslog] action disable
    * ruby_block[disable remote-syslog] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab::storage-check_disable
  * service[storage-check] action nothing (skipped due to action :nothing)
  * runit_service[storage-check] action disable
    * ruby_block[disable storage-check] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab-pages::disable
  * service[gitlab-pages] action nothing (skipped due to action :nothing)
  * runit_service[gitlab-pages] action disable
    * ruby_block[disable gitlab-pages] action run (skipped due to only_if)
     (up to date)
Recipe: registry::disable
  * service[registry] action nothing (skipped due to action :nothing)
  * runit_service[registry] action disable
    * ruby_block[disable registry] action run (skipped due to only_if)
     (up to date)
Recipe: mattermost::disable
  * service[mattermost] action nothing (skipped due to action :nothing)
  * runit_service[mattermost] action disable
    * ruby_block[disable mattermost] action run (skipped due to only_if)
     (up to date)
Recipe: letsencrypt::disable
  * crond_job[letsencrypt-renew] action delete
    * file[/var/opt/gitlab/crond/letsencrypt-renew] action delete (up to date)
     (up to date)
Recipe: gitlab::gitlab-healthcheck
  * template[/opt/gitlab/etc/gitlab-healthcheck-rc] action create (up to date)
Recipe: monitoring::pgbouncer-exporter_disable
  * service[pgbouncer-exporter] action nothing (skipped due to action :nothing)
  * runit_service[pgbouncer-exporter] action disable
    * ruby_block[disable pgbouncer-exporter] action run (skipped due to only_if)
     (up to date)
Recipe: monitoring::node-exporter_disable
  * service[node-exporter] action nothing (skipped due to action :nothing)
  * runit_service[node-exporter] action disable
    * ruby_block[disable node-exporter] action run (skipped due to only_if)
     (up to date)
  * consul_service[node-exporter] action delete
    * file[/var/opt/gitlab/consul/config.d/node-exporter-service.json] action delete (up to date)
     (up to date)
Recipe: monitoring::gitlab-exporter
  * directory[/var/opt/gitlab/gitlab-exporter] action create (up to date)
  * directory[/var/log/gitlab/gitlab-exporter] action create (up to date)
  * env_dir[/opt/gitlab/etc/gitlab-exporter/env] action create
    * directory[/opt/gitlab/etc/gitlab-exporter/env] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/MALLOC_CONF] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/RUBY_GC_HEAP_INIT_SLOTS] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/RUBY_GC_HEAP_FREE_SLOTS_MIN_RATIO] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/RUBY_GC_HEAP_FREE_SLOTS_MAX_RATIO] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/SSL_CERT_DIR] action create (up to date)
    * file[/opt/gitlab/etc/gitlab-exporter/env/SSL_CERT_FILE] action create (up to date)
     (up to date)
  * template[/var/opt/gitlab/gitlab-exporter/gitlab-exporter.yml] action create (up to date)
  * version_file[Create version file for GitLab-Exporter] action create
    * file[/var/opt/gitlab/gitlab-exporter/RUBY_VERSION] action create (up to date)
     (up to date)
  * service[gitlab-exporter] action nothing (skipped due to action :nothing)
  * runit_service[gitlab-exporter] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/gitlab-exporter] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-exporter/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-exporter/log] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-exporter/log/main] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-exporter/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_gitlab-exporter] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/gitlab-exporter/config] action create (up to date)
    * template[/opt/gitlab/sv/gitlab-exporter/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/gitlab-exporter/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for gitlab-exporter service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-exporter/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/gitlab-exporter/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/gitlab-exporter/control] action create (up to date)
    * link[/opt/gitlab/init/gitlab-exporter] action create (up to date)
    * file[/opt/gitlab/sv/gitlab-exporter/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/gitlab-exporter] action create (up to date)
    * ruby_block[wait for gitlab-exporter service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/gitlab-exporter/current] action touch (skipped due to only_if)
     (up to date)
  * consul_service[gitlab-exporter] action delete
    * file[/var/opt/gitlab/consul/config.d/gitlab-exporter-service.json] action delete (up to date)
     (up to date)
Recipe: monitoring::redis-exporter
  * directory[/var/log/gitlab/redis-exporter] action create (up to date)
  * directory[/opt/gitlab/etc/redis-exporter/env] action create (up to date)
  * env_dir[/opt/gitlab/etc/redis-exporter/env] action create
    * directory[/opt/gitlab/etc/redis-exporter/env] action create (up to date)
    * file[/opt/gitlab/etc/redis-exporter/env/SSL_CERT_DIR] action create (up to date)
     (up to date)
  * service[redis-exporter] action nothing (skipped due to action :nothing)
  * runit_service[redis-exporter] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/redis-exporter] action create (up to date)
    * template[/opt/gitlab/sv/redis-exporter/run] action create (up to date)
    * directory[/opt/gitlab/sv/redis-exporter/log] action create (up to date)
    * directory[/opt/gitlab/sv/redis-exporter/log/main] action create (up to date)
    * template[/opt/gitlab/sv/redis-exporter/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_redis-exporter] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/redis-exporter/config] action create (up to date)
    * template[/opt/gitlab/sv/redis-exporter/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/redis-exporter/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for redis-exporter service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/redis-exporter/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/redis-exporter/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/redis-exporter/control] action create (up to date)
    * link[/opt/gitlab/init/redis-exporter] action create (up to date)
    * file[/opt/gitlab/sv/redis-exporter/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/redis-exporter] action create (up to date)
    * ruby_block[wait for redis-exporter service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/redis-exporter/current] action touch (skipped due to only_if)
     (up to date)
  * consul_service[redis-exporter] action delete
    * file[/var/opt/gitlab/consul/config.d/redis-exporter-service.json] action delete (up to date)
     (up to date)
Recipe: monitoring::user
  * account[Prometheus user and group] action create (up to date)
Recipe: monitoring::prometheus
  * directory[/var/opt/gitlab/prometheus] action create (up to date)
  * directory[/var/opt/gitlab/prometheus/rules] action create (up to date)
  * directory[/var/log/gitlab/prometheus] action create (up to date)
  * directory[/opt/gitlab/etc/prometheus/env] action create (up to date)
  * env_dir[/opt/gitlab/etc/prometheus/env] action create
    * directory[/opt/gitlab/etc/prometheus/env] action create (up to date)
    * file[/opt/gitlab/etc/prometheus/env/SSL_CERT_DIR] action create (up to date)
     (up to date)
  * execute[reload prometheus] action nothing (skipped due to action :nothing)
  * file[Prometheus config] action create (up to date)
  * service[prometheus] action nothing (skipped due to action :nothing)
  * runit_service[prometheus] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/prometheus] action create (up to date)
    * template[/opt/gitlab/sv/prometheus/run] action create (up to date)
    * directory[/opt/gitlab/sv/prometheus/log] action create (up to date)
    * directory[/opt/gitlab/sv/prometheus/log/main] action create (up to date)
    * template[/opt/gitlab/sv/prometheus/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_prometheus] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/prometheus/config] action create (up to date)
    * template[/opt/gitlab/sv/prometheus/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/prometheus/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for prometheus service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/prometheus/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/prometheus/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/prometheus/control] action create (up to date)
    * link[/opt/gitlab/init/prometheus] action create (up to date)
    * file[/opt/gitlab/sv/prometheus/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/prometheus] action create (up to date)
    * ruby_block[wait for prometheus service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/prometheus/current] action touch (skipped due to only_if)
     (up to date)
  * consul_service[prometheus] action delete
    * file[/var/opt/gitlab/consul/config.d/prometheus-service.json] action delete (up to date)
     (up to date)
  * template[/var/opt/gitlab/prometheus/rules/gitlab.rules] action create (up to date)
  * template[/var/opt/gitlab/prometheus/rules/node.rules] action create (up to date)
Recipe: monitoring::alertmanager
  * directory[/var/opt/gitlab/alertmanager] action create (up to date)
  * directory[/var/log/gitlab/alertmanager] action create (up to date)
  * directory[/opt/gitlab/etc/alertmanager/env] action create (up to date)
  * env_dir[/opt/gitlab/etc/alertmanager/env] action create
    * directory[/opt/gitlab/etc/alertmanager/env] action create (up to date)
    * file[/opt/gitlab/etc/alertmanager/env/SSL_CERT_DIR] action create (up to date)
     (up to date)
  * file[Alertmanager config] action create (up to date)
  * service[alertmanager] action nothing (skipped due to action :nothing)
  * runit_service[alertmanager] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/alertmanager] action create (up to date)
    * template[/opt/gitlab/sv/alertmanager/run] action create (up to date)
    * directory[/opt/gitlab/sv/alertmanager/log] action create (up to date)
    * directory[/opt/gitlab/sv/alertmanager/log/main] action create (up to date)
    * template[/opt/gitlab/sv/alertmanager/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_alertmanager] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/alertmanager/config] action create (up to date)
    * template[/opt/gitlab/sv/alertmanager/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/alertmanager/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for alertmanager service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/alertmanager/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/alertmanager/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/alertmanager/control] action create (up to date)
    * link[/opt/gitlab/init/alertmanager] action create (up to date)
    * file[/opt/gitlab/sv/alertmanager/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/alertmanager] action create (up to date)
    * ruby_block[wait for alertmanager service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/alertmanager/current] action touch (skipped due to only_if)
     (up to date)
Recipe: monitoring::postgres-exporter
  * directory[/var/log/gitlab/postgres-exporter] action create (up to date)
  * directory[/var/opt/gitlab/postgres-exporter] action create (up to date)
  * env_dir[/opt/gitlab/etc/postgres-exporter/env] action create
    * directory[/opt/gitlab/etc/postgres-exporter/env] action create (up to date)
    * file[/opt/gitlab/etc/postgres-exporter/env/SSL_CERT_DIR] action create (up to date)
    * file[/opt/gitlab/etc/postgres-exporter/env/DATA_SOURCE_NAME] action create (up to date)
     (up to date)
  * service[postgres-exporter] action nothing (skipped due to action :nothing)
  * runit_service[postgres-exporter] action enable
    * ruby_block[restart_service] action nothing (skipped due to action :nothing)
    * ruby_block[restart_log_service] action nothing (skipped due to action :nothing)
    * ruby_block[reload_log_service] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/sv/postgres-exporter] action create (up to date)
    * template[/opt/gitlab/sv/postgres-exporter/run] action create (up to date)
    * directory[/opt/gitlab/sv/postgres-exporter/log] action create (up to date)
    * directory[/opt/gitlab/sv/postgres-exporter/log/main] action create (up to date)
    * template[/opt/gitlab/sv/postgres-exporter/log/config] action create (up to date)
    * ruby_block[verify_chown_persisted_on_postgres-exporter] action nothing (skipped due to action :nothing)
    * link[/var/log/gitlab/postgres-exporter/config] action create (up to date)
    * template[/opt/gitlab/sv/postgres-exporter/log/run] action create (up to date)
    * directory[/opt/gitlab/sv/postgres-exporter/env] action create (up to date)
    * ruby_block[Delete unmanaged env files for postgres-exporter service] action run (skipped due to only_if)
    * template[/opt/gitlab/sv/postgres-exporter/check] action create (skipped due to only_if)
    * template[/opt/gitlab/sv/postgres-exporter/finish] action create (skipped due to only_if)
    * directory[/opt/gitlab/sv/postgres-exporter/control] action create (up to date)
    * link[/opt/gitlab/init/postgres-exporter] action create (up to date)
    * file[/opt/gitlab/sv/postgres-exporter/down] action nothing (skipped due to action :nothing)
    * directory[/opt/gitlab/service] action create (up to date)
    * link[/opt/gitlab/service/postgres-exporter] action create (up to date)
    * ruby_block[wait for postgres-exporter service socket] action run (skipped due to not_if)
    * file[/var/log/gitlab/postgres-exporter/current] action touch (skipped due to only_if)
     (up to date)
  * template[/var/opt/gitlab/postgres-exporter/queries.yaml] action create (up to date)
  * consul_service[postgres-exporter] action delete
    * file[/var/opt/gitlab/consul/config.d/postgres-exporter-service.json] action delete (up to date)
     (up to date)
Recipe: gitlab::gitlab-backup-cli_disable
  * template[/opt/gitlab/etc/gitlab-backup-cli-config.yml] action delete (up to date)
  * account[GitLab Backup User] action remove (up to date)
  * group[git] action manage (up to date)
  * group[gitlab-psql] action manage (up to date)
  * group[registry] action manage (up to date)
Recipe: gitlab::database_reindexing_disable
  * crond_job[database-reindexing] action delete
    * file[/var/opt/gitlab/crond/database-reindexing] action delete (up to date)
     (up to date)
Recipe: gitlab-ee::sentinel_disable
  * sentinel_service[redis] action disable
    * service[sentinel] action nothing (skipped due to action :nothing)
    * runit_service[sentinel] action disable
      * ruby_block[disable sentinel] action run (skipped due to only_if)
       (up to date)
    * file[/var/opt/gitlab/sentinel/sentinel.conf] action delete (up to date)
    * directory[/var/opt/gitlab/sentinel] action delete (up to date)
     (up to date)
Recipe: gitlab-ee::geo-postgresql_disable
  * service[geo-postgresql] action nothing (skipped due to action :nothing)
  * runit_service[geo-postgresql] action disable
    * ruby_block[disable geo-postgresql] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab-ee::geo-logcursor_disable
  * service[geo-logcursor] action nothing (skipped due to action :nothing)
  * runit_service[geo-logcursor] action disable
    * ruby_block[disable geo-logcursor] action run (skipped due to only_if)
     (up to date)
Recipe: consul::disable_daemon
  * service[consul] action nothing (skipped due to action :nothing)
  * runit_service[consul] action disable
    * ruby_block[disable consul] action run (skipped due to only_if)
     (up to date)
Recipe: pgbouncer::disable
  * service[pgbouncer] action nothing (skipped due to action :nothing)
  * runit_service[pgbouncer] action disable
    * ruby_block[disable pgbouncer] action run (skipped due to only_if)
     (up to date)
Recipe: patroni::disable
  * service[patroni] action nothing (skipped due to action :nothing)
  * runit_service[patroni] action disable
    * ruby_block[disable patroni] action run (skipped due to only_if)
     (up to date)
Recipe: spamcheck::disable
  * service[spamcheck] action nothing (skipped due to action :nothing)
  * runit_service[spamcheck] action disable
    * ruby_block[disable spamcheck] action run (skipped due to only_if)
     (up to date)
  * service[spam-classifier] action nothing (skipped due to action :nothing)
  * runit_service[spam-classifier] action disable
    * ruby_block[disable spam-classifier] action run (skipped due to only_if)
     (up to date)
Recipe: gitlab-ee::geo-secondary_disable
  * templatesymlink[Removes the geo database settings from database.yml and create a symlink to Rails root] action create
    * template[/var/opt/gitlab/gitlab-rails/etc/database.yml] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/config/database.yml to /var/opt/gitlab/gitlab-rails/etc/database.yml] action create (up to date)
     (up to date)
Recipe: gitlab-ee::suggested_reviewers
  * templatesymlink[Create a gitlab_suggested_reviewers_secret and create a symlink to Rails root] action create[2025-03-19T08:02:19+00:00] WARN: only_if block for templatesymlink[Create a gitlab_suggested_reviewers_secret and create a symlink to Rails root] returned a string, did you mean to run a command?

    * template[/var/opt/gitlab/gitlab-rails/etc/gitlab_suggested_reviewers_secret] action create (up to date)
    * link[Link /opt/gitlab/embedded/service/gitlab-rails/.gitlab_suggested_reviewers_secret to /var/opt/gitlab/gitlab-rails/etc/gitlab_suggested_reviewers_secret] action create (up to date)
     (up to date)
[2025-03-19T08:02:19+00:00] INFO: templatesymlink[Create a gitlab.yml and create a symlink to Rails root] sending restart action to runit_service[puma] (delayed)
Recipe: gitlab::puma
  * runit_service[puma] action restart (up to date)
[2025-03-19T08:02:19+00:00] INFO: templatesymlink[Create a gitlab.yml and create a symlink to Rails root] sending restart action to sidekiq_service[sidekiq] (delayed)
Recipe: gitlab::sidekiq
  * sidekiq_service[sidekiq] action restart
    * service[sidekiq] action nothing (skipped due to action :nothing)
    * runit_service[sidekiq] action restart (up to date)
     (up to date)
[2025-03-19T08:02:23+00:00] INFO: templatesymlink[Create a gitlab.yml and create a symlink to Rails root] sending run action to execute[clear the gitlab-rails cache] (delayed)
Recipe: gitlab::gitlab-rails
  * execute[clear the gitlab-rails cache] action run
[2025-03-19T08:02:47+00:00] INFO: execute[clear the gitlab-rails cache] ran successfully

    - execute /opt/gitlab/bin/gitlab-rake cache:clear
[2025-03-19T08:02:47+00:00] INFO: Cinc Client Run complete in 34.605791312 seconds

Running handlers:
[2025-03-19T08:02:47+00:00] INFO: Running report handlers
Running handlers complete
[2025-03-19T08:02:47+00:00] INFO: Report handlers complete
Infra Phase complete, 6/820 resources updated in 35 seconds

Deprecations:
* git_data_dirs has been deprecated since 17.8 and will be removed in 18.0. See https://docs.gitlab.com/omnibus/settings/configuration.html#migrating-from-git_data_dirs for migration instructions.

Update the configuration in your gitlab.rb file or GITLAB_OMNIBUS_CONFIG environment.

gitlab Reconfigured!
root@192:/etc/gitlab#
root@192:/etc/gitlab# [root@localhost ~]#
