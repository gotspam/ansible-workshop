Deploy BIG-IP App Services
==========================

You will create a consolidated playbook to deploy VS, Pools and associated Members.


#. Create a playbook ``hack11.yaml``.

   - Type ``nano playbooks/hack11.yaml``
   - Type the following into the ``playbooks/hack11.yaml`` file.

   .. code::

    ---

    - name: "Deploy / teardown a web app (VS, pool, nodes)"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        vs_name: "hack11"
        vs_ip: "10.1.10.11"
        vs_port: "443"
        vs_snat: "automap"
        pl_name: "hack11_pl"
        pl_monitor: "/Common/http"
        pl_lb: "round-robin"
        nd_port: "80"
        nd_ip1: "10.1.20.17"
        nd_ip2: "10.1.20.20"
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Adjust a VS
          bigip_virtual_server:
            name: "{{ vs_name }}"
            destination: "{{ vs_ip }}"
            port: "{{ vs_port }}"
            snat: "{{ vs_snat }}"
            all_profiles:
              - "tcp-lan-optimized"
              - "clientssl"
              - "http"
              - "analytics"
            state: "{{ state }}"

        - name: Adjust a pool
          bigip_pool:
            name: "{{ pl_name }}"
            monitors: "{{ pl_monitor }}"
            lb_method: "{{ pl_lb }}"
            state: "{{ state }}"

        - name: Add nodes
          bigip_node:
            name: "{{ item.name }}"
            host: "{{ item.host }}"
            state: "{{ state }}"
          loop:
            - { name: "{{ nd_ip1 }}", host: "{{ nd_ip1 }}" }
            - { name: "{{ nd_ip2 }}", host: "{{ nd_ip2 }}" }

        - name: Add nodes to pool
          bigip_pool_member:
            host: "{{ item.host }}"
            port: "{{ nd_port }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
          loop:
            - { host: "{{ nd_ip1 }}" }
            - { host: "{{ nd_ip2 }}" }
          when: state == "present"

        - name: Update a VS
          bigip_virtual_server:
            name: "{{ vs_name }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
          when: state == "present"

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook -e @creds.yaml --ask-vault-pass playbooks/hack11.yaml``

   You will be prompted for vault ``password`` before executing the playbook.
   If successful, you should see config for virtual servers, pools and nodes.

   .. NOTE::

    Type ``ansible-vault view creds.yaml`` to modify the vault file.
    Type ``ansible-vault edit creds.yaml`` to modify the vault file.


#. Verify results in BIG-IP GUI.

   - Select **Local Traffic -> Virtual Servers -> Virtual Server List**

   .. image:: /_static/image032.png
         :height: 100px

   - Select **Local Traffic -> Pools -> Pool Members -> hack11_pl**

   .. image:: /_static/image033.png
         :height: 140px

#. Browse to ``https://10.1.10.11`` to test Application

   .. image:: /_static/image034.png
          :height: 300px
