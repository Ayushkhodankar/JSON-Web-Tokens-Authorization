# JSON-Web-Tokens-Authorization

<img src="https://th.bing.com/th/id/OIP.1A34osrtczMKlCKGfoXYXAHaED?rs=1&pid=ImgDetMain" alt="jwt"></img>

# JSON Web Token (JWT)

**JSON Web Token (JWT)** is a general-purpose, text-based messaging format for transmitting information in a compact and secure way. While it's most commonly used for sending and receiving identity tokens on the web, JWTs can transmit any type of data.

## Structure of a JWT

A JWT consists of two main parts:
1. **Payload**: The primary data within the JWT, which can be absolutely anything that can be represented as a byte array (such as Strings, images, documents, etc.).
2. **Header**: A JSON object containing name/value pairs that represent metadata about the payload and the message itself.

While the payload can be anything, it is often a JSON object called **Claims** when used for identity-related data. Each name/value pair within the Claims object is called a **claim**. For example, a claim might represent information about a user or computer system.

## Trust and Verifiability

While anyone can create a JWT, it's important to trust the claims made in the payload. This is where JWT security comes into play. JWTs can be secured in two main ways:
- **JWS (JSON Web Signature)**: A cryptographically signed JWT, which ensures that the JWT comes from a trusted source and hasn't been tampered with.
- **JWE (JSON Web Encryption)**: An encrypted JWT, which ensures that the contents of the JWT remain confidential.

By verifying the signature of a JWS or decrypting a JWE, the recipient can confidently trust the JWT's authenticity.

## Compact and Efficient

To make JWTs efficient for web transmission, they can be compacted into **Base64URL-encoded** strings, making them suitable for use in HTTP headers, URLs, and more. They can also be compressed for further efficiency.

---

For more details:
- [JWS (RFC 7515)](https://tools.ietf.org/html/rfc7515)
- [JWE (RFC 7516)](https://tools.ietf.org/html/rfc7516)



