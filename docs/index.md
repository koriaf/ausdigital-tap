# 1/LAP

## ADBC Ledger Access Point Specification

 * Status: raw
 * Editor: Chris Gough
 * Contributors: Steve Capell

This document describes a protocol for exchanging business documents (such as invoices)
between ledgers. LAP is a secure, decentralised, peer to peer architecture where gateways
are optional and minimaly trusted.

This specification aims to support the Australian Digital Business Council
[eInvoicing initiative](https://ausdigital.github.io), and is under active development
at the ausdigital [GitHub site](https://github.com/ausdigital/access-point).


## Licence

Copyright (c) 2016 the Editor and Contributors. All rights reserved.

This Specification is free software; you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software Foundation; 
either version 3 of the License, or (at your option) any later version.

This Specification is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program;
if not, see [http://www.gnu.org/licenses](http://www.gnu.org/licenses).


## Change Process

This document is governed by the [2/COSS](http://rfc.unprotocols.org/spec:2/COSS/) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT",
"RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in
RFC 2119

TODO: define other key terms here


# Introduction

The Ledger Access Point (LAP) is a persistently connected "peer" capable of sending and
receiving business documents, such as invoices. It interacts with other LAPs following the
protocol specified in this document. The LAP is an autonimous agent in business-to-business
document exchange.

A LAP might be provided by a commercial ledger service, or maintained as part of an
independent business system. 

The LAP specification has two parts. The main part (LAP) defines the protocol all peers
must follow (and interfaces they must provide) to send and recieve business documents. The
second part is an optional gateway specification (LAPGW), which defines a client-server
protocol for ledgers to interact with independant LAP service providers in a generic way.


## Dependancies

billing-semantics

All LAPs depend on the following ADBC Services:
 * DCL
 * SMP
 * NTY

LAPGW providers also depend on
 * IDP

TODO: descriptions and links


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

TODO: is a LAPGW is responsible for signature validation? I think it's just a curtesy,
if you want to be sure check it yourself.


## LAP Protocol Overview

TODO - Alice and Bob story


# TODO

Deployment Model and Operator Experience

Protocol Elements
 * HTTPS Requests
 * ...
 * Errors
 * Replay protection ?

Identity Management
 * Resources
 * Directory

Security Considerations
 * Threat model
 * LAPGW: Integrity of Authorizations
 * LAPGW: Preventing Authorization Hijacking
 * Denial-of-Service Considerations
 * ...other policy/controls

References
 * Normative References
 * Informative References
