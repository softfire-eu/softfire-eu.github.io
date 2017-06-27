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

From the code is easy to understand the this is a POST method with body parameters:

* username: the username of the openvpn user
* password: in case you want to encrypt the private key with passphrase
* days: number of validity days (starting from today)

It is also clear that your user need to belong to the _portal_ role or higher.

a list of curl commands are listed as example

```sh
curl -v -X POST --cookie $COOKIE_FILE --form 'username=foobar23' --form 'password=123456' --form 'days=1' http://experiment.manager.ip:5080/certificates
```

The ```$COOKIE_FILE``` variable contains the authentication info that should be used as cookie and that are retrieved after authentication
