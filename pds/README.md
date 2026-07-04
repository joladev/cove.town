# pds.cove.town

Using tranquil PDS.

## Wildcard cert renewal (every ≤90 days)

Cert covers `*.cove.town`, serves all handle subdomains. Expiry check:
`echo | openssl s_client -connect pds.cove.town:443 2>/dev/null | openssl x509 -noout -enddate`

1. `docker run -it --rm -v "$PWD/le:/etc/letsencrypt" certbot/certbot certonly --manual --preferred-challenges dns -d '*.cove.town'`
2. Certbot prints a TXT value. Update `_acme-challenge.cove.town TXT` in the marque console.
   Confirm before continuing: `dig +short TXT _acme-challenge.cove.town @cirrus.ns.marque.network`
3. Press Enter in certbot. New files: `le/live/cove.town/fullchain.pem`, `privkey.pem`.
4. Dokploy → Settings → Certificates → `covetown-wildcard`: paste both over the old contents.
5. Restart Traefik (Dokploy → Web Server). Required — Traefik only loads the updated cert files on boot.
6. Verify: `curl -s https://johanna.cove.town/.well-known/atproto-did` → `did:plc:3pnunm7komgxzfwrrxdtvsvl`
