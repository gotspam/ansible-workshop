Creating a pool on BIG-IP
=========================

You need to create a pool on a BIG-IP.  Use the ``bigip_pool`` module.

**Create a pool on a BIG-IP**

#. Create a playbook ``pool.yaml``.

   - Type ``nano playbooks/pool.yaml``
   - Type the following into the ``playbooks/pool.yaml`` file.


     .. image:: /_static/image010.png
       :height: 300px

   - Ctrl x to save file.

   .. HINT::

      If you encounter syntax errors you can troubleshoot issues and/or the use of an online editor such as http://www.yamllint.com/


#. Run this playbook.

   - Type ``ansible-playbook playbooks/pool.yaml``

   If successful, you should see similar results

   .. image:: /_static/image011.png
       :height: 180px

#. Verify BIG-IP results.

   - Login to BIG-IP Admin GUI with username: ``admin`` and password: ``admin``
   - Select **Local Traffic -> Pools**

   .. image:: /_static/image012.png
       :height: 180px

.. NOTE::

   The ``bigip_pool`` module can configure a number of attributes for a pool.
   At a minimum, the ``name`` is required.

   This module is idempotent. Therefore, you can run it over and over again and
   so-long as no settings have been changed, this module will report no changes.

   Notice how we also included the credentials to log into the device as arguments
   to the task. This is *not* the preferred way to do this, but it illustrates a
   way for beginners to get started without needing to know a less obvious way to
   specify these values.

   The module has several more options, all of which can be seen at `this link`_.
   I have reproduced them below. These are relevant to the 2.5 release of Ansible.

   * ``description``
   * ``lb_method``
   * ``monitor_type``
   * ``monitors``
   * ``name``
   * ``quorum``
   * ``reselect_tries``
   * ``service_down_action``
   * ``slow_ramp_time``

   .. _this link: http://docs.ansible.com/ansible/latest/bigip_pool_module.html
