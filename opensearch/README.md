# Opensearch

Pass this variables to the ansible playbook in this way.

-e OLD_OS_PATH_CONF="/path/name" | mandatory

-e OLD_OS_PATH_PLUGINS="/path/name" | if plugins

-e CERT_PATH="/path/name" | if cert

-e CERT_NAME=name | if cert

-e NEW_CERT_PATH="/path/name" | if cert

-e PEM_PATH="/path/name" | if pem

-e NEW_PEM_PATH="/path/name" | if pem

-extra-vars='{ "PLUGINS_NAMES": [item1,item2,item3]}' | if plugins

-e NEW_OS_VERSION=X.X.X | mandatory

-e DATA_PATH="/path/name" | if change in config file

-e LOG_PATH="/path/name" | if change in config file

-e SYSTEMD_SERVICE_NAME=NAME | mandatory
