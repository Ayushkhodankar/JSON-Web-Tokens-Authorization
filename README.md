# JSON-Web-Tokens-Authorization
# What is a JSON Web Token?
JSON Web Token (JWT) is a general-purpose text-based messaging format for transmitting information in a compact and secure way. Contrary to popular belief, JWT is not just useful for sending and receiving identity tokens on the web - even if that is the most common use case. JWTs can be used as messages for any type of data.

A JWT in its simplest form contains two parts:

The primary data within the JWT, called the payload, and

A JSON Object with name/value pairs that represent metadata about the payload and the message itself, called the header.

A JWT payload can be absolutely anything at all - anything that can be represented as a byte array, such as Strings, images, documents, etc.

But because a JWT header is a JSON Object, it would make sense that a JWT payload could also be a JSON Object as well. In many cases, developers like the payload to be JSON that represents data about a user or computer or similar identity concept. When used this way, the payload is called a JSON Claims object, and each name/value pair within that object is called a claim - each piece of information within 'claims' something about an identity.

And while it is useful to 'claim' something about an identity, really anyone can do that. What’s important is that you trust the claims by verifying they come from a person or computer you trust.

A nice feature of JWTs is that they can be secured in various ways. A JWT can be cryptographically signed (making it what we call a JWS) or encrypted (making it a JWE). This adds a powerful layer of verifiability to the JWT - a JWS or JWE recipient can have a high degree of confidence it comes from someone they trust by verifying a signature or decrypting it. It is this feature of verifiability that makes JWT a good choice for sending and receiving secure information, like identity claims.

Finally, JSON with whitespace for human readability is nice, but it doesn’t make for a very efficient message format. Therefore, JWTs can be compacted (and even compressed) to a minimal representation - basically Base64URL-encoded strings - so they can be transmitted around the web more efficiently, such as in HTTP headers or URLs.


JWT Example
Once you have a payload and header, how are they compacted for web transmission, and what does the final JWT actually look like? Let’s walk through a simplified version of the process with some pseudocode:

Assume we have a JWT with a JSON header and a simple text message payload:

header

{
  "alg": "none"
}
payload

The true sign of intelligence is not knowledge but imagination.
Remove all unnecessary whitespace in the JSON:

String header = '{"alg":"none"}'
String payload = 'The true sign of intelligence is not knowledge but imagination.'
Get the UTF-8 bytes and Base64URL-encode each:

String encodedHeader = base64URLEncode( header.getBytes("UTF-8") )
String encodedPayload = base64URLEncode( payload.getBytes("UTF-8") )
Join the encoded header and claims with period ('.') characters:

String compact = encodedHeader + '.' + encodedPayload + '.'
The final concatenated compact JWT String looks like this:

eyJhbGciOiJub25lIn0.VGhlIHRydWUgc2lnbiBvZiBpbnRlbGxpZ2VuY2UgaXMgbm90IGtub3dsZWRnZSBidXQgaW1hZ2luYXRpb24u.
This is called an 'unprotected' JWT because no security was involved - no digital signatures or encryption to 'protect' the JWT to ensure it cannot be changed by 3rd parties.

If we wanted to digitally sign the compact form so that we could at least guarantee that no-one changes the data without us detecting it, we’d have to perform a few more steps, shown next.


JWS Example
Instead of a plain text payload, the next example will use probably the most common type of payload - a JSON claims Object containing information about a particular identity. We’ll also digitally sign the JWT to ensure it cannot be changed by a 3rd party without us knowing.

Assume we have a JSON header and a claims payload:

header

{
  "alg": "HS256"
}
payload

{
  "sub": "Joe"
}
In this case, the header indicates that the HS256 (HMAC using SHA-256) algorithm will be used to cryptographically sign the JWT. Also, the payload JSON object has a single claim, sub with value Joe.

There are a number of standard claims, called Registered Claims, in the specification and sub (for 'Subject') is one of them.

Remove all unnecessary whitespace in both JSON objects:

String header = '{"alg":"HS256"}'
String claims = '{"sub":"Joe"}'
Get their UTF-8 bytes and Base64URL-encode each:

String encodedHeader = base64URLEncode( header.getBytes("UTF-8") )
String encodedClaims = base64URLEncode( claims.getBytes("UTF-8") )
Concatenate the encoded header and claims with a period character '.' delimiter:

String concatenated = encodedHeader + '.' + encodedClaims
Use a sufficiently-strong cryptographic secret or private key, along with a signing algorithm of your choice (we’ll use HMAC-SHA-256 here), and sign the concatenated string:

 SecretKey key = getMySecretKey()
 byte[] signature = hmacSha256( concatenated, key )
Because signatures are always byte arrays, Base64URL-encode the signature and join it to the concatenated string with a period character '.' delimiter:

String compact = concatenated + '.' + base64URLEncode( signature )
And there you have it, the final compact String looks like this:

eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJKb2UifQ.1KP0SsvENi7Uz1oQc07aXTL7kpQG5jBNIybqr60AlD4
This is called a 'JWS' - short for signed JWT.

Of course, no one would want to do this manually in code, and worse, if you get anything wrong, you could introduce serious security problems and weaknesses. As a result, JJWT was created to handle all of this for you: JJWT completely automates both the creation of JWSs and the parsing and verification of JWSs for you.
