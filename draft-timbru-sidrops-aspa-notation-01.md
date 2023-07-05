%%%
Title = "Human Readable ASPA Notation"
abbrev = "ASPA Notation"
ipr = "trust200902"

[seriesInfo]
status = "informational"
name = "Internet-Draft"
value = "draft-timbru-sidrops-aspa-notation-01"

[[author]]
initials="T."
surname="Bruijnzeels"
fullname="Tim Bruijnzeels"
organization = "NLnet Labs"
  [author.address]
  email = "tim@nlnetlabs.nl"

[[author]]
initials="O."
surname="Borchert"
fullname="Oliver Borchert"
organization = "NIST"
  [author.address]
  email = "oliver.borchert@nist.gov"

[[author]]
initials="D."
surname="Ma"
fullname="Di Ma"
organization = "ZDNS"
  [author.address]
  email = "madi@zdns.cn"

[pi]
 toc = "yes"
 compact = "yes"
 symrefs = "yes"
 sortrefs = "yes"

%%%

.# Abstract

This document defines a human readable notation for Validated ASPA
Payloads (VAP, see ID-aspa-profile) for use with RPKI tooling based on
ABNF (RFC 5234).

{mainmatter}

# Requirements notation

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in BCP 14 [@!RFC2119]
[@!RFC8174] when, and only when, they appear in all capitals, as shown here.

# Introduction

This informational document defines a human readable ASPA notation for
Validated ASPA Payloads (VAPs) [@!I-D.ietf-sidrops-aspa-profile].

The main motivations for providing this notations style are:
* This can help to create consistency between RPKI Relying Party
  software output, making it easier for operators to compare results.
* This can be used by RPKI Certificate Authorities (CA) command line
  interfaces and/or configuration. E.g. allowing a CA to provide a
  listing of intended VAPs which can be easily compared to RP output.
* This can be used for documentation.

That said, this definition is informational. Implementations can choose
to use their own notation styles instead of, or in addition to this.

# ASPA Notation Definition

This specification uses ABNF syntax specified in [@!RFC5234].

~~~
notation           = customer-asid separator providers
customer-asid      = asn
separator          = " => "
providers          = provider-as *(provider-separator provider-as)
provider-as        = asn
provider-separator = ", "
asn                = ["AS"] uint32
uint32             = %d0-4294967295
~~~

## customer-asid

This field represents the customerASID defined in section 3.2 of
[@!I-D.ietf-sidrops-aspa-profile]

## providers

This field represents the providers defined in section 3.3 of
[@!I-D.ietf-sidrops-aspa-profile]. Note that the normative constraints
which are defined in that section mean that following :

* There must be at least one provider-as.
* The customer-asid "asn" value must not appear in any provider-as.
* The elements of providers must be ordered in ascending numerical order
  by the "asn" value of the provider-as field.
* Each "asn" value for used for a provider-as must be unique.

### provider-as

This field represents a Provider AS as defined in section 3.3 of
[@!I-D.ietf-sidrops-aspa-profile].

## asn

This field can optionally be prepended with the string "AS" followed by
a decimal value of a 32 bit Autonomous System Number using the asplain
presentation as specified in [@!RFC5396]. Decimal values MUST be used,
and values MUST be part of the range 0-4294967295.

# Example Notations

~~~
AS65000 => AS65001
AS65000 => AS65002
AS65000 => AS65001, AS65002, AS65003

65000 => 65001
65000 => AS65002
65000 => 65001, 65002, 65003
~~~

# IANA Considerations

This document has no IANA actions.

# Security Considerations

TBD

# Acknowledgements

TBD

{backmatter}
