# JSON Web Tokens

### Structure

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
Used to validate the token is trustworthy and has not been tampered with. The signature must be verified before storing and using a JWT. It is made up of a hash of the *header*, *payload*, and *secret*. A signed JWT is known as JWS (JSON Web Signature)

```javascript
// creating the signature
const encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload);

HMACSHA256(encodedString, 'secret');
```

The `'secret'` is a signature held by the server. This is how the server can verify existing tokens and sign new ones

Two Common Methods:
  - **HMAC**
    - Combines the payload with a hashed secret. Produces a signature which can be used to verify the message.
    - Known as shared-secret signing scheme since both the generating and validating party know the secret.
    - Both can generate a new signed message.
  - **RSASSA**
    - The receiving party can *only verify* the authenticity of the message, but cannot generate it.
    - Uses a public/private key scheme.
    - Private key can create and verify the signed message. Public key can only verify.
    - Important to one-to-many signing scenarios such as Single-Sign-On where there's only one producer but many consumers.

### How JSON Web Tokens Work
  - A JWT is returned when a user logs in with credentials.
  - Whenever a user needs to access protected resource/source, the user agent sends a JWT, typically in the **Authorization** header using the **Bearer** schema (as shown below).
  ```js
  Authorization: Bearer <token>
  ```
  - The server's routes will check if a valid JWT exists in the `Authorization` header. If present, the user can access the protected resources.
  - The JWT can sometimes have useful data which avoids having to query databases for more information.
  - This method of sending tokens in the `Authorization` header does not use cookies and is stateless (aka the user state is never stored in server memory). This will avoid [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) issues and using URL parameters which would make client state bookmarkable.

*Security Notes:*
  - Tokens are credentials and should not be stored longer than necessary.
    - Using things like the [local storage](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html#local-storage) of your browser to store sensitive information is generally considered bad practice due to security concerns.
  - Signed tokens expose information to users which they can read but not modify. So, you should not put secret information within the tokens. JWTs can be encrypted to obscure this data and prevent users from reading or modifying session data.

### Why Use JSON Web Tokens?
  - Less verbose than XML and smaller in size. More compact than Security Assertion Markup Language Tokens (SAML).
  - Simple Web Tokens (SWT) need shared keys. JWT can have public/private key pairs using  X.509 certificate for signing.
  - JSON parsers are ubiquitous and it's easier to map JSON to objects than for XML/Document Based formats.
  - JWT is used at an internet scale, meaning client-side processing is much easier.
  - Information is self-contained. No need to constantly refer to database.
  - Allows granular security, or the ability to specify a particular set of permissions in the token, which improves debuggability.

### Our Use Cases for JSON Web Tokens
  - Most likely not for typical login flow
  - Would use instead for single-sign-on functionality
    - e.g. a C4C general sign in that would return a JWT to the requester
  - Can also use for transferring data between different APIs, whether our own (e.g. single-sign-on or maybe in doing something where we would transfer data between Speak for the Trees and Lucy's Love Bus) or when communicating with external APIs.

### Libraries
  - [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) library can be used to encode/decode JWT.
  ```js
  const jwt = require('jsonwebtoken');
  const secret = 'shhhhh';
  // encode
  const token = jwt.sign({ foo: 'bar' }, secret);
  // verify and decode
  const decoded = jwt.verify(token, secret);
  console.log(decoded.foo) // bar
  ```
  - List of Libraries commonly used for JWT [here](https://jwt.io) (in Libraries section)

  - For Java, we will likely want to use one of the libraries provided by [Vert.x](https://github.com/vert-x3/vertx-auth) or [Auth0](https://github.com/auth0/java-jwt)
    - For Vert.x, use `maven: io.vertx / vertx-auth-jwt / 3.5.1`
    - The Maven dependency syntax for Auth0 is included in the link above

### Certificates
  - If X.509, PGP, or SDSI certificates are used, then we will likely want to use the [`Certificate` Java class](https://docs.oracle.com/javase/8/docs/api/java/security/cert/Certificate.html) to generate our public key for the back end
  ```java
    public static void main(String[] args) throws FileNotFoundException, IOException, CertificateException {
    try(java.io.InputStream is = new java.io.FileInputStream(FILENAME)) {
      java.security.cert.CertificateFactory cf = java.security.cert.CertificateFactory.getInstance("X.509");

      java.security.cert.X509Certificate cert = (java.security.cert.X509Certificate) cf.generateCertificate(is);

      System.out.println(Main.bytesToHex(cert.getPublicKey().getEncoded()));

    is.close();
    }
 ```
   But without printing out the public key.
   [An example is shown here](https://repl.it/repls/UntriedFormalCalculator)

  - If X.509 certificates are used, then for the front end we will want to look at something like [PKI.js](http://pkijs.org/) to generate the public key
  ```js
      //require("babel-polyfill"); // Would be required only if you compiled PKI.js for Node <= v4
    const asn1js = require("asn1js");
    const pkijs = require("pkijs");
    const Certificate = pkijs.Certificate;

    const buffer = new Uint8Array([
        // ... cert hex bytes ...
    ]).buffer;

    const asn1 = asn1js.fromBER(buffer);
    const certificate = new Certificate({ schema: asn1.result });
 ```
