# docker-wordpress

For setting this up I have used Docker on top of Ubuntu os as host.

Things I have used :
 1. MariaDB
 2. Wordpress container.
 3. MySQL on my host
 4. Nginx as reverse proxy for the WordPress container.
 
 Setup the wordpress container :
  docker pull wordpress:latest
  docker run -e WORDPRESS_DB_USER=wpuser -e WORDPRESS_DB_PASSWORD=wpuser@ -e WORDPRESS_DB_NAME=wordpress_db -p 8081:80 -v /root/wordpress    /html:/var/www/html --link wordpressdb:mysql --name wpcontainer -d wordpress
  To test it use: curl -I localhost:8081
  
  Now installing nginx:
   apt-get install nginx
   cd /etc/nginx/sites-available/
vim wordpress

server {
  listen 80;
  server_name wordpress-docker.co www.wordpress-docker.co;
 
  location / {
    proxy_pass http://localhost:8081;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}

To activate new Wordpress host:
ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
rm -f /etc/nginx/sites-available/default
rm -f /etc/nginx/sites-enabled/default
systemctl restart nginx

Finally setting up Wordpress, goto www.wordpress-docker.co and manage the Dashboard.
