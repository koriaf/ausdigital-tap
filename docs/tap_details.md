# TAP Protocol Details

The TAP Protocol is a very simple REST API. One business sends a message directly to another business' TAP endpoint (discovered via the SMP):

 * The sender uses the HTTP POST verb (over HTTPS) to send the signed message to a TAP.
 * The TAP replies with a HATEOS-style list of callback URLs.
 * The TAP notarises some non-sensitive but useful data to the blockchain.


## Sending a business message

Pre-requsites:

 * Sender knows their own business identity, and has access to their own private key.
 * Sender knows the recipient business identity (e.g. ABN).
 * Sender knows recipient Service Metadata Publisher (SMP) address, which can be discovered from business identity using Digital Capability Lookup (DCL).
 * Sender knows appropriate TAP endpoint address for the intended message type (discovered via SMP lookup).
 * Sender knows appropriate recipient public key (discovered via SMP lookup).
 * The sender has a valid cleartext (not encrypted) business document to send (e.g. an invoice), encoding the appropriate business semantics (e.g. UBL2.0) in an appropriate format (e.g. json).

The process is:

 * Sign the business message
 * Identify hashing algorithm
 * Create hash of signed business document
 * Create cyphertext of the signed business document
 * Create message
 * Generate message signature
 * POST message and signature to the recipient TAP


### Sign the Business Document

The TAP protocol can be used to transport any business message in any format. We assume the message is formatted as XML or JSON and UTF-8 encoded.

Message signing is interpreted in the sense used by the OpenPGP standard (RFC4880), however strict compliance would involve 3DES algorithm which is not supported. Approved signing algorithms are those determined `--safe` by the *modern* (Eliptic Curve Cryptography compatible) distribution of GnuPG. This is version 2.1.15 at the time of writing, but any stable release at or above v2.1.15 is appropriate.

Business documents compression is discouraged, because it is redundant due to compression at the HTTP layer (per RFC2616 and RFC1952). Business documents MAY be compressed with ZIP (RFC1951), ZLIB (RFC1950), or BZip2 algorithms.

The business document and signature are combined into a single ASCII-Armoured file per RFC4880 (i.e. including use of Radix-64 to convert UTF-8 to 7 bit ASCII in the signed file). For example, assuming the current working directory contains a business document `doc.json`, a signed document `signed_doc.txt` can be created using the GnuPG command line program like this:

```bash
gpg2 \
 --output "signed_doc.txt" \
 --clearsign "doc.json"
```


### Identify hashing algorithm

 
select sig_algorithm from approved list, e.g. SHA2 (?).

TODO:

 * nominate list of approved algorithms
 * seek feedback from technical working group


### Create hash of signed business document

use sig_algorithm to create hash_of_signed_document

bash example

```bash
openssl dgst -sha256 "signed_doc.txt" \
 | cut -d'=' -f2 \ # remove leading text
 | sed -e 's/^ //g' # remove leading whitespace
```

This is not the message hash. It is notarised to the blockchain then used by a trusted third party (e.g. Creditor) interested third parties to verify that a signed_document was actually sent.


### Create cyphertext of signed business document

signed_document_cyphertext = encypher(own.secret_key, TAP.PubKey, signed_document)

You may wish to archive the signed_document somehow, but won't be needing it further.

### Compose Message

The `message` part is a mixture of cleartext metadata (used by TAPs) and encyphered payload (used by trusted business system components). The cleartext metadata does not contain sensitive business information, wheras access to the business-sensitive information within the payload is not necessary for participating in the TAP protocol.


reference_string = ...

no need for canonical form: valid json document.

TODO:

 * footnote about reference strings: can be anything. Routing Key example. Job Code example, etc.

Python example -> message.json


### Generate signature

The `signature` part is created by a business system component trusted by the sender (with access to the sender's private key material). The signature can also be used to uniquely identify the message contents.


Assuming the current working directory contains the message (as message.json), the following 
```bash
gpg --output message.sig --sign message.json
```
bash example -> signature.sig


### Posting the message to the recipient TAP

Layered on top of HTTP Protocol:

 * MUST use HTTPS.
 * MUST use Content-Type: multipart/form-data. (application-x-www-form-urlencode is not appropriate, because URLENCODING cyphertext is problematic)
 * MAY use Content-Transfer-Encoding: base64 (what else? this might be moot)
 * MUST NOT rely on additional TAP-related information in HTTP headers

Standards compliance:
 * TODO: cite RFC for HTTPS
 * TODO: cite RFC for multipart/form-data

The message is sent to the recieving TAP using HTTP POST operation, with a `Content-Type: multipart/form-data` body. This body contains two parts, `message` and `signature`.

Assuming the current working directory contains the message (as message.json) and signature (as message.sig), the following curl command will post the message to the recipient TAP (replace `<TAP_URL>` with HTTPS URL discovered from the Service Metadata Publisher).

```bash
curl -X POST \
 -H "Content-Type: multipart/form-data" \
 -F "message=@message.json" \
 -F "signature=@message.sig" \
 <TAP_URL>
```

## Receipt and Technical Acknowledgement

When a valid message is received, the TAP issues an HTTP 200 status and returns a response body with `Content-Type: text/json`, containing a HATEOS-style list of callback URLs.




footnote: Receiving TAPs may also use the signature as a filter (messages with invalid signatures MAY be dropped by recieving TAPs, rather than delivered). This allows TAPs to buffer trusted components from nusiance false messages activity by

