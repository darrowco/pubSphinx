.. pubSphinx documentation master file, created by
   sphinx-quickstart on Thu Aug  8 22:31:17 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Local ReadTheDocs, GitLab on Docker with a Sync to GitHub
=========================================================

.. toctree::
  :maxdepth: 2
  :caption: Contents:

  Features

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

  sudo docker run --detach \\\
     --hostname 192.168.1.76 \\\
     --publish 443:443 \\\
     --publish 80:80 \\\
     --publish 53022:22 \\\
     --name gitlab \\\
     --restart always \\\
     --volume /home/gitlab/config:/etc/gitlab \\\
     --volume /home/gitlab/logs:/var/log/gitlab \\\
     --volume /home/gitlab/data:/var/opt/gitlab \\\
     gitlab/gitlab-ce:latest

Set gitlab.rb
-------------
.. code-block:: bash

  sudo nano /home/gitlab/config/gitlab.rb

    ### can be set to https (self-signed cert will be used)
    external_url 'http://192.168.1.76'
    ### external_url 'GENERATED_EXTERNAL_URL'

Config repository mirror to github.com
--------------------------------------

- projects/pubSphinx
- Left: Settings/repository
- Choose Mirroring repositories and enter data

  :Git repository URL: https://darrowco:pubSphinx@github.com/darrowco/pubSphinx.git
  :Mirror direction: Push
  :Authentication method: Password
  :Password: ...

- Button: Mirror repository

gitlab gui
----------

- Create a new user
- Login as the new user
- Top-Right: Settings
- Left: Access tokens

  :Name: rtd
  :Scope: read_repository

- Button: Create personal access token
- Copy the new tokens
- Create a new project

Create directories for GitLab volumes
-------------------------------------
.. code-block:: bash

  sudo useradd rtd
  sudo usermod -u 1005 rtd
  sudo groupmod -g 205 rtd
  id -G rtd
  sudo su rtd
  sudo mkdir /home/rtd
  sudo chown rtd: /home/rtd
  sudo chmod 0777 /home/rtd ### verify 07

Run ReadTheDocs with docker
---------------------------
.. code-block:: bash

  docker run -it --detach -v /home/rtd:/data \\\
  -p 8000:8000 \\\
  -e RTD_PRODUCTION_DOMAIN="localhost:8000" \\\
  --name readthedocs \\\
  --restart always \\\
  seblon/readthedocs-docker

Config RTF to read in Project from Gitlab Docker
----------------------------------

- Top-Right: Admin
- Button: Import a project
- Button: Import manually

  :Name: Gitlab on docker
  :Repository URL - Public: http://192.168.1.76/root/pubsphinx
  OR
  :Name: Gitlab on docker
  :Repository URL - Private: http://root:token@192.168.1.76/root/pubsphinx







* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

References
==========
