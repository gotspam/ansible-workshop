Deploy Web Applications
=======================

You will create a playbook to deploy applications to previously provisioned webservers.

**Examine ansible inventory file**

Ansible works against multiple systems in your infrastructure at the same time.
It does this by selecting portions of systems listed in Ansible’s inventory.
For this lab, the ansible inventory file is ``inventory/hosts``.

#. Edit ansible inventory file and ensure it has the entries listed below.  This lab uses nano editor but you may substitute for vi or vim.

   - Type ``nano inventory/hosts`` and modify if necessary
   - Type the following into the ``inventory/hosts`` file

   .. code::

     [bigips]
     10.1.1.245

     [appservers]
     10.1.20.17 ansible_user=root
     10.1.20.20 ansible_user=root

     [webservers]
     10.1.20.15 ansible_user=root
     10.1.20.16 ansible_user=root

   - Type ``ctrl x`` then press ``enter`` to save file.

**Create Playbook to deploy web application**

#. Create a playbook ``newapp.yaml``.

   - Type ``nano playbooks/newapp.yaml``
   - Type the following into the ``playbooks/newapp.yaml`` file.


   .. code::

     ---

     - name: Deploy newapp
       hosts: webservers
       vars:
         valid_certs: no

       tasks:
         - name: deploy to server5
           copy:
             src: ../files/f5-hello-world-develop/customizations/blue.css
             dest: /var/www/server/5/css/custom.css
           tags: serv5

         - name: deploy to server6
           copy:
             src: ../files/f5-hello-world-develop/customizations/green.css
             dest: /var/www/server/6/css/custom.css
           tags: serv6

   - Type ``ctrl x`` and save file.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/newapp.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 500px

#. Verify results by browsing to websites.

   - Type ``curl http://10.1.20.15`` and ``curl http://10.1.20.16``.


   .. NOTE::

     The ``hosts: webservers`` referenced are located in the ``inventory/hosts``
     file.  These addresses are on an internal vlan and not accessible by external
     users.  Upon successful deployment of the app you must configure app services
     for these applications to be accessible externally.
