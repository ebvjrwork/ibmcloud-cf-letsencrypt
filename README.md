# LetsEncrypt on IBM Cloud Foundry
ebvjr

## Prerequisite
- ibmcloud (https://cloud.ibm.com/docs/cli?topic=cli-getting-started)
- certbot (https://certbot.eff.org/)
- domain added on cloudfoundry (`cf create-domain ORG DOMAIN`)

## Instruction
- Clone this repository
- Run the command `certbot certonly --manual -d YOUR_DOMAIN_HERE` and accept all of terms.
- On the instruction of certbot, you will be given like this:
```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Create a file containing just this data:

XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

And make it available on your web server at this URL:

http://YOUR_DOMAIN_HERE/.well-known/acme-challenge/YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
- Open a new terminal and cd to this repository, create `public/.well-known/acme-challenge` folder by running `mkdir -p public/.well-known/acme-challenge` and run `echo 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' > 'public/.well-known/acme-challenge/YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY'`
- Run the command `ibmcloud cf push letsencrypt-static -d YOUR_DOMAIN_HERE --no-hostname --route-path .well-known/acme-challenge/ -m 64M -b nginx_buildpack`
- After running the last command, press enter on letsencypt.
- If succeed, it should display this message:
```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/YOUR_DOMAIN_HERE/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/YOUR_DOMAIN_HERE/privkey.pem
   Your cert will expire on XXXX-XX-XX. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

- Delete the temporary app by running `ibmcloud cf delete letsencrypt-static`
- To upload your certificate run the command `ibmcloud app domain-cert-add YOUR_DOMAIN_HERE -
i /etc/letsencrypt/live/YOUR_DOMAIN_HERE/fullchain.pem -c /etc/letsencrypt/live/YOUR_DOMAIN_HERE/fullchain.pem -k /etc/letsencrypt/live/YOUR_DOMAIN_HERE/privkey.pem`
