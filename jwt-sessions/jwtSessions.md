# JSON Web Tokens

## Structure

JSON Web Token (JWT) is a self-contained way to securely transmit information as a JSON object.

A JSON Web Token is a string consisting of three parts, each separated by a dot (.):

  - Header
  - Payload
  - Signature

Following this pattern, a typical JWT looks like:
```
aaaaa.bbbbb.ccccc
```
a - header
b - payload
c - signature

**Header**  
Usually two parts, includes the type of token (in this case JWT) and the [signing algorithm](https://auth0.com/docs/tokens/concepts/signing-algorithms) being used.

```javascript
// example header
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload**
Contains claims. Claims are statements about an entity (e.g., the user) and additional data. There are three types of claims:

  - **Registered Claims**: Predefined. Not mandatory, but recommended. Examples include **iss** (issuer), **exp** (expiration time), and **sub** (subject). A more complete list of examples can be found [here](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html#RegisteredClaimName)

  - **Public Claims**: Claims created by us. For example: `user_name`

  - **Private Claims**: Custom claims created to share information between parties that have agreed to use them

```javascript
// example payload
{
  "iss": "c4cneu.com", // issuer
  "exp": "1570934917" // expiration time
  "user_name": "Joseph Aoun",
  "admin": true
  "spiritual leader": true
}
```

**Signature**
Used to validate the token is trustworthy and has not been tampered with. The signature must be verified before storing and using a JWT. It is made up of a hash of the *header*, *payload*, and *secret*  

```javascript
// creating the signature
const encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload);

HMACSHA256(encodedString, 'secret');
```

The `'secret'` is a signature held by the server. This is how the server can verify existing tokens and sign new ones
