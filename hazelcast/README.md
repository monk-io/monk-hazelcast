# Monk & Hazelcast

This repository contains Monk.io template to deploy Hazelcast system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

[Set up Monk](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk Hazelcast repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-hazelcast

# and change directory to the monk-hazelcast  template folder
cd monk-hazelcast/hazelcast

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `hazelcast/variables` section

```yaml
  variables:
    hazelcast-management-image-tag: "latest"
    hazelcast-image-tag: "5.2.1"
    cluster-name: "local-cluster"
```

### Hazelcast Stack configuration files

You can find configuration files in `/files` directory in repository and can edit before the running kit. There is one configuration files which bind to the container while run monk-hazelcast kit

| Configuration File       | Format Used | Directory in Container                       | Purpose                                  |
| ------------------------ | ----------- | -------------------------------------------- | ---------------------------------------- |
| **hazelcast-config.yml** | yaml        | `/opt/hazelcast/config/hazelcast-config.yml` | Primary configuration file for Hazelcast |

## Template variables

| Variable                           | Description                         | Type   | Defaults      |
| ---------------------------------- | ----------------------------------- | ------ | ------------- |
| **hazelcast-management-image-tag** | Hazelcast management image version. | string | latest        |
| **hazelcast-image-tag**            | Hazelcast  image version.           | string | 5.2.1         |
| **cluster-name**                   | Hazelcast cluster name              | string | local-cluster |

## Local Deployment

| First clone the repository simply run below command after launching `monkd`: |
| :--------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  ├─🧩 hazelcast/hazelcast
 │  └─🧩 hazelcast/hazelcast-management
 ├─🔗 Process groups:
 │  └─🧩 hazelcast/stack
 └─⚙️ Entity instances:
    ├─🧩 hazelcast/hazelcast/metadata
    └─🧩 hazelcast/hazelcast-management/metadata
✔ All templates loaded successfully

➜  monk list -l hazelcast

✔ Got the list
Type      Template                        Repository  Version  Tags
runnable  hazelcast/hazelcast             local       -        self hosted, distributed systems, database
runnable  hazelcast/hazelcast-management  local       -        self hosted, distributed systems, database
group     hazelcast/stack                 local       -        -


➜   monk run hazelcast/stack

✔ Started local/hazelcast/stack

```

This will start the  Hazelcast with a Hazelcast management center.

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name hazelcast
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p aws -f <path/to/your-credentials-file>
✔ Provider added successfully

➜  monk cluster grow -p  aws
? Name of the new instance hazelcast-instance
? Tags (split by whitespace) hazelcast
? Region eu-north-1
? Instance type t3.large
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for aws is HDD (standard). Would you like to change it? Yes
? Disk type SSD (gp3)
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on aws... DONE
✔ Creating node: hazelcast-instance-1 DONE
✔ Initializing node: hazelcast-instance-1 DONE
✔ Creating node: hazelcast-instance-2 DONE
✔ Initializing node: hazelcast-instance-2 DONE
✔ Creating node: hazelcast-instance-3 DONE
✔ Initializing node: hazelcast-instance-3 DONE
✔ Connecting: hazelcast-instance-1 DONE
✔ Syncing peer: hazelcast-instance-1 DONE
✔ Connecting: hazelcast-instance-2 DONE
✔ Syncing peer: hazelcast-instance-2 DONE
✔ Connecting: hazelcast-instance-3 DONE
✔ Syncing peer: hazelcast-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  ├─🧩 hazelcast/hazelcast-management
 │  └─🧩 hazelcast/hazelcast
 ├─🔗 Process groups:
 │  └─🧩 hazelcast/stack
 └─⚙️ Entity instances:
    ├─🧩 hazelcast/hazelcast-management/metadata
    └─🧩 hazelcast/hazelcast/metadata
✔ All templates loaded successfully

➜  monk list -l hazelcast

✔ Got the list
Type      Template                        Repository  Version  Tags
runnable  hazelcast/hazelcast             local       -        self hosted, distributed systems, database
runnable  hazelcast/hazelcast-management  local       -        self hosted, distributed systems, database
group     hazelcast/stack                 local       -        -


➜  monk run hazelcast/stack
? Select tag to run [local/hazelcast/stack] on: hazelcast

✔ Started local/hazelcast/stack
```

## Logs & Shell

```bash
# show Elasticsearch logs
➜  monk logs -l 1000 -f hazelcast/hazelcast

# show Kibana logs
➜  monk logs -l 1000 -f hazelcast/hazelcast-management


# access shell in the container running Hazelcast
➜  monk shell hazelcast/hazelcast

# access shell in the container running Hazelcast management
➜  monk shell hazelcast/hazelcast-management

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge  --ii --rv --rs --no-confirm --rv --rs  hazelcast/hazelcast hazelcast/hazelcast-management hazelcast/stack

✔ hazelcast/hazelcast purged
✔ hazelcast/hazelcast-management purged
✔ hazelcast/stack purged
```
