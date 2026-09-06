# pds.cove.town

Using tranquil PDS.

## Wildcard cert renewal (every ≤90 days)

For now, the wildcard cert has to be regenerated every 90 days.

Check expiry with: `echo | openssl s_client -connect pds.cove.town:443 2>/dev/null | openssl x509 -noout -enddate`

### Steps to renew

1. `docker run -it --rm -v "$PWD/le:/etc/letsencrypt" certbot/certbot certonly --manual --preferred-challenges dns -d '*.cove.town'`
2. Grab the printed value and update the DNS record for `_acme-challenge`. Wait a tick, then hit enter to generate the cert. If you were too impatient you have to do it again and wait longer.
3. Do a little `openssl x509 -in le/live/cove.town/fullchain.pem -noout -subject -issuer -startdate -enddate -ext subjectAltName` if you want, it's probably fine though.
4. Edit the certificate in Dokploy, replacing Certificate data with `fullchain.pem` and Private key with `privkey.pem`. Make sure to update them both at the same time. Or not, I'm not the boss of you.
5. Restart Traefik through the Web server page.
6. Run expiry check from above again.
