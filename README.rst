############
Descrition
############

************
Introduction
************

A docker-compose to create a Nextcloud server. The stack uses Postgres as database, Nginx as webserver and reserve proxy.

This configuration is using HTTPS and the certificates are in a volume container. Follow `this steps <https://gitlab.com/raill/lets-encrypt-certificate-from-container>`_ for generating a letsencrypt certificate with container, or ajust the current docker-compose file.

It is important to change:

* the **POSTGRES_PASSWORD** variable due to security issues.
* the **cloud.example.com** address in the nginx.conf file.

**********
Installion
**********

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

  docker-compose -p cloud up

******
Remove
******

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

   docker-compose -p cloud stop

.. code-block:: bash

  docker-compose -p cloud rm

Backup any usefull data in volumes. The will be lost after deleting them.

.. code-block:: bash

  docker volume rm $(docker volume ls -q | grep cloud_)

**************************
Update from older versions
**************************

Set the `maintenance mode <https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/occ_command.html?highlight=maintenance%20mode#maintenance-commands-label>`_ in the nextcloud container:

.. code-block:: bash

    docker exec -u www-data -it cloud_nextcloud_1 sh

    php occ maintenance:mode --off

The `Nextcloud documention <https://docs.nextcloud.com/server/latest/admin_manual/maintenance/upgrade.html>`_ recommends **do not** skipping major releases.

.. note::

    It is best to keep your Nextcloud server upgraded regularly, and to install all point releases and major releases. Major releases are 11, 12, and 13. Point releases are intermediate releases for each major release. For example, 13.0.4 and 12.0.9 are point releases. Skipping major releases is not supported.
