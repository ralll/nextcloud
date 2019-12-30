############
Descrition
############

************
Introduction
************

A docker-compose to create a Nextcloud server. The stack uses Postgres as database, and Nginx as webserver and reserve proxy.

This configuration is using HTTPS and the certificates are in a volume container. Follow `this steps <https://gitlab.com/raill/lets-encrypt-certificate-from-container>` for generating a letsencrypt certificate with container, or ajust the docker-compose file.

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

