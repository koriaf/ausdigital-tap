# TAP Protocol Details

The TAP Protocol is a very simple REST API. One business sends a message directly to another business' TAP endpoint (discovered via the SMP):

 * The sender uses the HTTP POST verb (over HTTPS) to send the signed message to a TAP.
 * The TAP replies with a HATEOS-style list of callback URLs.
 * The TAP notarises some non-sensitive but useful data to the blockchain.


## Sending a business message

HTTP POST (HTTPS).

Pre-requsites:

 * Sender knows their own business identity, and has access to their own private key.
 * Sender knows the recipient business identity (e.g. ABN).
 * Sender knows recipient Service Metadata Publisher (SMP) address, which can be discovered from business identity using Digital Capability Lookup (DCL).
 * Sender knows appropriate TAP endpoint address for the intended message type (discovered via SMP lookup).
 * Sender knows appropriate recipient public key (discovered via SMP lookup).
 * The sender has a valid cleartext (not encrypted) business document to send (e.g. an invoice), encoding the appropriate business semantics (e.g. UBL2.0) in an appropriate format (e.g. json).


### Sign the Business Document

signed_document = sign(own.secret_key, TAP.PubKey, document)

GPG example.

TODO:

 * nominage use of GnuPG, identify appropriate RFCs etc.


### Identify hashing algorithm
 
select sig_algorithm from approved list, e.g. SHA2 (?).

TODO:

 * nominate list of approved algorithms
 * seek feedback from technical working group


### Create hash of signed business document

use sig_algorithm to create hash_of_signed_document

bash example

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

The `signature` part is created by a business system component trusted by the sender (with access to the sender's private key material). The signature is also used as a unique message id.

bash example -> message_sig.txt


### Posting the message to the recipient TAP

Layered on top of HTTP Protocol:

 * MUST use HTTPS.
 * MUST use Content-Type: multipart/form-data. (application-x-www-form-urlencode is not appropriate, because URLENCODING cyphertext is problematic)
 * MAY use Content-Transfer-Encoding: base64 (what else? can it be implied)
 * MUST NOT rely on additional TAP-related information in HTTP headers

The message is sent to the recieving TAP using HTTP POST operation, with a `Content-Type: multipart/form-data` body. This body contains two parts, `message` and `signature`.

python example, using message.json + message_sig.txt


## Receipt and Technical Acknowledgement

When a valid message is received, the TAP issues an HTTP 200 status and returns a response body with `Content-Type: text/json`, containing a HATEOS-style list of callback URLs.




footnote: Receiving TAPs may also use the signature as a filter (messages with invalid signatures MAY be dropped by recieving TAPs, rather than delivered). This allows TAPs to buffer trusted components from nusiance false messages activity by

