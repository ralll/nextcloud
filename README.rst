############
Description
############

************
Introduction
************

This repository contains a docker-compose to create a Nextcloud server. The stack uses Postgres as database, Nginx as webserver and reserve proxy.

This configuration uses HTTPS, and the certificates for the HTTPS are storaged in a volume container. Follow `these steps <https://gitlab.com/raill/lets-encrypt-certificate-from-container>`_ for generating a letsencrypt certificate with container, or ajust the current docker-compose file to fit your scenario.

It is important to update
=========================

* the **POSTGRES_PASSWORD** variable due to security issues.
* the **cloud.example.com** address in the nginx.conf file to your real domain.

**********
Installion
**********

New service
===========

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

  docker-compose -p cloud up -d

Update service
==============

Do not Skip Major releases
--------------------------

The `Nextcloud documention <https://docs.nextcloud.com/server/latest/admin_manual/maintenance/upgrade.html>`_ recommends **do not** skipping major releases. So, if you intend to install the 18 release from the 16 one, for instance, then you must install the 17 as a intermediate process.

    *"It is best to keep your Nextcloud server upgraded regularly, and to install all point releases and major releases. Major releases are 11, 12, and 13. Point releases are intermediate releases for each major release. For example, 13.0.4 and 12.0.9 are point releases. Skipping major releases is not supported."*

Enable the maintence mode
-------------------------

Set the `maintenance mode <https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/occ_command.html?highlight=maintenance%20mode#maintenance-commands-label>`_ in the nextcloud container:

Check if the ``cloud_nextcloud_1`` is actual the name of the container running nextcloud. Run ``docker ps -a`` to verify.

.. code-block:: bash

    docker exec -u www-data -it cloud_nextcloud_1 sh

    php occ maintenance:mode --on
    
Press down the ``<CTRL>`` while press the ``p`` and then the ``q`` keys to leave the container without stopping. 

Remove the old containers
-------------------------

Remember to **Keep the volumes**.

.. code-block:: bash

   docker-compose -p cloud stop

   docker-compose -p cloud rm

Start the new service
---------------------

.. code-block:: bash

  docker-compose -p cloud up -d --build
  
Disable the maintenance mode
----------------------------

.. code-block:: bash

    docker exec -u www-data -it cloud_nextcloud_1 sh

    php occ maintenance:mode --off

Then continue the configurations in the site admin area.
    
**************
Remove service
**************

Containers
==========

From the same folder of the dockerfile and nginx configurations.

.. code-block:: bash

   docker-compose -p cloud stop

.. code-block:: bash

  docker-compose -p cloud rm

Volumes
=======

The **volumes** keep data after containers' releases updates.

Be sure before remove them.

Backup any usefull data from the volumes. The data will be lost after deleting the volumes.

.. code-block:: bash

  docker volume rm $(docker volume ls -q | grep cloud_)
