Configure BIG-IP App Services
=============================

The application has been deployed and DevOps team has entered a ticket for NetOps
to configure App Services so applications can be accessible from external users.
This often dependent of available change windows and can create huge delays.

F5 BIG-IP has create iApp templates to ease deployment process.

#. Login to BIG-IP gui and configure services.

   - Select **iApps -> Application Services -> Applications** then click **create**
   - Type ``hello10`` for **Name**
   - Select ``f5.http`` **Template**
   - Select ``Advanced - Configure advanced options``

     .. image:: /_static/image012.png
         :height: 140px

   - In the **Network Section** select ``Yes, use new profiles``.

     .. image:: /_static/image013.png
         :height: 140px

   - In the **Virtual Server and Pools** section enter info as shown on image below

     .. image:: /_static/image015.png
         :height: 350px

   - In the **App Health** section type ``hello, world`` for expected response

     .. image:: /_static/image014.png
         :height: 300px

   - Click **Finished**

     If successful, you should see similar results

     .. image:: /_static/image016.png
         :height: 230px

#. Verify results by browsing to websites.

   - Open browser to ``http://10.1.10.10``.

   .. NOTE::

     The application is now published and should be available from external network.
     The F5 BIG-IP is terminating secure connections and distributing traffic to
     backend webservers.
