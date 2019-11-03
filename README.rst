############
Descrition
############

************
Introduction
************

A docker-compose file to create a Nextcloud server. The stack uses Postgres as database, and Nginx as webserver and reserve proxy.

This version is for using in localhost, without SSL configured.

**********
Installion
**********

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

  docker-compose -p local_cloud up

******
Remove
******

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

  docker-compose -p local_cloud rm

Backup any usefull data in volumes. The will be lost after deleting them.

.. code-block:: bash

  docker volume rm $(docker volume ls -q | grep local_cloud)

