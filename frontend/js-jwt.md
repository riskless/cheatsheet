### JWT
- JSON Web Token
- Open standard for passing claims (security information) between two parties
	- Self-contained: carries all the information necessary within itself
	- JSON object on its own
- Mainly used in web applications (or services)
	- Can be used in JavaScript, Node.js, Java, Python etc.
	- Can be used/passed as part of URL (Query string), form body parameter, cookie or HTTP Header (x-access-token)
	- Great for single sign-on context

### JWT - Structure
- Three sections separated with dots
	- header, payload and signature
	- All are base64 encoded
- Header - usually contains 2 parts, in the form of JSON
	- typ - should be JWT
	- alg - hashing algorithm (HS256, RS512, ES384 etc.)
- Payload
	- Contains
		- the information which we need to transmit
		- the information related to token itself
		- Information is JSON representation of claims (key:value)
			- iss - issuer
			- sub - subject
			- aud - audience
			- exp - expiration time
			- nbf - not before
			- iat - issued at
			- jti - JWT id

- Signature
	- A hash of header and payload using a secret

```js
var s = base64Encode(header) + "." + base64Encode(payload);
var signature = hashAlgHs256(s, 'secret');

// JWT = header + payload + signature
var jwt = s + "." + base64Encode(signature)
```

### Flow between Client and Server
1. Client
	- Request token (by providing auth. Credentials)
2. Server
	- Check credentials
	- Create token (JWT)
3. Client
	- Verify token (optional)
	- Extract information for app usage (optional)
	- Persist token (at client-side)
4. Server
	- Verify token for all subsequent requests
5. Client
	- Use same (persisted) token for subsequent calls to server
6. Server
	- If valid token, respond with result else, return error

### Base64 Encode using JavaScript
- download CrytoJs
	- https://github.com/brix/crypto-js/tree/develop/src
	- core.js
	- enc-base64.js
	- hmac.js
	- sha256.js

- index.html
```html
<script src="core.js"></script>
<script src="enc-base64.js"></script>
<script src="hmac.js"></script>
<script src="sha256.js"></script>

<input type="text" id="encode"/>
<button onclick="doEncode()">Encode</button>
<span id="resultEncode"></span>

<script>
function getBase64Encoded(rawStr) {
	var wordArray = CryptoJS.enc.Utf8.parse(rawStr);
	var reusult = CryptoJS.enc.Base64.stringify(wordArray);
	return result;
}

function doEncode() {
	var encode = document.getElementById("encode");
	var resultEncode = document.getElementById("resultEncode");
	resultEncode.innerText = getBase64Encoded(encode.value);
}
</script>
```

### Base64 Decode using JavaScript
- index.html
```html
<input type="text" id="decode"/>
<button onclick="doDecode()">Decode</button>
<span id="resultDecode"></span>

<script>
function getBase64Decoded(encStr) {
	var wordArray = CryptoJS.enc.Base64.parse(encStr);
	var reusult = wordArray.toString(CryptoJS.enc.Utf8);
	return result;
}

function doDecode() {
	var decode = document.getElementById("decode");
	var resultDecode = document.getElementById("resultDecode");
	resultEncode.innerText = getBase64Decoded(decode.value);
}
</script>
```

### How to create JWT using JavaScript
- index.html
```html
<input type="text" id="header"/>
<input type="text" id="payload"/>
<input type="text" id="secret"/>
<button onclick="createJWT()">Show JWT</button>
<div id="resultJWT"></div>

<script>
function createJWT() {
	var header = document.getElementById("header");
	var payload = document.getElementById("payload");
	var secret = document.getElementById("secret");
	resultJWT = document.getElementById("resultJWT");
	
	var base64Header = getBase64Encoded(header.value);
	var base64Payload = getBase64Encoded(payload.value);

	var signature = CyptoJS.HmacSHA256(base64Header+"."+base64Payload, secret);
	var base64Signature = CrytoJS.enc.Base64.stringify(signature);

	// JWT = header + payload + signature
	var jwt = base64Header + "." + base64Payload + "." + base64Signature;
	resultJWT.innerText = jwt;
}
</script>
```

### References
- Create JWT
http://jwtbuilder.jamiekurtz.com/
http://kjur.github.io/jsjws/

- Verify JWT
https://jwt.io/

- Base64 encode/decode
https://www.base64encode.org/
https://www.base64decode.org/

- [JSON Web Token](https://www.youtube.com/watch?v=oXxbB5kv9OA&list=RDCMUCJ1GreMvJv6U5JtPGCinwJw&index=2)
- [How to create JWT (JSON web token) using pure JavaScript (and Crypto-Js)](https://www.youtube.com/watch?v=oneCuYkWz0c&list=RDCMUCJ1GreMvJv6U5JtPGCinwJw&index=2)
