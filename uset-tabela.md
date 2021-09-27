### Arguments

The version 3 of USet script introduces command line arguments.
They can be used in conbination with interactive mode and configuration file. The command line arguments can overwrite settings in configuration file.

The following table show available argumants and variables they set, possible values and short description of what certain argument does.


| Argument                 | Coresponding Variable     | Boolean | Possible Values | Description                   |
| :---                     | :---                      | :---    | :---            | :---                          |
| --skip-interaction       | $skip_interaction         | true    | true, false     | Skips user interaction
| --language               | $conf_language            | `false` | `ISO 639-1`     | Set interface language
| --disable-colors         | $conf_disable_colors      | true    |                 | Enable or disable screen colors
| --skip-welcome           | $conf_skip_welcome_screen | true    |                 | Skips welcome screen
| --data-folder-name       | $conf_data_folder_name    | `false` | `set by user`   | Set data folder name
| --data-file-name         | $conf_data_file_name      | `false` | `set by user`   | Set data file name
| --sslinfo-file-name      | $conf_ssl_info_file_name  | `false` | `set by user`   | Set SSL-Info file name
| --dbinfo-file-name       | $conf_db_info_file_name   | `false` | `set by user`   |
| --install-php-extensions
| --install-programs
| --enable-apt-universe
| --install-webmin         | $conf_install_webmin      | `true`  | true, false     | Skip webmin installation
| --webmin-port            | $conf_webmin_port         | `false` | `set by user`   | Set Webmin port
| --webmin-ssl
| --install-imagemagick
| --wp-locale
| --wp-php-extensions
| --create-demo-index
| --create-phpinfo
| --adminer-buils
| --hostname               | $hostname                 | `false` | `set by user`   | Set server default domain
| --rootpass
| --unixuser
| --unixpass
| --mysqlrpass
| --email
| --server-type            | $server_type              | `false` | apache, nginx   | Choose server type