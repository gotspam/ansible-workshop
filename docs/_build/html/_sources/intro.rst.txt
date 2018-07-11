Getting Started
---------------

The purpose of this guide is to provide a sampling of hands-on exercises to better understand deployment of BIG-IP using Ansible.

Lab Topology
~~~~~~~~~~~~

Each student will have a BIG-IP VE environment with IP addressing as below:

.. image:: /_static/image000.png

Connect to lab
~~~~~~~~~~~~~~

**Accessing the lab environment.**

#. Open a browser and go to student portal site as provided by instructor.

#. Look for the xubuntu-jumpbox-vxx.  You will use the xubuntu jumpbox for all the labs. (see below)

.. image:: /_static/image001.png
   :height: 700px

#. You can click on **RDP** to RDP to the Xubuntu jumpbox or you can select the **CONSOLE** link and access the jumpbox via your browser.  **The CONSOLE link requires you turn off pop-up blockers.**

.. image:: /_static/image002.png
   :height: 300px


**Connecting to ansible host.**

#. Launch putty and connect to ansible.

#. Change directory to ``f5-ansible-workshop``.

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
