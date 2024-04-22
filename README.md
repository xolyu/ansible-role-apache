# apache &ndash; Apache HTTP Server

<!-- [![CI](https://github.com/xolyu/ansible-role-apache/actions/workflows/ci.yml/badge.svg)](https://github.com/xolyu/ansible-role-apache/actions/workflows/ci.yml) -->

Installs and configures Apache2. The role should provide a rudimentary startup configuration for Apache by disabling the default vhost and configuring the remoteip module for using behind a reverse proxy.


## Requirements

None.


## Dependencies

None.


## Role Variables

* **`apache_enabled`**  
  Defines if the Apache2 service should be enabled.  
  Type: bool  
  Default: `yes`

* **`apache_state`**  
  Apache2 can be started or stopped after performing the role.  
  Choices: `started`, `stopped`  
  Default: `started`

### Enable/Disable mechanisms

* **`apache_disable_default_vhost`**  
  Disables the default vhost (on Debian/Ubuntu: `000-default`).  
  Type: bool  
  Default: `yes`

* **`apache_mods_enabled`**  
  List of modules to be enabled. Create link for .load and corresponding .conf file if exist.  
  Type: list of str  
  Default: _undefined_

* **`apache_mods_disabled`**  
  List of modules to be disabled.  
  Type: list of str  
  Default: _undefined_

* **`apache_conf_enabled`**  
  List of configs to be enabled. Configs defined in `apache_configs` are enabled automatically.  
  Type: list of str  
  Default: _undefined_

* **`apache_conf_disabled`**  
  List of configs to be disabled.  
  Type: list of str  
  Default: _undefined_

* **`apache_sites_enabled`**  
  List of sites/vhosts to be enabled. Vhosts defined in `apache_simple_vhosts` are enabled automatically.  
  Type: list of str  
  Default: _undefined_

* **`apache_sites_disabled`**  
  List of sites/vhosts to be disabled.  
  Type: list of str  
  Default: _undefined_

* **`apache_fail_if_mod_not_exist`**  
  Defines if the play fails (`yes`) if the module to be enabled not exists or will be skipped (`no`).  
  Type: bool  
  Default: `yes`

* **`apache_fail_if_conf_not_exist`**  
  Defines if the play fails (`yes`) if the config to be enabled not exists or will be skipped (`no`).  
  Type: bool  
  Default: `yes`

* **`apache_fail_if_site_not_exist`**  
  Defines if the play fails (`yes`) if the site/vhost to be enabled not exists or will be skipped (`no`).  
  Type: bool  
  Default: `yes`

### General options

* **`apache_listen_ip`**  
  Listen IP address for Apache2. Is configured in `ports.conf` and vhosts.  
  Type: str  
  Default: `*`

* **`apache_listen_port_http`**  
  Listen port for HTTP.  
  Type: str or int  
  Default: `80`

* **`apache_listen_port_https`**  
  Listen port for HTTPS.  
  Type: str or int  
  Default: `443`

### Default values for Vhosts

* **`apache_directory_index`**  
  Default value for the `DirectoryIndex` directive.  
  Type: str or list of str  
  Default: `['index.php', 'index.html', 'index.htm']`

* **`apache_allow_override`**  
  Default value for the `AllowOverride` directive.  
  Type: str or list of str  
  Default: `All`

* **`apache_options`**  
  Default value for the `Options` directive.  
  Type: str or list of str  
  Default: `-Indexes +FollowSymLinks`

### General configurations and Vhosts

* **`apache_configs`**  
  Definition for own general configs. Saved in `conf-available`.  
  Type: dict  
  Default: _undefined_

  ```yml
  apache_configs:
    {conf_name}:
      content: |
        {content}
  ```

  * `conf_name`: Name of the file (without extension) in `conf-available` directory.
  * `content`: Content to print in conf file.

* **`apache_simple_vhosts`**  
  Definition for own vhosts. Saved in `sites-available`.  
  Type: list of dicts  
  Default: _undefined_

  Example:

  ```yml
  apache_simple_vhosts:
    - name: example       # required
      http: yes           # default: yes
      https: no           # default: no
      redirect_https: no  # default: no
      servername: example.com
      serveralias: www.example.com
      # serveradmin: webmaster@example.com
      # directory_index: main.php
      documentroot: /var/www/example_page
      # allow_override: All
      # options: +Indexes +FollowSymLinks
      # cert_file:        # required if https is activated
      # cert_keyfile:     # required if https is activated
      # cert_chainfile:   # optional
      extra_block: |
        AuthType Basic
        AuthName "secure area"
  ```

  Dict keys:
  * `name`: Name of the vhost. Required.  
    Is used as filename for the sites conf file and for the log files.
  * `http`: `yes` If vhost for HTTP should be configured otherwise `no`. Default: `yes`
  * `https`: `yes` If vhost for HTTPS should be configured otherwise `no`. Default: `no`
  * `redirect_https`: Redirects HTTP to HTTPS if it's enabled (`yes`). Default: `no`
  * `servername`, `serveralias`, `serveradmin` &ndash; is printed to vhost section.
  * `directory_index`: If unset, defaults to *apache_directory_index*.
  * `documentroot`: DocumentRoot path.
  * `allow_override`: If unset, defaults to *apache_allow_override*.
  * `options`: If unset, defaults to *apache_options*.
  * `cert_file`, `cert_keyfile`, `cert_chainfile` (optional): SSL/TLS cert files. Required for HTTPS.
  * `extra_block`: Raw content printed to vhost.

### Special configurations

* **`apache_trusted_proxy_ip`**  
  IP address of the reverse proxy server that forwards requests to Apache2. Required to log correct IP addresses.  
  If this variable is set, a config `remoteip` is automatically generated with this IP address and enabled. If this variable is no longer set, the `remoteip` config will be disabled automatically.  
  Type: str or list of str  
  Default: _undefined_


<!--
* **`VAR`**  
  DESC  
  Type: bool/str/dict/list/list of str/list of dicts/dict of dict  
  Default: `VAL`

* **`VAR`**  
  DESC  
  Choices: `VAL`, `ANOTHER`  
  Default: `VAL`
-->


## Example Playbook

Examples of playbooks using and configuring the role.


## License

GNU General Public License v3.0


## Author Information

Xolyu.
