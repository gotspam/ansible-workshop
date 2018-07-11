Configure BIG-IP App Services
=============================

The application has been deployed and DevOps team has entered a ticket for NetOps
to configure App Services so applications can be accessible from external users.
This often dependent of available change windows and can create huge delays.

#. Login to BIG-IP gui and configure services.

   - Select **Local Traffic-> Virtual Servers -> Virtual Server List** then click **create**
   - Type ``hello10`` for **Name**
   - Select ``10.1.10.10`` **Destination Address**
   - Select ``HTTP`` **Service Port**

     .. image:: /_static/image020.png
         :height: 240px

   - In the **Configuration** section select ``Automap`` for **Source Address Translation**.

     .. image:: /_static/image021.png
         :height: 80px

   - In the **Resources** section click ``+`` next to **Default Pool**

     .. image:: /_static/image022.png
         :height: 100px

     This will open the **New Pool Configuration**

   - In the **Configuration** section type ``hello10pool`` for **Name** and select ``http`` for **Health Monitors**

     .. image:: /_static/image023.png
         :height: 240px

   - In the **Resource** section add members ``10.1.20.15`` and ``10.1.20.16`` with **Service Port** ``80``

     .. image:: /_static/image024.png
         :height: 240px

   - Click **Finished** then click **Finished** again.

     If successful, you should see similar results

     .. image:: /_static/image025.png
         :height: 120px

#. Verify results by browsing to websites.

   - Open browser to ``http://10.1.10.10``.

     .. image:: /_static/image026.png
         :height: 300px


   .. NOTE::

     The application is now published and should be available from external network.
     The F5 BIG-IP is terminating connections and distributing traffic to
     backend webservers.
