Configure BIG-IP App Services (iApp)
====================================

F5 BIG-IP has created iApp templates to ease deployment process.

#. Login to BIG-IP gui and configure services.

   - Select **iApps -> Application Services -> Applications** then click **create**
   - Type ``hello15`` for **Name**
   - Select ``f5.http`` **Template**
   - Select ``Advanced - Configure advanced options``

     .. image:: /_static/image012.png
         :height: 300px

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

   - Open browser to ``http://10.1.10.15``.

   .. NOTE::

     The application is now published and should be available from external network.
     The F5 BIG-IP is terminating secure connections and distributing traffic to
     backend webservers.
