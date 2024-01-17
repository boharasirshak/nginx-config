# Preconfigured nginx config file.

## Purpose
This pre-configured `nginx.conf` file for multiple deployment purposes.
- To serve the frontend and backend on the same server, IP, and base domain.
- To proxy the request coming from the domain to frontend app.
- To proxy the request coming from subdomain to backend app (API).
- To upgrade all the incoming HTTP traffic on 80 port to 443 HTTPS.

## Guide
I am assuming that you use ubuntu. If not, please download nginx depending on your OS & distro.


### Installation

#### Instal Nginx
```bash
sudo apt update
sudo apt install nginx
```
#### Allow nginx through firewall
```bash
sudo ufw allow 'Nginx HTTP'
```

#### Enable nginx
```bash
sudo systemctl enable nginx
```

#### Start nginx
```bash
sudo systemctl start nginx
```

#### Check the status of nginx
```bash
sudo systemctl status nginx
```

### Usage

### Clone this repo
Go to `etc/nginx` directory and clone this repo.

```bash
cd /etc/nginx/

git clone https://github.com/boharasirshak/nginx-config.git
```

#### Delete the default nginx.conf file
```bash
sudo rm /etc/nginx/nginx.conf
```

#### Move the entire files and directories to the nginx directory
```bash
sudo mv nginx-config/* /etc/nginx/
```

#### Remove the cloned repo
```bash
sudo rm -rf nginx-config
```

#### Edit the nginx.conf file
```bash
sudo nano /etc/nginx/nginx.conf
```

#### Replace the following variables with your own values

- $frontend_port -> The port on which your frontend app is running. (Ex: NextJS listening on PORT 3000)

- $backend_port -> The port on which your backend app is running. (Ex: Strapi running on PORT 1337)

- example.com & www.example.com -> Your base domain name for the frontend app.

- api.example.com -> The subdomain name you reserved for the backend API. 


#### Save the file and exit

### SSL
If you do not own a SSL certificate, you can get one for free from [Let's Encrypt](https://letsencrypt.org/), and use [Certbot](https://certbot.eff.org/) to install it.
However, if you already have a SSL certificate, you can skip these steps, and move your certificate to `/etc/nginx/ssl/` directory, and name them `fullchain.pem` and `privkey.pem` respectively.

#### Certbot

Install certbot
```bash
sudo apt install certbot python3-certbot-nginx
``` 

Get the SSL certificate
```bash
sudo certbot --nginx -d example.com -d www.example.com
```

Verifying Certbot Auto-Renewal
```bash
sudo systemctl status certbot.timer
```

To test the renewal process
```bash
sudo certbot renew --dry-run
```

This should save the certificate in `/etc/letsencrypt/live/example.com/` directory.

Move the certificate to `/etc/nginx/ssl/` directory
```bash
sudo mv /etc/letsencrypt/live/example.com/fullchain.pem /etc/nginx/ssl/
sudo mv /etc/letsencrypt/live/example.com/privkey.pem /etc/nginx/ssl/
```


### Test the nginx config file
```bash
sudo nginx -t
```

### Restart nginx
```bash
sudo systemctl restart nginx
```

### Check the status of nginx
```bash
sudo systemctl status nginx
```

### Done
You should be able to access your frontend app on `https://example.com` and backend API on `https://api.example.com`.