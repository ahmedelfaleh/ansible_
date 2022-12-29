# Opensearch Version Upgrading Process

## Refrence URL

<https://opensearch.org/docs/2.0/upgrade-to/upgrade-to/>

### Note

I quoted from the "How it works" guide in the upgrade tool too, as it a good reference to upgrading best practice.

<https://opensearch.org/docs/2.0/upgrade-to/upgrade-to/#how-it-works>

#### 1. Disable shard allocation to prevent Opensearch OSS from replicating shards as you shut down nodes

```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": "primaries"
  }
}
```

#### 2. Stop Opensearch on one node (rolling upgrade) or all nodes (cluster restart upgrade)

##### Rolloing Upgrade

Rolling upgrades let you shut down one node at a time for minimal disruption of service.

Rolling upgrades work between minor versions (e.g. 6.5 to 6.8) and also support a single path to the next major version (e.g. 6.8 to 7.10.2). Performing these upgrades might require intermediate upgrades to arrive at your desired version and can affect cluster performance as nodes leave and rejoin, but the cluster remains available throughout the process.

##### Cluster Restart Upgrade

Cluster restart upgrades require you to shut down all nodes, perform the upgrade, and restart the cluster.

Cluster restart upgrades work between minor versions (e.g. 6.5 to 6.8) and the next major version (for example, 6.x to 7.10.2). Cluster restart upgrades are faster to perform and require fewer intermediate upgrades, but require downtime.

So we are going to stop one node, I will follow **"rolling upgrade"** method

`# systemctl stop opensearch.service`

If the opensearch service is up and running by tarball installation, you have to search for the process and kill it

```
  # ps aux | grep opensearch
  # kill -9 {PID}
```

#### 3. Upgrade the node (rolling) or all nodes (cluster restart)

- Download the tarball file from this link, choose the version that you want to upgrade to.
  <https://opensearch.org/downloads.html>

- Extract the OpenSearch tarball to a new directory
  
  **Note**: Do not overwrite the current Opensearch config, data, and logs directories.
  
  `tar zxvf opensearch-1.3.6-linux-x64.tar.gz`

- Set the OPENSEARCH_PATH_CONF environment variable to the directory that contains opensearch.yml (e.g. /etc/opensearch).
  
  `export OPENSEARCH_PATH_CONF=/new/path/of/opensearch/conf-file_name`
- Port your settings from old opensearch.yml to the new opensearch.yml. At a minimum, specify cluster.name, node.name, discovery.seed_hosts, and cluster.initial_cluster_manager_nodes.

- (Optional) If you’re actively connecting to the cluster with legacy clients that check for a particular version number, such as Logstash OSS, add a compatibility setting to opensearch.yml:

  `compatibility.override_main_response_version: true`

- (Optional) Add your certificates to your config directory, add them to opensearch.yml, and initialize the security plugin.
  `# cp /old/path/config/{CERT_NAME}.crt /new/path/config/`

- As per **Upgradting tool** further steps

  ```
    # cp -ar /usr/share/opensearch/config/jvm.options* $OPENSEARCH_PATH_CONF/
    # cp -ar /usr/share/opensearch/config/opensearch.keystore $OPENSEARCH_PATH_CONF/
    # cp -ar /usr/share/opensearch/config/*pem $OPENSEARCH_PATH_CONF/
  ```

- In opensearch.yml, set path.data and path.logs. You might also want to disable the security plugin for now. opensearch.yml might look something like this:

```
  path.data: /usr/share/opensearch/data
  path.logs: /usr/share/opensearch/logs
  plugins.security.disabled: true
```
  
  - Start OpenSearch on the node (rolling) or all nodes (cluster restart).

    For the tarball, run `./bin/opensearch -d.`

  - Wait for the OpenSearch node to rejoin the cluster (rolling) or for the cluster to start (cluster restart). Check the _nodes summary to verify that all nodes are available and running the expected version:

  ##### Security plugin disabled
  
  `curl -XGET 'localhost:9200/_nodes/_all?pretty=true'`

  ##### Security plugin enabled

  `curl -XGET -k -u 'username:password' 'https://localhost:9200/_nodes/_all?pretty=true'`

  Specifically, check the nodes.<node_name>.version portion of the response. Also check _cat/indices?v for a green status on all indices.

- (Rolling) Repeat steps 2–5 until all nodes are using OpenSearch.

- After all nodes are using the new version, re-enable shard allocation:

```
  PUT _cluster/settings
  {
    "persistent": {
      "cluster.routing.allocation.enable": "all"
    }
  }
```

- Reconfigure the systemd configuation file **"opensearch.service"** to the new path of the new opensearch version.

- `# systemctl daemon-reload`

- `# systemctl status opensearch.service`