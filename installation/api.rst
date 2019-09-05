Mapping App Setup
*************

Server App
-----------------

The RPM Package has certain requirements in order to run. It requires:

- epel-release
- python-setuptools
- python-pip 
- python-devel 
- geos-devel 
- nginx 
- certbot-nginx 
- gcc

While these are easy to install, we did encounter situations where we had to down and install manually. For those situations, please follow these steps:

- ``sudo yum update``
- ``sudo yum install -y python-devel python-setuptools python-pip geos-devel``
- ``wget https://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/geos-devel-3.5.0-1.rhel6.x86_64.rpm``
- ``wget https://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/geos-3.5.0-1.rhel6.x86_64.rpm``
- ``wget http://mirror.centos.org/centos/7/updates/x86_64/Packages/python-devel-2.7.5-77.el7_6.x86_64.rpm``
- ``sudo rpm -ivh geos-3.5.0-1.rhel6.x86_64.rpm``
- ``sudo rpm -ivh geos-devel-3.5.0-1.rhel6.x86_64.rpm``
- ``sudo rpm -ivh python-devel-2.7.5-77.el7_6.x86_64.rpm --nodeps``

Installing RPM Package
-----------------------

1. Running ``sudo rpm -ivh navagis-1.0.0-1.noarch.rpm`` will look for "requirements" that are either installed or needs to be installed.
2. Once the requirements are installed, re-run ``sudo rpm -ivh navagis-1.0.0-1.noarch.rpm``
3. The RPM package performs several functions:

a. It copies the correct Nginx configuration file to the appropriate location - ``/etc/nginx/nginx.conf``
b. It copies the Navagis app to the default Nginx directory to serve apps - ``/etc/share/nginx/html/``
c. It starts Supervisor, which starts the mapping app.

Important Commands
-------------------

1. To confirm Nginx is working properly, run ``sudo systemctl status nginx``
2. To confirm Supervisor is running, run ``ps aux | grep supervisord``
3. To uninstall the RPM, run ``sudo rpm -e navagis-1.0.0.1``

Create Materialized View
-------------------------

For the mapping app to display the heat map, you must create a materialized view in Hana. Simply open a SQL query window and input the following code:

.. code-block:: javascript

    CREATE VIEW v_account_address_territory AS
    SELECT ac.accountseq,
        CASE WHEN (aa.latitude IS NOT NULL AND aa.longitude IS NOT NULL) THEN NEW ST_POINT(aa.longitude, aa.latitude) ELSE NULL END as geom,
        ac.accounttype,
        ac.revenue,
        ac.sales,
        ac.accountvalue,
        ac.unittypeforrevenue,
        ac.unittypeforsales,
        ac.unittypeforaccountvalue,
        (SELECT STRING_AGG(ta.territoryseq, '|') FROM csq_territoryaccount ta WHERE ta.accountseq = ac.accountseq) as territoryseq
    FROM csq_account ac
    LEFT JOIN csq_accountaddress aa
    ON aa.accountseq = ac.accountseq
    ORDER BY ac.accounttype;

More info on Materialized Views `here <https://www.postgresql.org/docs/10/rules-materializedviews.html>`_

.. note::

    **Resources:**

    - `Deploy flask app with nginx using gunicorn and supervisor (Medium) <https://medium.com/ymedialabs-innovation/deploy-flask-app-with-nginx-using-gunicorn-and-supervisor-d7a93aa07c18>`_
    - `How To Serve Flask Applications with Gunicorn and Nginx on CentOS 7 (Digital Ocean) <https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-centos-7>`_
    - `How To Secure Nginx with Let's Encrypt on CentOS 7 (Digital Ocean) <https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-centos-7>`_
    - If using **CentOS**, set SELinux to "permissive". More info `here <https://stackoverflow.com/questions/22586166/why-does-nginx-return-a-403-even-though-all-permissions-are-set-properly#answer-26228135>`_ and `here <https://stackoverflow.com/questions/6795350/nginx-403-forbidden-for-all-files>`_
    - If Nginx will not start for some reason, check to see what is running on the ports. More info `here <https://stackoverflow.com/questions/42303401/nginx-will-not-start-address-already-in-use/42303738>`_
    - For CentOs, if you need to bind to a certain port (e.g. 8181), refer to `this article <https://www.itadminstrator.com/2017/07/nginx-no-permission-to-bind-port-9080.html>`_ and `this article <https://serverfault.com/questions/790404/selinux-error-valueerror-port-tcp-5000-already-defined>`_