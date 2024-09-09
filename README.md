# JSON-Web-Tokens-Authorization

<img src="https://th.bing.com/th/id/OIP.1A34osrtczMKlCKGfoXYXAHaED?rs=1&pid=ImgDetMain" alt="jwt"></img>

 What is a JSON Web Token?

JSON Web Token (JWT) is a _general-purpose_ text-based messaging format for transmitting information in a
compact and secure way.  Contrary to popular belief, JWT is not just useful for sending and receiving identity tokens
on the web - even if that is the most common use case.  JWTs can be used as messages for _any_ type of data.

A JWT in its simplest form contains two parts:

. The primary data within the JWT, called the `payload`, and
. A JSON `Object` with name/value pairs that represent metadata about the `payload` and the
message itself, called the `header`.

A JWT `payload` can be absolutely anything at all - anything that can be represented as a byte array, such as Strings,
images, documents, etc.

But because a JWT `header` is a JSON `Object`, it would make sense that a JWT `payload` could also be a JSON
`Object` as well. In many cases, developers like the `payload` to be JSON that
represents data about a user or computer or similar identity concept. When used this way, the `payload` is called a
JSON `Claims` object, and each name/value pair within that object is called a `claim` - each piece of information
within 'claims' something about an identity.

And while it is useful to 'claim' something about an identity, really anyone can do that. What's important is that you
_trust_ the claims by verifying they come from a person or computer you trust.

A nice feature of JWTs is that they can be secured in various ways. A JWT can be cryptographically signed (making it
what we call a https://tools.ietf.org/html/rfc7515[JWS]) or encrypted (making it a
https://tools.ietf.org/html/rfc7516[JWE]).  This adds a powerful layer of verifiability to the JWT - a
JWS or JWE recipient can have a high degree of confidence it comes from someone they trust
by verifying a signature or decrypting it. It is this feature of verifiability that makes JWT a good choice
for sending and receiving secure information, like identity claims.

Finally, JSON with whitespace for human readability is nice, but it doesn't make for a very efficient message
format.  Therefore, JWTs can be _compacted_ (and even compressed) to a minimal representation - basically
Base64URL-encoded strings - so they can be transmitted around the web more efficiently, such as in HTTP headers or URLs.


