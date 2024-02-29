# Provision a host with Telegraf for Xronos applications

Provision a host with Telegraf configured for Xronos applications.

This role performs the following steps:

- Deploys Telegraf in a docker container
- Configures Telegraf to receive TCP and UDP influx data lines
- Configures an InfluxDB output plugin

## Requirements

Provisioning host:

- Ansible 2.15

Remote host:

- Ubuntu 22.04 or later
- docker

## Example playbook

```yaml
- hosts: telegraf
  gather_facts: true
  roles:
    - name: xronos_telegraf_ansible
      role: xronos_telegraf_ansible
      vars:
        telegraf_influxdb_url: "https://influxdb:8086" # <-- your influxdb URI here
        telegraf_influxdb_bucket: "default" # <-- your default influxdb bucket here
        telegraf_influxdb_org: "org" # <-- your influxdb organization here
        telegraf_influxdb_token: "supersecrettocken" # <-- your influxdb token here
```

After running, the following services should be running:

- telegraf: `http://telegraf:8186` TCP listener
- telegraf: `telegraf:8094` UDP listener

## Variables

- `deployment` (= `xronos` ): The name of this deployment. Used to prefix filesystem and docker resources so that more than once instance of this service may coexist on the same host.
- `telegraf_path` (= `/opt/{{ deployment }}/telegraf` ): The path to store telegraf configuration.
- `telegraf_docker_network`: Docker network in which to run the container.
- `telegraf_influxdb_url`: InfluxDB URL including port.
- `telegraf_influxdb_bucket`: InfluxDB default bucket.
- `telegraf_influxdb_org`: InfluxDB organization.
- `telegraf_influxdb_token`: Secret token for writing to InfluxDB.
