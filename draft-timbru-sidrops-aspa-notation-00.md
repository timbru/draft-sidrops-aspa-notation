%%%
Title = "Human Readable ASPA Notation"
abbrev = "ASPA Notation"
ipr = "trust200902"

[seriesInfo]
status = "informational"
name = "Internet-Draft"
value = "draft-timbru-sidrops-aspa-notation-00"

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
provider-as        = asn / asn-limit-v4 / asn-limit-v6
provider-separator = ", "
uint32             = %d0-4294967295
asn                = ["AS"] uint32
asn-limit-v4       = asn "(v4)"
asn-limit-v6       = asn "(v6)"
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
* Each "asn" value for used for a provider-as must be unique. Assertions
  for the same "asn" with different afiLimit values must be merged.

### provider-as

This field represents a ProviderAS as defined in section 3.3.1 of
[@!I-D.ietf-sidrops-aspa-profile].

A ProviderAS in ASPA consists of a providerASID (section 3.3.1.1) and
an optional afiLimit (section 3.3.1.2). In the notation defined here
we use a simple "asn" to represent a ProviderAS that has no afiLimit,
we use "asn-limit-v4" to represent a ProviderAS with an afiLimit for
IPv4, and we use "asn-limit-v4" to represent a ProviderAS with an afiLimit
for IPv6.

As mentioned earlier: the same "asn" MUST NOT appear more than once in
the providers. There is no point in listing the same "asn" with and
without an afiLimit, as the entry without an afiLimit already encompassed
the other. Similarly, there is no point in listing the same "asn" with
an IPv4 and an IPv6 limit, as this can and must be more concisely
expressed as a single entry without an afiLimit.

## asn

This field can optionally be prepended with the string "AS" followed by
a decimal value of a 32 bit Autonomous System Number using the asplain
presentation as specified in [@!RFC5396]. Decimal values MUST be used,
and values MUST be part of the range 0-4294967295.

## asn-v4

This represents a providerAS that uses an afiLimit for IPv4.

## asn-v6

This represents a providerAS that uses an afiLimit for IPv6.

# Example Notations

~~~
AS65000 => AS65001
AS65000 => AS65002(v4)
AS65000 => AS65001, AS65002(v4), AS65003(v6)

65000 => 65001
65000 => AS65002(v4)
65000 => 65001, 65002(v4), 65003(v6)
~~~

# IANA Considerations

This document has no IANA actions.

# Security Considerations

TBD

# Acknowledgements

TBD

{backmatter}
