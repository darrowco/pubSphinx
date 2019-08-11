.. pubSphinx documentation master file, created by
   sphinx-quickstart on Thu Aug  8 22:31:17 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Local ReadTheDocs, GitLab on Docker with Sync to GitHub
=======================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Install GitLab
==============

Create directories for GitLab volumes
-------------------------------------
.. code-block:: bash

  sudo mkdir /home/GitLab
   sudo chown -R rtd: /home/GitLab
   sudo chgrp -R 0777 /home/GitLab ### verify 07

Run GitLab with docker
----------------------
.. code-block:: bash

  sudo docker run --detach \
     --hostname 192.168.1.76 \
     --publish 443:443 \
     --publish 80:80 \
     --publish 53022:22 \
     --name gitlab \
     --restart always \
     --volume /home/gitlab/config:/etc/gitlab \
     --volume /home/gitlab/logs:/var/log/gitlab \
     --volume /home/gitlab/data:/var/opt/gitlab \
     gitlab/gitlab-ce:latest

Set gitlab.rb
-------------
.. code-block:: bash

  sudo nano /home/gitlab/config/gitlab.rb

    ### can be set to https
    ### self-signed cert will be used
    external_url 'http://192.168.1.76'
    ### external_url 'GENERATED_EXTERNAL_URL'

gitlab gui
----------

- Create a new user
- Login as the new user
- Top-RT: Settings
- Left: Access tokens

  :Name: rtd



* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

References
==========
