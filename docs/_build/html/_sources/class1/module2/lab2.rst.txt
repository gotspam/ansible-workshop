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
        username: admin
        password: admin
        server: 10.1.1.245
        valid_certs: no

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
            password: "{{ password }}"
            server: "{{ server }}"
            user: "{{ username }}"
            validate_certs: "{{ valid_certs }}"

        - name: Adjust a pool
          bigip_pool:
            name: "{{ pl_name }}"
            monitors: "{{ pl_monitor }}"
            lb_method: "{{ pl_lb }}"
            state: "{{ state }}"
            password: "{{ password }}"
            server: "{{ server }}"
            user: "{{ username }}"
            validate_certs: "{{ valid_certs }}"

        - name: Add nodes
          bigip_node:
            name: "{{ item.name }}"
            host: "{{ item.host }}"
            state: "{{ state }}"
            password: "{{ password }}"
            server: "{{ server }}"
            user: "{{ username }}"
            validate_certs: "{{ valid_certs }}"
          loop:
            - { name: "{{ nd_ip1 }}", host: "{{ nd_ip1 }}" }
            - { name: "{{ nd_ip2 }}", host: "{{ nd_ip2 }}" }

        - name: Add nodes to pool
          bigip_pool_member:
            host: "{{ item.host }}"
            port: "{{ nd_port }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
            password: "{{ password }}"
            server: "{{ server }}"
            user: "{{ username }}"
            validate_certs: "{{ valid_certs }}"
          loop:
            - { host: "{{ nd_ip1 }}" }
            - { host: "{{ nd_ip2 }}" }
          when: state == "present"

        - name: Update a VS
          bigip_virtual_server:
            name: "{{ vs_name }}"
            pool: "{{ pl_name }}"
            state: "{{ state }}"
            password: "{{ password }}"
            server: "{{ server }}"
            user: "{{ username }}"
            validate_certs: "{{ valid_certs }}"
          when: state == "present"

   - Ctrl x to save file.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/hack11.yaml``

#. Verify results in BIG-IP GUI.

   - Select **Local Traffic -> Virtual Servers -> Virtual Server List**

   .. image:: /_static/image032.png
         :height: 100px

   - Select **Local Traffic -> Pools -> Pool Members -> hack11_pl**

   .. image:: /_static/image033.png
         :height: 140px

#. Browse to ``https://10.1.10.11`` to test Application.  Hackazon image should be **green**.

   .. image:: /_static/image034.png
          :height: 300px
