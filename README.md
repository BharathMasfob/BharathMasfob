# 1 nvm installation
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
`````
```
source ~/.bashrc
```
```
nvm ls-remote
```
```
nvm install 18.4.0
```
```
node -v
```
## 2 GIT installation
```
sudo apt install git
```
```
git config --global user.name "venkat"
```
```
git config --global user.email "venkat.r@mindvisiontechnologies.com"
```
## 3 PM2 installation
```
npm install pm2 -g
```
```
pm2 -v
```
## 4 NGINX installation
```
sudo apt install nginx
```
```
sudo ufw enable
```
```
sudo ufw status
```
```
sudo ufw allow ssh
```
```
sudo ufw allow http
```
```
sudo ufw allow https
```
```
systemctl status nginx
```
## 5 SSH connection - github
```
ssh-keygen -t rsa
```
enter passpharse and note that carefully
```
cat ~/.ssh/id_rsa.pub
```
copy the output string start as `ssh-rsa` - `root@[name]` to github.com -> settings -> ACCESS -> ssh and cpg keys -> new ssh key -> give title and paste the string and add
```
ssh -T git@github.com
```
type `yes` and enter passpharse
```
eval "$(ssh-agent -s)"
```
```
ssh-add /root/.ssh/id_rsa
```
enter the passpharse
## 6 clone repo from github
```
git clone git@github.com:MindVisionTechnologies/webhook.git
```
!!! use `ssh` method not `https` and
```
cd webhook
```
```
rm -r node_modules
```
```
npm i
```
```
node .
```
check if the server is running properly and if ok press `CTRL+C` to exit
## 7 add servers to PM2
```
cd webhook
```
```
pm2 start index.js --name webhook
```
!! check the index file may be `index.js` or `server.js`
```
pm2 ls
```
to list all pm2 processes
## 8 link PM2 process to NGINX
```
sudo nano /etc/nginx/sites-available/webhook.mindvisiontechnologies.com
```
```
  server{
    index index.js;
    server_name webhook.mindvisiontechnologies.com;
    location / {
        proxy_pass http://localhost:9999;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    }
```
replace `server_name` with your `servername`as domain name and `proxy_pass` as localhost that rubs with pm2
```
nginx -t
```
it check the file configuration and return success message
```
sudo systemctl reload nginx
```
reload  nginx and check the domain
## 9 install certbot
```
sudo add-apt-repository ppa:certbot/certbot
```
```
sudo apt-get update
```
```
sudo apt-get install python3-certbot-nginx
```
```
sudo certbot --nginx
```
it will show the domains with and enter the number of domain to generate ssl certificate
