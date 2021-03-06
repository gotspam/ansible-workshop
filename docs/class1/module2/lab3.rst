Modify Pool Member State
========================

It is a common task to modify pool member states to make application updates.
You will pass variables using ``extra-vars`` to override playbook variables.
Use the ``-e``, or ``--extra-vars`` argument of ``ansible-playbook``

**Create pool member forced offline playbook**

#. Create a playbook ``offline.yaml``.

   - Type ``nano playbooks/offline.yaml``
   - Type the following into the ``playbooks/offline.yaml`` file.

   .. code::

    ---

    - name: "Pool member offline"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        pool: ""
        pmhost: ""
        pmport: ""
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Modify pool member state
          bigip_pool_member:
            state: "{{ state }}"
            session_state: "disabled"
            monitor_state: "disabled"
            host: "{{ pmhost }}"
            port: "{{ pmport }}"
            pool: "{{ pool }}"

#. Run this playbook enable pool member.

   - Type ``ansible-playbook playbooks/offline.yaml -e @creds.yaml --ask-vault-pass -e pool="hack11_pl" -e pmhost="10.1.20.17" -e pmport="80"``

   You will be prompted for a password before executing the playbook.  Password is **password**.
   If successful, you should see config for virtual servers, pools and nodes.

   .. NOTE::

    ``ansible-vault`` is used to store passwords and other sensitive info for use by ansible playbooks.

    Type ``cat creds.yaml`` to confirm vault file is encrypted.
    Type ``ansible-vault view creds.yaml`` to view the vault file.
    Type ``ansible-vault edit creds.yaml`` to modify the vault file.
    
#. Verify if pool member 10.1.20.17 is ``forced offline``

   - Select **Local Traffic -> Pools -> Pool List -> hack11_pl -> Members**

   .. image:: /_static/image035.png
         :height: 200px

**Create pool member enable playbook**

#. Create a playbook ``enable.yaml``.

   - Type ``nano playbooks/enable.yaml``
   - Type the following into the ``playbooks/enable.yaml`` file.


   .. code::

    ---

    - name: "Pool member enable"
      hosts: bigips
      gather_facts: False
      connection: local

      vars:
        pool: ""
        pmhost: ""
        pmport: ""
        state: "present"

      environment: "{{ bigip_env }}"

      tasks:
        - name: Modify pool member state
          bigip_pool_member:
            state: "{{ state }}"
            session_state: "enabled"
            monitor_state: "enabled"
            host: "{{ pmhost }}"
            port: "{{ pmport }}"
            pool: "{{ pool }}"

#. Run this playbook enable pool member.

   - Type ``ansible-playbook playbooks/enable.yaml -e @creds.yaml --ask-vault-pass -e pool="hack11_pl" -e pmhost="10.1.20.17" -e pmport="80"``

#. Verify if pool member 10.1.20.17 is ``enabled``

   - Select **Local Traffic -> Pools -> Pool List -> hack11_pl -> Members**
