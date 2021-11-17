Production Deployment
=====================

Example how to deploy Qeeqbox Chameleon for production environment.

Prerequisites
-------------

Create a virtual machine named ``qeeqbox-chameleon-box`` with 2 NICs, for example:

    1. ``192.168.122.2``: For administration purpose such as SSH, tunnel Postgres, and bind Grafana.
    2. ``192.168.123.2``: For Honeypots pod.

.. note::

    Both NICs needed to be on different subnet. Otherwise, ``firewalld`` multiple zones doesn't work properly.

In box's ``/etc/ssh/sshd_config``, change ``#ListenAddress 0.0.0.0`` to ``ListenAddress 192.168.122.2``.

Create a ``firewalld`` zone for Honeypots
-----------------------------------------

Create a file ``/etc/firewalld/zones/honeypots.xml`` with the following lines:

.. code-block:: bash

    <?xml version="1.0" encoding="utf-8"?>
    <zone>
      <short>honeypots</short>
      <description>Zone for honeypots deployment from qeeqbox/chameleon.</description>
      <interface name="enp7s0"/>
      <port port="21" protocol="tcp"/>
      <port port="22" protocol="tcp"/>
      <port port="23" protocol="tcp"/>
      <port port="25" protocol="tcp"/>
      <port port="80" protocol="tcp"/>
      <port port="110" protocol="tcp"/>
      <port port="143" protocol="tcp"/>
      <port port="443" protocol="tcp"/>
      <port port="5432" protocol="tcp"/>
      <port port="445" protocol="tcp"/>
      <port port="1080" protocol="tcp"/>
      <port port="3306" protocol="tcp"/>
      <port port="5900" protocol="tcp"/>
      <port port="6379" protocol="tcp"/>
      <port port="8080" protocol="tcp"/>
      <port port="9200" protocol="tcp"/>
      <port port="1433" protocol="tcp"/>
      <port port="389" protocol="tcp"/>
    </zone>

.. note::

    Change NIC ``enp7s0`` according to your system.

Execute the following command to reload ``firewalld`` and apply changes:

.. code-block:: bash

    sudo firewall-cmd --reload

Check and make sure the correct NIC is assigned to zone ``honeypots``:

.. code-block:: bash

    sudo firewall-cmd --get-active-zones

Build Qeeqbox Honeypots image
-----------------------------

From the project root directory, ``cd`` into ``src/qeeqbox-chameleon`` and then execute the following command:

.. code-block:: bash

    cd src/qeeqbox-chameleon
    podman build -t qeeqbox/honeypots -f honeypots-Dockerfile --build-arg PORTS="21 22 23 25 80 110 143 389 443 445 1080 3306 5900 6379 8080 9200 1433 5432" .

Deploy Postgres
---------------

From the project root directory, ``cd`` into ``deployment/production/postgres``:

.. code-block:: bash

    cd deployment/production/postgres

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

    podman play kube --configmap configmaps/qeeqbox-chameleon-postgres.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-postgres-pod.yaml

Test postgres. Make sure the following command success:

.. code-block:: bash

    podman run -it --rm --network=host -e PGPASSWORD=abcde12345 docker.io/library/postgres:9.6 psql --username postgres --host 127.0.0.1 --port 9999 -c "\l"

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

From the project root directory, ``cd`` into ``deployment/production/redis``:

.. code-block:: bash

    cd deployment/production/redis

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

    podman play kube --seccomp-profile-root ./seccomp qeeqbox-chameleon-redis-pod.yaml

Test redis. Make sure the following command returns ``PONG``:

.. code-block:: bash

    podman run -it --network=host --rm docker.io/redis:6.2-alpine redis-cli -h 127.0.0.1 -p 6379 ping

Deploy Grafana
--------------

From the project root directory, ``cd`` into ``deployment/production/grafana``:

.. code-block:: bash

    cd deployment/production/grafana

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

    podman play kube --configmap configmaps/qeeqbox-chameleon-grafana.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-grafana-pod.yaml

Test Grafana deployment. Open your web-browser and go to http://localhost:3000.

Deploy Honeypots
----------------

From the project root directory, ``cd`` into ``deployment/production/honeypots``:

.. code-block:: bash

    cd deployment/production/honeypots

Create config files:

.. code-block:: bash

    cp -v configmaps/qeeqbox-chameleon-honeypots.yaml{.example,}
    cp -v configs/config.json{.example,}

.. note::

    Make sure to change IP address in ``configs/config.json`` file, for example ``"ip": "x.x.x.x"`` to ``"ip": "192.168.123.2"``. Also make sure to check the NIC name for the ``enp7s0`` and change according to your NIC.

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

    podman play kube --configmap configmaps/qeeqbox-chameleon-honeypots.yaml --seccomp-profile-root ./seccomp qeeqbox-chameleon-honeypots-pod.yaml

To view Honeypots logs, execute the following command:

.. code-block:: bash

    podman logs --follow qeeqbox-chameleon-honeypots-pod-srv01

.. note::

    Network sniff functionality doesn't work because rootless Podman doesn't have privileged on host network.

Testing
-------

Test MySQL:

.. code-block:: bash

    podman run -it --rm --network=host docker.io/library/mysql:latest mysql -utest -ptest --host 192.168.123.2 --port 3306

Test SSH:

.. code-block:: bash

    podman run -it --rm --network=host docker.io/linuxserver/openssh-server:latest ssh -p 22 root@192.168.123.2
    ssh -p 22 test@192.168.123.2

Test Elasticsearch:

.. code-block:: bash

    curl --user rdeniro:taxidriver -XPUT 'http://192.168.123.2:9200/idx'
