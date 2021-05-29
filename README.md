# HTTPS with automatic certificate renewal

## Initial Setup

1. Map the domain name to the IP address of website host (e.g. using an A record in Google Domains or Freenom).
2. After cloning this repository, launch an NGINX instance to handle the initial certbot challenge process using the initial-setup.conf configuration file.

```
$ sudo docker run -it --rm --name initial_setup_nginx -d \
    -v $(pwd)/initial-setup.conf:/etc/nginx/conf.d/app.conf \
    -v $(pwd)/etc/letsencrypt:/etc/letsencrypt \
    -v $(pwd)/certbot/www:/var/www/certbot \
    -p 80:80 nginx:latest
```

3. Obtain a new certificate by using the `certonly --webroot` command. When prompted for the webroot, input: `/var/www/certbot`

```
$ sudo docker run -it --rm --name initial_setup_certbot \
    -v $(pwd)/etc/letsencrypt:/etc/letsencrypt \
    -v $(pwd)/certbot/www:/var/www/certbot \
    certbot/certbot:latest certonly --webroot
```

4. Kill the NGINX instance used for initial configuration.

```
$ sudo docker kill initial_setup_nginx
```

## Start HTTPS web server

Once the key and certificate is obtained, launch the HTTPS web server using the following command:

```
$ sudo docker-compose up -d
```

## Stop HTTPS web server

The following command can be used to stop the web server.

```
$ sudo docker-compose down
```