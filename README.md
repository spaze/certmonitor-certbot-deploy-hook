# certmonitor-certbot-deploy-hook
Certbot deploy hook to report my newly issued certificates back to my app

1. Copy `certbot_deploy_hook.template.conf` to `certbot_deploy_hook.conf`
2. Edit the details in `certbot_deploy_hook.conf`
3. Symlink `certbot_deploy_hook` to `/etc/letsencrypt/renewal-hooks/deploy`
4. ...
5. Profit!
