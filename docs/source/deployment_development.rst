Development Deployment
======================

Example how to deploy Qeeqbox Chameleon for development environment.

Build Qeeqbox Honeypots image
-----------------------------

From the project root directory, ``cd`` into ``src/qeeqbox-chameleon`` and then execute the following command:

.. code-block:: bash

    cd src/qeeqbox-chameleon
    podman build -t qeeqbox/honeypots -f honeypots-Dockerfile --build-arg PORTS="21 22 23 25 80 110 143 389 443 445 1080 3306 5900 6379 8080 9200 1433 5432" .

Create Podman Network (the ``qeeqboxnet``) with no Internet access
------------------------------------------------------------------

Create ``~/.config/cni/net.d/qeeqboxnet.conflist`` file:

.. code-block:: yaml

    {
      "cniVersion": "0.4.0",
      "name": "qeeqboxnet",
      "plugins": [
        {
          "type": "bridge",
          "bridge": "cni-podman1",
          "isGateway": true,
          "ipMasq": false,
          "hairpinMode": true,
          "ipam": {
            "type": "host-local",
            "routes": [{ "dst": "0.0.0.0/0" }],
            "ranges": [
              [
                {
                  "subnet": "192.168.125.0/24",
                  "gateway": "192.168.125.1"
                }
              ]
            ]
          }
        },
        {
          "type": "portmap",
          "capabilities": {
            "portMappings": true
          }
        },
        {
          "type": "firewall"
        },
        {
          "type": "tuning"
        },
        {
          "type": "dnsname",
          "domainName": "qeeqboxnet"
        }
      ]
    }

.. note::

    If ``~/.config/cni/net.d/`` does not exists, create the directory using ``sudo mkdir -pv ~/.config/cni/net.d/``.

.. warning::

    Rename ``cni-podman1`` to ``cni-podman2`` and etc if the name is already used by other Podman deployments. Also make sure to change IP address for ``subnet`` and ``gateway`` if they are already exists in your existing deployments.

Notice that the ``"ipMasq": false,`` will ensure the ``qeeqboxnet`` will not have any Internet access.

Make sure the following command failed:

.. code-block:: bash

    podman run -it --rm --network=qeeqboxnet docker.io/curlimages/curl:latest https://google.com

Deploy Postgres
---------------

From the project root directory, ``cd`` into ``deployment/development/postgres``:

.. code-block:: bash

    cd deployment/development/postgres

Create config files:

.. code-block:: bash

    cp -v configmaps/qeeqbox-chameleon-postgres.yaml{.example,}
    cp -v configs/postgres.conf{.example,}
    chmod o+r configs/postgres.conf

Create pod file:

.. code-block:: bash

    cp -v qeeqbox-chameleon-postgres-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/qeeqbox_chameleon_postgres.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "qeeqbox_chameleon_postgres"

Deploy postgres:

.. code-block:: bash

    podman play kube --network qeeqboxnet --configmap configmaps/qeeqbox-chameleon-postgres.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-postgres-pod.yaml

Test postgres. Make sure the following command success:

.. code-block:: bash

    podman run -it --rm --network=qeeqboxnet -e PGPASSWORD=abcde12345 docker.io/library/postgres:9.6 psql --username postgres --host qeeqbox-chameleon-postgres-pod.qeeqboxnet --port 9999 -c "\l"

Create user and database for Honeypots and Grafana
--------------------------------------------------

Execute ``psql`` command from container ``qeeqbox-chameleon-postgres-pod-srv01``:

.. code-block:: bash

    podman exec -it qeeqbox-chameleon-postgres-pod-srv01 psql --port 9999 --username postgres --password

Create database and credentials for Chameleon:

.. code-block:: bash

    CREATE DATABASE chameleon;
    CREATE USER qeeqboxuser WITH ENCRYPTED PASSWORD 'abcde12345';
    ALTER USER qeeqboxuser CREATEDB;
    GRANT ALL PRIVILEGES ON DATABASE chameleon TO qeeqboxuser;
    ALTER DATABASE chameleon OWNER TO qeeqboxuser;

    CREATE DATABASE qeeqboxuser;
    GRANT ALL PRIVILEGES ON DATABASE qeeqboxuser TO qeeqboxuser;
    ALTER DATABASE qeeqboxuser OWNER TO qeeqboxuser;

Create database and credentials for Grafana:

.. code-block:: bash

    CREATE DATABASE grafanadb;
    CREATE USER grafana WITH ENCRYPTED PASSWORD 'abcde12345';
    GRANT ALL PRIVILEGES ON DATABASE grafanadb TO grafana;
    ALTER DATABASE grafanadb OWNER TO grafana;

Deploy Redis
------------

From the project root directory, ``cd`` into ``deployment/development/redis``:

.. code-block:: bash

    cd deployment/development/redis

Create pod file:

.. code-block:: bash

    cp -v qeeqbox-chameleon-redis-pod.yaml{.example,}

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/qeeqbox_chameleon_redis.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "qeeqbox_chameleon_redis"

Deploy redis:

.. code-block:: bash

    podman play kube --network qeeqboxnet --seccomp-profile-root ./seccomp qeeqbox-chameleon-redis-pod.yaml

Test redis. Make sure the following command returns ``PONG``:

.. code-block:: bash

    podman run -it --rm --network qeeqboxnet docker.io/redis:6.2-alpine redis-cli -h qeeqbox-chameleon-redis-pod -p 6379 ping

Deploy Grafana
--------------

From the project root directory, ``cd`` into ``deployment/development/grafana``:

.. code-block:: bash

    cd deployment/development/grafana

Create config files:

.. code-block:: bash

    cp -v configmaps/qeeqbox-chameleon-grafana.yaml{.example,}
    cp -v configs/chameleon.json{.example,}
    cp -v configs/dashboards.yml{.example,}
    cp -v configs/postgres.yml{.example,}

Create pod file:

.. code-block:: bash

    cp -v qeeqbox-chameleon-grafana-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/qeeqbox_chameleon_grafana.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "qeeqbox_chameleon_grafana"

Deploy grafana:

.. code-block:: bash

    podman play kube --network qeeqboxnet --configmap configmaps/qeeqbox-chameleon-grafana.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-grafana-pod.yaml

Test Grafana deployment. Open your web-browser and go to http://localhost:3000.

Deploy Honeypots
----------------

From the project root directory, ``cd`` into ``deployment/development/honeypots``:

.. code-block:: bash

    cd deployment/development/honeypots

Create config files:

.. code-block:: bash

    cp -v configmaps/qeeqbox-chameleon-honeypots.yaml{.example,}
    cp -v configs/config.json{.example,}

Create pod file:

.. code-block:: bash

    cp -v qeeqbox-chameleon-honeypots-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Load SELinux policy:

.. code-block:: bash

    sudo semodule -i selinux/qeeqbox_chameleon_honeypots.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "qeeqbox_chameleon_honeypots"

Execute the following command:

.. code-block:: bash

    podman play kube --network qeeqboxnet --configmap configmaps/qeeqbox-chameleon-honeypots.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-honeypots-pod.yaml

To view Honeypots logs, execute the following command:

.. code-block:: bash

    podman logs --follow qeeqbox-chameleon-honeypots-pod-srv01

Testing
-------

Test MySQL:

.. code-block:: bash

    podman run -it --rm --network=qeeqboxnet docker.io/library/mysql:latest mysql -utest -ptest --host qeeqbox-chameleon-honeypots-pod.qeeqboxnet --port 3306

Test SSH:

.. code-block:: bash

    podman run -it --rm --network=qeeqboxnet docker.io/linuxserver/openssh-server:latest ssh -p 22 root@qeeqbox-chameleon-honeypots-pod.qeeqboxnet

Filebeat deployment (optional)
------------------------------

Filebeat will be used to forward honeypots logs to Elastic Stack. You may skip this Section if you don't intend to push honeypots logs to Elastic Stack.

Prerequisites
~~~~~~~~~~~~~

Deploy Elastic Stack from the following projects:

    * `extra2000/elastic-elasticsearch-pod`_
    * `extra2000/elastic-kibana-pod`_
    * `extra2000/elastic-logstash-pod`_

.. _extra2000/elastic-elasticsearch-pod: https://github.com/extra2000/elastic-elasticsearch-pod

.. _extra2000/elastic-kibana-pod: https://github.com/extra2000/elastic-kibana-pod

.. _extra2000/elastic-logstash-pod: https://github.com/extra2000/elastic-logstash-pod

Put the following certificates according to the following lists:

    * Put ``beats-certificate-bundle`` directory into ``./secrets/``
    * Put ``elastic-ca.pem`` file into ``./secrets/``

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./secrets

Create Podman Network (the ``qeeqboxbeatsnet``)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create ``~/.config/cni/net.d/qeeqboxbeatsnet.conflist`` file:

.. code-block:: yaml

    {
      "cniVersion": "0.4.0",
      "name": "qeeqboxbeatsnet",
      "plugins": [
        {
          "type": "bridge",
          "bridge": "cni-podman2",
          "isGateway": true,
          "ipMasq": true,
          "hairpinMode": true,
          "ipam": {
            "type": "host-local",
            "routes": [{ "dst": "0.0.0.0/0" }],
            "ranges": [
              [
                {
                  "subnet": "192.168.126.0/24",
                  "gateway": "192.168.126.1"
                }
              ]
            ]
          }
        },
        {
          "type": "portmap",
          "capabilities": {
            "portMappings": true
          }
        },
        {
          "type": "firewall"
        },
        {
          "type": "tuning"
        },
        {
          "type": "dnsname",
          "domainName": "qeeqboxbeatsnet"
        }
      ]
    }

.. note::

    If ``~/.config/cni/net.d/`` does not exists, create the directory using ``sudo mkdir -pv ~/.config/cni/net.d/``.

.. warning::

    Rename ``cni-podman2`` to ``cni-podman3`` and etc if the name is already used by other Podman deployments. Also make sure to change IP address for ``subnet`` and ``gateway`` if they are already exists in your existing deployments.

Deploy Filebeat
~~~~~~~~~~~~~~~

From the project root directory, ``cd`` into ``deployment/development/filebeat``:

.. code-block:: bash

    cd deployment/development/filebeat

Create config files:

.. code-block:: bash

    cp -v configmaps/qeeqbox-chameleon-filebeat.yaml{.example,}
    cp -v configs/filebeat.yml{.example,}
    chmod go-w configs/filebeat.yml

.. note::

    Make sure to change the following configs in ``./configs/filebeat.yml``:

        * ``output.logstash.hosts``
        * ``monitoring.cluster_uuid``
        * ``monitoring.elasticsearch.hosts``
        * ``monitoring.elasticsearch.username``
        * ``monitoring.elasticsearch.password``

Create pod file:

.. code-block:: bash

    cp -v qeeqbox-chameleon-filebeat-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/qeeqbox_chameleon_filebeat.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "qeeqbox_chameleon_filebeat"

Deploy Filebeat:

.. code-block:: bash

    podman play kube --network qeeqboxbeatsnet --configmap configmaps/qeeqbox-chameleon-filebeat.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-filebeat-pod.yaml
