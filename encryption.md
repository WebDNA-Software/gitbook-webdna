# Encryption

### \[encrypt] [context](https://docs.webdna.us/contexts-and-tags)

\[encrypt] allows you to store sensitive data in your databases, files and carts. There are a number of encryption methods available in WebDNA.

```
Encrypted text will render as unintelligible, eg: ;)g*fg&x#98d(lk$K]esdL,J^`F*j24h0'1
```

**Parameters**

| Parameter                        | Description                                                                                                                                                                                                                                                                                                                                           |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>method<br>(optional)</p>      | <p>"CyberCash", "APOP", "Base64", "blowfish"<br><strong>WedDNA v8.1+</strong> "SHA256", "twofish", "AES"<br><strong>WebDNA v8.2+</strong> "SHA1", "GCM"<br><strong>WebDNA v8.6+</strong> "bcrypt"<br><strong>WebDNA v8.6.5+</strong> "hmacsha256"<br><strong>WebDNA v8.6.5+</strong> "hmacsha512"</p>                                                 |
| seed                             | (required, except for Base64, SHA256 and APOP/MD5) Key used to encrypt the text. For CyberCash, this should be the MerchantKey you were assigned when you created a CyberCash merchant account. For standard WebDNA encryption, this is your secret key for decryption later. CyberCash encryption is one-way; it cannot be decrypted by your server. |
| <p>file<br>(optional)</p>        | Specifies a file that is to be encoded using Base64. This is useful for sending e-mail attachments using the WebDNA sendmail context. Note that anything between the opening and closing encrypt tag will be ignored if this parameter is present.                                                                                                    |
| <p>emailformat<br>(optional)</p> | Used for Base64 only, this specifies if the resulting encoded string should contain line breaks suitable for e-mail applications. Valid values are either 'T' or 'F' the later being the default. This should be used in conjunction with the file parameter above when sending e-mail attachments from a WebDNA template.                            |

If **method** is not specified, then standard WebDNA encryption is assumed.

\


| Method     | WebDNA Version | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CyberCash  |                | CyberCash is the triple-DES encryption used to communicate with the CyberCash CashRegister servers.                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Base64     |                | Base64 is the encoding (not safe for encryption) standard HTML browsers use for Basic Authentication.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| APOP       |                | APOP is the MD5 encryption used by email servers that support APOP authentication. It is a hash method. A hash is simply a one-way function, that will take a string or data source and create an encrypted looking string.                                                                                                                                                                                                                                                                                                            |
| BLOWFISH   |                | BLOWFISH is a symmetric-key block cipher, designed in 1993 by Bruce Schneier. Blowfish provides a good encryption rate in software and no effective cryptanalysis of it has been found to date. However, TwoFish or AES will be prefered in most cases.                                                                                                                                                                                                                                                                                |
| SHA256     | 8.1+           | SHA256 is one of the strongest hash methods, meaning that no way is known to recover the original string from the hash. It is a Secure Hash Standard.                                                                                                                                                                                                                                                                                                                                                                                  |
| TWOFISH    | 8.1+           | TWOFISH is a symmetric key block cipher with a block size of 128 bits and key sizes up to 256 bits. Twofish is related to and further improves the earlier block cipher Blowfish.                                                                                                                                                                                                                                                                                                                                                      |
| AES        | 8.1+           | AES (Advanced Encryption Standard), is a specification for the encryption of electronic data established by the U.S. National Institute of Standards and Technology. AES has been adopted by the U.S. government and is now used worldwide. AES is included in the ISO/IEC 18033-3 standard. AES is available in many different encryption packages, and is the first publicly accessible and open cipher approved by the National Security Agency (NSA) for top secret information when used in an NSA approved cryptographic module. |
| SHA1       | 8.2+           | SHA1 is considered a weak method but is still requested by services like cloudify.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| GCM        | 8.2+           | Galois/Counter Mode (GCM) is a mode of operation for symmetric key cryptographic block ciphers that has been widely adopted because of its efficiency and performance. GCM throughput rates for state of the art, high speed communication channels can be achieved with reasonable hardware resources. The operation is an authenticated encryption algorithm designed to provide both data authenticity (integrity) and confidentiality.                                                                                             |
| bcrypt     | 8.6+           | Bcrypt is a password salt / hashing setup that conforms with current standards.                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| hmacsha256 | 8.6.5+         | HMACSHA256 is a type of keyed hash algorithm that is constructed from the SHA-256 hash function and used as a Hash-based Message Authentication Code (HMAC). The HMAC process mixes a secret key with the message data, hashes the result with the hash function, mixes that hash value with the secret key again, and then applies the hash function a second time. The output hash is 256 bits in length.                                                                                                                            |
| hmacsha512 | 8.6.5+         | HMACSHA512 is a type of keyed hash algorithm that is constructed from the SHA-512 hash function and used as a Hash-based Message Authentication Code (HMAC). The HMAC process mixes a secret key with the message data and hashes the result. The hash value is mixed with the secret key again, and then hashed a second time. The output hash is 512 bits in length.                                                                                                                                                                 |

Do not lose or change your seed value, as it is unrecoverable; the developers of WebDNA cannot recover encrypted values without the original seed.

**Example WebDNA code:**

```
[encrypt seed=secret_value]Keep this secret[/encrypt]
```

\


```
[decrypt seed=secret_value][YourEncryptedField][/decrypt]
```

**Using a specific method:**

```
[encrypt method=blowfish&seed=secret_value]Keep this secret[/encrypt]
```

When writing your encrypted value to a database, you must wrap the encrypted value in double \[url] tags. When reading them back out of the database, use a single \[unurl] context. See explanation\* below.\
**Example WebDNA code:**

```
[replace ...]UserPassword=[url][url][encrypt seed=1234][UserPassword][/encrypt][/url][/url][/replace][search ...]
  [founditems]
    Your password is: [decrypt seed=1234][unurl][UserPassword][/unurl][/decrypt]
  [/founditems]
[/search]
```

Note that the \[url] tags are _outside_ the \[encrypt] tags when writing to the database, but _inside_ the \[decrypt] tags when retrieving.

**Explanation of the \[url] requirement:**\
When writing to a database, any value that has potentially misunderstood characters, such as "&" "=" and "]" must be wrapped in a \[url] context to change these characters into ascii equivalents (&, etc.). When reading data out of a database, WebDNA automatically converts these escaped characters back to normal characters, so no \[unurl] is needed. However, with encryption, some of the encrypted characters may be high ASCII characters that are not encoded enough with a single \[url]. Running it through a second time will encode them properly. Because WebDNA is already applying one \[unurl] to anything coming out of a database, you will only have to add one \[unurl] to balance the two \[url]s.

**Use this table as a guide to ensure a proper encrypt value:**

| Type      | Example                                                                                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Variables | <p>[text]varSECRET=keep it secret[/text]<br>[url][encrypt seed=<em>1234</em>][varSECRET][/encrypt][/url]</p>                                                        |
| Databases | <p>[text]varSECRET=keep it secret[/text]<br>[append ......]db_value=[url][url][encrypt seed=<em>1234</em>][varSECRET][/encrypt][/url][/url][/append]</p>            |
| Cookies   | <p>[text]varSECRET=keep it secret[/text]<br>[setcookie name=thecookie&#x26;value=[url][url][encrypt seed=<em>1234</em>][varSECRET][/encrypt][/url][/url]]</p>       |
| Orderfile | <p>[text]varSECRET=keep it secret[/text]<br>[setheader cart=[cart]]header40=[url][url][encrypt seed=<em>1234</em>][varSECRET][/encrypt][/url][/url][/setheader]</p> |

**Hash algorithms are one way functions**.\
They turn any amount of data into a fixed-length "fingerprint" that cannot be reversed. They also have the property that if the input changes at all, the resulting hash is completely different. This is great for protecting passwords, because passwords should be stored in a form that protects them even if the password file itself is compromised, but at the same time, we need to be able to verify that a user's password is correct. **This is known as "one way" encrytpion.**\
When the user attempts to login, the hash of the password they entered is checked against the hash of their real password retrieved from the database. If the hashes match, the user is granted access. If not, the user has entered invalid login credentials. The "un-hashed" password is not stored so it can never be recovered if the data is ever compromised.

**Using special-purpose encryption methods:**

| Method                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bcrypt                                | <p><strong>The simplest method:</strong><br>[encrypt method=bcrypt]secretpassword[/encrypt]<br>When only "method" and no "arguments" are provided, WebDNA will generate the prefix and a random salt to use.</p><p>Later, when required to check the hash, simply pass the old hash in as the seed, then the proper salt, prefix, and count are used to regenerate the hash.</p><p><strong>Example WebDNA code:</strong></p><pre><code>[text]myhash=$2y$10$jVw1GVXv56uAr2.zDDriK.BkuxORc1y9.qJbkGyRDLWK26Gy3hRSq[/text]
</code></pre><p><br></p><pre><code>[encrypt method=bcrypt&#x26;seed=[myhash]]secretpassword[/encrypt]
</code></pre><p><br>If the [encrypt] output is identical to the hash, then the password matches the hash.</p><p>WebDNA defaults to a prefix of <em>$2y$</em><br>If a new hash is required that is compatible with older software, simply provide your own prefix.</p><p>A default count of 10 is used.<br>The count can be increased to make passwords harder to break, however, this will slow down the hashing process.<br><em>The count cannot be less than 4, or greater than 31.</em></p><p><strong>Example WebDNA code:</strong></p><pre><code>[encrypt prefix=$2a$&#x26;count=12]password[/encrypt]
</code></pre><p><br><em>If a seed argument is provided, then the prefix and count are used from the seed, and the prefix and count arguments are ignored.</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p>GCM<br>with AES<br>or twofish</p>  | <p>GCM is an authentication encryption mode of operation, it is composed by two separate functions: one for encryption (AES-CTR) and one for authentication (GMAC).</p><p><em>It receives as input:</em><br>1) a Key<br>2) a unique IV (Initialization Vector)<br>3) Data to be processed only with authentication<br>4) Data to be processed by encryption and authentication</p><p><em>It outputs:</em><br>1) The encrypted data of input 4<br>2) An authentication TAG<br>3) The authentication TAG is required as an input to the decryption. If your associated data or your encrypted data has been altered, GCM decryption will recognise the alteration and not output any data or return an error, you should discard the received data without processing it.</p><p><em>Mandatory parameters are:</em><br>1) method=aesgcm <em>or</em> method=twofishgcm<br>2) seed=ascii text value of the key</p><p><strong>Optional parameters are:</strong><br>1) IV = Hex value of initialization vector. If this is not provided during encryption, 8 bytes will be randomly generated by WebDNA and appended to the beginning of ADATA and the output. If it is not provided during decryption, WebDNA will automatically remove the first 8 bytes of data from the beginning of the cipher text to use as the IV and prepend to ADATA.<br>2) ADATA = Hex value of any additional plaintext data that won't be sent in the ciphertext, this will be hashed and validated during decryption.<br>3) TAGSIZE = Integer number of bytes for hash tag, this hash is appended to the end of the encrypted data, the default is 12 bytes.</p><p>All optional parameters are not required, but if they are provided, they need to be identical during encryption and decryption to avoid a hash failure error.<br>A failure produces this output: <em>Exception in DECRYPT command: HashVerificationFilter: message hash or MAC not valid.</em></p><p><br><strong>ENCRYPT WebDNA code examples:</strong><br>TWOFISHGCM:<br></p><pre><code>[encrypt method=twofishgcm&#x26;seed=secret_value]Twofish: passwords, credit card number, etc.[/encrypt]
</code></pre><p><br>AESGCM:<br></p><pre><code>[encrypt method=aesgcm&#x26;seed=secret_value]passwords, credit card number, etc.[/encrypt]
</code></pre><p><br>AESGCM with IV and ADATA and TAGSIZE:<br></p><pre><code>[encrypt method=aesgcm&#x26;seed=secret_value&#x26;iv=0123456789abcdef&#x26;adata=abcdefabcdef&#x26;tagsize=8]passwords, credit card number, etc.[/encrypt]
</code></pre> |
| <p>hmacsha256<br>or<br>hmacsha512</p> | <p>hmacsha256:<br></p><pre><code>[encrypt method=hmacsha256&#x26;seed=secret_value]passwords, credit card number, etc.[/encrypt]
</code></pre><p><br>hmacsha512:<br></p><pre><code>[encrypt method=hmacsha512&#x26;seed=secret_value]passwords, credit card number, etc.[/encrypt]
</code></pre><p><br></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

\


**How to encrypt your templates**

If security of a template is required so that the "source code" of a template is not exposed, it is possible to encrypt an entire template.

To create an encrypted template, the template must first be designed and debuged in the normal way. Then use the \[encrypt] context with a "secret" seed to create the encrypted version of the original template. Note that the "secret" seed value should not be made public. After the template has been encrypted, a special tag must be added to the top of the file so it can be recognized as an encrypted file, and then the "secret" seed value must be encrypted using WebDNA's default encryption.

Use the following WebDNA to do all of the necessary steps at once:\
1\) \[encrypt] the file and add the special WebDNA header tag to the beginning of the file.\
\<!--HAS\_WEBDNA\_TAGS\[!]\[/!]\_ENCRYPTED\_2-->\
2\) Note that any unencrypted text such as copyright information should be inserted before the header tag. The encrypted WebDNA must start on the line after the header tag. In the example below, to automatically convert the template (file.dna) to an encrypted template (newfile.dna), execute the following on another page:

```
[writefile file=newfile.dna&secure=F]<!--HAS_WEBDNA_TAGS[!][/!]_ENCRYPTED_2--> 
[encrypt]seed=1234&product=WDNA[/encrypt] 
[encrypt seed=1234][include file=file.dna&raw=T][/encrypt][/writefile]
```

After using the above template, the file named "newfile.dna" can be used, confident that no-one can read or modify it. "Serving" the template form your web server works just like any other template.

The encrypted template will look like this:\
![](https://docs.webdna.us/assets/images/encrypted\_template.png)

\[!]\[/!] is a special trick to fool WebDNA into thinking this page is not an encrypted page, and should be treated like a normal template. The \[!]\[/!] is removed during processing, which causes the resulting template to contain the correct tag that indicates it is encrypted.

In order to prevent someone from displaying or accessing the decrypted templates, the following precaution has been made: \[include raw=T] cannot be done on encrypted templates; nothing is returned.

\


### \[decrypt] [context](https://docs.webdna.us/contexts-and-tags)

\[decrypt] simply decrypts the encrypted values.\
As per the guidelines of \[encrypt], be careful to \[unurl] values.

**Use this table as a guide to ensure a proper decrypt value:**

| Type      | Example                                                                                                                                         |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Variables | <p>[text]varSECRET=g*fg&#x26;x#98d(lk$K]esdL,J^`F*j24h0'1[/text]<br>[decrypt seed=<em>1234</em>][unurl][varSECRET][/unurl][/decrypt]</p>        |
| Databases | <p>[search ......]<br>  [founditems]<br>    [decrypt seed=<em>1234</em>][unurl][db_value][/unurl][/decrypt]<br>  [/founditems]<br>[/search]</p> |
| Cookies   | \[decrypt seed=_1234_]\[getcookie name=thecookie]\[/decrypt]                                                                                    |
| Orderfile | <p>[orderfile cart=[cart]]<br> [decrypt seed=<em>1234</em>][unurl][header40][/unurl][/decrypt]<br>[/orderfile]</p>                              |

**DECRYPT WebDNA code examples:**

TWOFISHGCM:\


```
[decrypt method=twofishgcm&seed=secret_value&iv=89e12236ce056aa2&adata=89e12236ce056aa2]999fe9ff644e7e835c3d9d46cfedce88b2de6d1b5d114e29f161e6246d40380b639e73f006a073db567c6e7aa5740f9db45d81fd6a646cbd[/decrypt]
```

\
TWOFISHGCM:\


```
[decrypt method=twofishgcm&seed=secret_value]89e12236ce056aa2999fe9ff644e7e835c3d9d46cfedce88b2de6d1b5d114e29f161e6246d40380b639e73f006a073db567c6e7aa5740f9db45d81fd6a646cbd[/decrypt]
```

\
AESGCM:\


```
[decrypt method=aesgcm&seed=secret_value] 1a0ce107ed9ede09835b4c25336d8fd49699d21edd4fa365c545ed52b598b341df124b606934ca10f09bd31fbd3f03e993ca82c3f4d0eb[/decrypt]
```

\
AESGCM with IV and ADATA and TAGSIZE:\


```
[decrypt method=aesgcm&seed=secret_value&iv=0123456789abcdef&adata=abcdefabcdef&tagsize=8]7888f14d14bf5d87df292ad7171d391b9da2d6d42da0247683e95d2774bac82e580bde64c9e56bd97e8783[/decrypt]
```
