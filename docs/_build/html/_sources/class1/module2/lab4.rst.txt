Create a Virtual Server
=======================

**Create a virtual server and assign pool**

You need to assign newly created nodes to a pool.  Use the ``bigip_virtual_server``
module.

#. Create a playbook ``vs.yaml``.

   - Type ``nano playbooks/vs.yaml``
   - Type the following into the ``playbooks/vs.yaml`` file.


     .. image:: /_static/image021.png
       :height: 500px

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/vs.yaml``

   If successful, you should see similar results

   .. image:: /_static/image022.png
       :height: 200px

#. Verify BIG-IP results.

   - Select **Local Traffic -> Virtual Servers -> Virtual Server List**

   .. image:: /_static/image023.png
       :height: 200px

   - Open browser to ``http://10.1.10.10``.

.. NOTE::

  The ``bigip_virtual_server`` module can configure a number of attributes for
  virtual server. At a minimum, the ``name`` is required.

  This module is idempotent. Therefore, you can run it over and over again
  and so-long as no settings have been changed, this module will report no
  changes.

  The module has several more options, all of which can be seen at `this link`_.

  .. _this link: https://docs.ansible.com/ansible/latest/modules/bigip_virtual_server_module.html
