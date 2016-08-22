# Introduction

The Ledger Access Point (LAP) is a persistently connected "peer" capable of sending and
receiving business documents, such as invoices. It interacts with other LAPs following the
protocol specified in this document. The LAP is an autonomous agent in business-to-business
document exchange.

A LAP might be provided by a commercial ledger service, or maintained as part of an
independent business system. 

The LAP specification has two parts. The main part (LAP) defines the protocol all peers
must follow (and interfaces they must provide) to send and receive business documents. The
second part is an optional gateway specification (LAPGW), which defines a client-server
protocol for ledgers to interact with independent LAP service providers in a generic way.


## Dependencies

The messages send between LAPs carry payloads that encode
[billing-semantics](https://billing-semantics.readthedocs.io/).
This specification is maintained independently in the
[https://github.com/ausdigital/billing-semantics](https://github.com/ausdigital/billing-semantics)
repository. Future semantic payloads may be supported without change to the protocol.

All LAPs depend on the following ADBC Services:

 * [Capability locator (DCL)](http://capability-locator.readthedocs.io/)
 * [Metadata publisher (SMP)](http://metadata-publisher.readthedocs.io/)
 * [Notary (NTY)](http://notary.readthedocs.io)

LAPGW providers also depend on the
[Identity Provider (IDP)](http://identity-provider.readthedocs.io/). LAPs do not need to
authenticate when they interact with each other, due to use of well known cryprograpic keys
and service endpoint addresses.


## LAP Gateway (LAPGW)

A ledger system that is persistently connected to the internet and natively supports the
LAP protocol has no need for LAPGW, either as a client or server.

There are various reasons why a Ledger may chose to delegate to a 3rd party LAP service.
If they do this, they only need to use the client side of the LAPGW protocol. The LAP
service provider must support the server side of the LAPGW protocol (as well as being a
LAP).

A LAPGW service is an avaialbility mediator (allowing a business ledger to have
intermittent connectivity, and/or to sit behind a firewall), It's slightly more than a
proxy server with store-and-forward capability; aditionally, it performs some simple
functions such as message validation, logging, access control, and protocol callbacks
(including to Notary services).

A LAPGW is minimally trusted; it is not custodian of business key material, is not
capable of inspecting encrypyed documents and cannot sign documents on anyones behalf.


## LAP Protocol Overview

TODO - Alice and Bob story

