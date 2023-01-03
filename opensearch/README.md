# Opensearch

Pass this variables to the ansible playbook in this way.

-e OLD_OS_PATH_CONF="/path/name" | mandatory

-e OLD_OS_PATH_PLUGINS="/path/name"

-e CERT_PATH="/path/name"

-e CERT_NAME=name

-e NEW_CERT_PATH="/path/name"

-e PEM_PATH="/path/name"

-e NEW_PEM_PATH="/path/name"

-extra-vars='{ "PLUGINS_NAMES": [item1,item2,item3]}'

-e NEW_OS_VERSION=X.X.X | mandatory

-e DATA_PATH="/path/name"

-e LOG_PATH="/path/name"

-e SYSTEMD_SERVICE_NAME=NAME | mandatory
