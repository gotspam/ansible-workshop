Create VS, Pool and Members using App Services (AS3)
====================================================

You will create a playbook to deploy VS, Pools and associated Members using App Services.

**Create service playbook using AS3**

#. Examine playbook ``service_4.yaml``.

#. Run this playbook.

   - Type ``ansible-playbook playbooks/service_4.yaml -e @creds.yaml --ask-vault-pass``

#. Verify results in BIG-IP GUI and browse to ``https://10.1.10.40``.

   .. HINT::

   You must be in the Service4 partition to see the serviceMain virtual server.
   AS3 is designed with multitenancy in mind.  Also, note that this application
   was deployed with an WAF security policy.

#. Run this playbook to teardown.

   - Type ``ansible-playbook playbooks/destroy_all_services.yaml -e @creds.yaml --ask-vault-pass``

#. Verify results in BIG-IP GUI.

.. NOTE::

  A role is a way to encapsulate a body of work. This role could have been zipped up and given to anyone else and they could have extracted it and run it the same way that you did.

  Roles can include their own files, templates, and even custom modules. They should be your go-to solution for all your work that is beyond a single playbook.
