API Setup
*********

1. ``cd /var/www``
2. ``sudo mkdir callidus``
3. ``sudo chown user:user callidus``
4. ``git clone https://github.com/Navagis-LLC/Callidus.git``
5. ``cd callidus``
6. ``bin/setup.sh``
7. ``bin/createdb.sh``
8. ``bin/setup-nginx.sh``
9. ``python manage.py db upgrade``
10. ``bin/run-app.sh``