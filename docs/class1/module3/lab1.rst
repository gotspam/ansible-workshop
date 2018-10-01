Create VS, Pool and Members using seed file
===========================================

You will create a consolidated playbook to deploy VS, Pools and associated Members.

**Create consolidated playbook**

#. Create a playbook ``seedapp.yaml``.

   - Type ``nano playbooks/seedapp.yaml``
   - Type the following into the ``playbooks/seedapp.yaml`` file.

   .. code::

    ---

    - name: "Create new web app"
      hosts: bigips
      gather_facts: False
      connection: local
      vars_files:
        - ../vars/{{ seed }}

      vars:
        state: "present"
        seed: "seed1.yaml"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Adjust virtual server
          bigip_virtual_server:
            name: "{{ vs_name }}"
            destination: "{{ vs_ip }}"
            port: "{{ vs_port }}"
            description: "Web App"
            snat: "Automap"
            all_profiles:
              - "tcp-lan-optimized"
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

   - Type ``ansible-playbook -e @creds.yaml --ask-vault-pass playbooks/seedapp.yaml``

#. Verify results in BIG-IP GUI and browse to app.

   .. hint::

     You should see www12 deployed with 2 pool members.  App should be accessible on http://10.1.10.12.

#. Run this playbook to teardown app.

   - Type ``ansible-playbook -e @creds.yaml --ask-vault-pass playbooks/seedapp.yaml -e state="absent"``

#. Verify that www12, pool and nodes should be deleted in BIG-IP GUI.

.. NOTE::

  This playbook leverages a config seed file in vars/seed1.yaml.  Create a new file vars/seed2.yaml to deploy a new service.
