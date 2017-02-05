 * Spec ID: ausdigital.org/ausdigital-tap/1.0
 * ![raw](http://rfc.unprotocols.org/spec:2/COSS/raw.svg)
 * Editor: the Digital Business Council (Council)
 * Contributors: 

## AusDigital Transaction Access Point (TAP) 1.0 Specification

This document describes a protocol for exchanging formal documents (such as invoices) between businesses. TAP is a secure, decentralised, peer to peer architecture where gateways are optional and minimally trusted.

## Overview

The Access Point Implementation Guide defines the standards for delivering and receiving of digital business documents within the Council’s Interoperability Framework (Framework). The Framework defines the following components:
* Access Point: A sender or receiver of digital business documents;
* Digital Capability Locator: A service for looking up the location of the Digital Capability Publisher for a Participant. (Digital Business Council, 2016a);
* Digital Capability Publisher: A service for Participants to store details of their capabilities, and includes what scenarios they can process, the data formats they support and the delivery address for their eInvoices. (Digital Business Council, 2016b); and
* Electronic Business Documents: A digital representation of a business document, e.g. an eInvoice.

![Figure 1: Context of Access Points within the Interoperability Framework](./tap_business_context_diagram.png "Figure 1: Context of Access Points within the Interoperability Framework")

Figure 1: Context of Access Points within the Interoperability Framework

The messaging protocol described in this document is business process and business document agnostic. The underlying OASIS ebMS3 standard has been designed to be flexible and support a large number of use cases. This implementation guide reduces complexity by focusing on the exchange of business documents regardless of their type or the process in which they are used.

The context diagram Figure 1 shows the components included in the Interoperability Framework and where the Access Point (AP) sits. The Digital Capability Publisher and Digital Capability Locator are used by the Access Points and enable the eInvoicing process.

The components of the framework are used in a four-corner model (see Figure 2: Four Corner Model). End users, or participants, don’t directly exchange electronic information but connect through a network of Access Points. The digital address for a receiving Access Point is dynamically discovered using the Digital Capability Locator (DCL) and Digital Capability Publisher(DCP) components of the Framework. Access Points may also update a participant’s digital endpoint in the DCP.

![Figure 2: Four Corner Model](./tap_four_corner_model.png "Figure 2: Four Corner Model")

Figure 2: Four Corner Model

See the complete [Access Point Implementation Guide](./Access_Point_Implementation_Guide_v1.0.pdf) for further details.

## Licence

Copyright (c) 2016 the Editor and Contributors. All rights reserved.

This work is licensed under a Creative Commons Attribution 4.0 International License.

## Change Process

This document is governed by the [2/COSS](http://rfc.unprotocols.org/spec:2/COSS/) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT",
"RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in
RFC 2119.
