Getting Started
---------------

The purpose of this guide is to provide a sampling of hands-on exercises to better understand deployment of BIG-IP using Ansible.

Lab Topology
~~~~~~~~~~~~

Each student will have a BIG-IP VE environment with IP addressing as below:

.. image:: /_static/image000.png

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **Component**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Jump Host
      - - **Management:** 10.1.1.51
        - **External:** 10.1.10.51
      - - ``f5student``/``f5DEMOs4u``
    * - Ansible Host
      - - **Management:** 10.1.1.150
        - **External:** 10.1.10.150
        - **Internal:** 10.1.20.150
      - - ``root``/``password``
        - ``admin``/``password``
    * - BIG-IP01
      - - **Management:** 10.1.1.245
        - **External:** 10.1.10.245
        - **Internal:** 10.1.20.245
      - - ``admin``/``admin``
    * - Lamp Host
      - - **Management:** 10.1.1.252
        - **External:** 10.1.10.252
        - **Internal:** 10.1.20.252
      - - ``root``/``default``

Connect to lab
~~~~~~~~~~~~~~

**Access f5 training portal**

#. Open a browser and go to student portal site as provided by instructor.

**Connect to Jump server**

#. Look for the xubuntu-jumpbox-vxx.  You will use the xubuntu jumpbox for all the labs. (see below)

   .. image:: /_static/image001.png
      :height: 500px

#. You can click on **RDP** to RDP to the Xubuntu jumpbox or you can select the **CONSOLE** link and access the jumpbox via your browser.  **The CONSOLE link requires you turn off pop-up blockers.**

   .. image:: /_static/image002.png
      :height: 300px

**Connect to BIG-IP admin gui**

#. From Jumpbox, open BIG-IP Admin GUI

   - Open Chrome browser found on launchpad at bottom of screen
   - Click on ``bigip01`` on favorites bar
   - Login with username: ``admin`` and password: ``admin``

**Connecting to ansible host.**

#. From Jumpbox, SSH to Ansible host

   - Open Terminal Emulator Window found on launchpad at bottom of screen
   - Type ``ssh root@10.1.1.150``
   - Type ``yes`` when asked "Are you sure..."
   - If prompted, password is **password**

#. Change directory to access lab directory folder

   - type ``cd f5-ansible-workshop`` to change directory to location of the ansible lab.
   - type ``pwd`` to confirm path is **/root/f5-ansible-workshop**.
