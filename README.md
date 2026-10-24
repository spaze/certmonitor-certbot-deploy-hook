# certmonitor-certbot-deploy-hook
Certbot deploy hook to report my newly issued certificates back to my app

1. Copy `certbot_deploy_hook.template.conf` to `certbot_deploy_hook.conf`
2. Edit the details in `certbot_deploy_hook.conf`
3. Symlink `certbot_deploy_hook` to `/etc/letsencrypt/renewal-hooks/deploy`
4. ...
5. Profit!

## Exit status

The hook exits with 0 when the certificate was reported and nginx reloaded, and with 1 to 3 when it could not even try (config file missing, a value in it empty, `cert.pem` unreadable). It exits with 4 when the certificate was not reported, either because the API refused it, then the HTTP status and the JSON answer above the exit say why, or because the request itself failed, then curl's error message says why. nginx is reloaded in that case too, so a reporting problem never leaves the old certificate in service. It exits with 5 when the nginx reload failed, which outranks a failed report because the old certificate stays in service.

certbot logs a non-zero exit status to `/var/log/letsencrypt/letsencrypt.log` as `Hook 'deploy-hook' reported error code N` together with the hook's output, and the renewal itself is not affected. To report the certificate again once the cause is fixed, run the hook manually:

```
RENEWED_LINEAGE=/etc/letsencrypt/live/example.com ./certbot_deploy_hook
```

Needs curl 7.76 or newer for `--fail-with-body`.
