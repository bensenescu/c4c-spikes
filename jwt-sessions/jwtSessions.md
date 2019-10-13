## JSON Web Tokens

JSON Web Token (JWT) is a self-contained way to securely transmit information as a JSON object.

They consist of three parts, each separated by a dot (.):

  - Header
  - Payload
  - Signature

Following this pattern, a typical JWT looks like:
```
[header].[payload].[signature]
```

**Header**  
Usually two parts, includes the type of token (in this case JWT) and the [signing algorithm](https://auth0.com/docs/tokens/concepts/signing-algorithms) being used.

```javascript
{
  "alg": "HS256",
  "typ": "JWT"
}
```
