# How to genereate OpenVPN configuration with certificate

The Experiment Manager provides a ReST API for generating the OpenVPN configuration file automatically.

```python
@bottle.post('/certificates')
@authorize(role='portal')
def get_certificate():
    username = post_get('username')
    if not username:
        raise bottle.HTTPError(500, "Username missing")
    password = post_get('password', default=None)
    days = int(post_get('days', default=None))
    cert_gen = CertificateGenerator()
    cert_gen.generate(password, username, days)
    openvpn_config = cert_gen.get_openvpn_config()
    headers = {
        'Content-Type': 'text/plain;charset=UTF-8',
        'Content-Disposition': 'attachment; filename="softfire-vpn_%s.ovpn"' % username,
        "Content-Length": len(openvpn_config)
    }
    return bottle.HTTPResponse(openvpn_config, 200, **headers)
```

From the code we see that this is a POST method with the following body parameters:

* username: the username of the openvpn user
* password: in case you want to encrypt the private key with passphrase
* days: number of validity days (starting from today)

The requesting user need to belong to the _portal_ role or higher.

The following shell commands can be used to retrieve the certificate file, a complete example can be found [here](https://github.com/softfire-eu/bootstrap/blob/master/gen_ovpn.sh)

```sh
COOKIE_FILE=$(tempfile)
curl -X POST --cookie-jar $COOKIE_FILE --form "username=$API_USER" --form "password=$API_PW" ${API_URL}/login
curl -s -X POST --cookie $COOKIE_FILE --form "username=${USERNAME}" --form "password=${PASSWORD}" --form "days=$VALID" ${API_URL}/certificates
curl -X GET --cookie-jar $COOKIE_FILE --cookie $COOKIE_FILE ${API_URL}/logout
```
