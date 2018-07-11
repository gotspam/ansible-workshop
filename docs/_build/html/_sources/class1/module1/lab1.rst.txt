Deploy Web Applications
=======================

You will create a playbook to deploy applications to previously provisioned webservers.

#. Edit ansible inventory file.

   - Type ``nano inventory/hosts``
   - Type the following into the ``inventory/hosts`` file

   .. code::

     [bigips]
     10.1.1.245

     [appservers]
     10.1.20.17 ansible_user=root
     10.1.20.20 ansible_user=root
     #10.1.1.252 ansible_user=root

     [webservers]
     10.1.20.15 ansible_user=root
     10.1.20.16 ansible_user=root

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
         - name: delete folder5
           file:
             path: /var/www/server/5/
             state: absent
           tags: serv5

         - name: create folder5
           file:
             path: /var/www/server/5/
             state: directory
           tags: serv5

         - name: deploy to server5
           copy:
             src: ../files/f5-hello-world-develop/hw/
             dest: /var/www/server/5/
           tags: serv5

         - name: delete folder6
           file:
             path: /var/www/server/6/
             state: absent
           tags: serv6

         - name: create folder6
           file:
             path: /var/www/server/6/
             state: directory
           tags: serv6

         - name: deploy to server6
           copy:
             src: ../files/f5-hello-world-develop/hw/
             dest: /var/www/server/6/
           tags: serv6

#. Run this playbook.

   - Type ``ansible-playbook playbooks/newapp.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 500px

#. Verify results by browsing to websites.

   - Type ``curl http://10.1.10.15`` and ``curl http://10.1.20.16``.


   .. NOTE::

     The ``hosts: webservers`` referenced are located in the ``inventory/hosts``
     file.  These addresses are on an internal vlan and not accessible by external
     users.  Upon successful deployment of the app you must configure app services
     for these applications to be accessible externally.
