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

[pi]
 toc = "yes"
 compact = "yes"
 symrefs = "yes"
 sortrefs = "yes"

%%%

.# Abstract

This document defines a human readable ASPA notation for use with RPKI tooling.
The desire is that this will help create consistency across various tools, and
thus support the principle of least surprise to operators. That said, this
definition is informational, and implementations can choose to use their own
notation styles instead.

{mainmatter}

# Requirements notation

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in BCP 14 [@!RFC2119]
[@!RFC8174] when, and only when, they appear in all capitals, as shown here.

# ASPA Notation Definition

This specification uses ABNF syntax specified in [@!RFC5234].

~~~
notation           = customer-as separator provider-as-set

customer-as        = asn
separator          = " => "
provider-as-set    = empty-list / provider-list

empty-list         = "<none>"
provider-list      = provider-as *(provider-separator provider-as)
provider-as        = asn / asn-v4 /asn-v6
provider-separator = ", "

asn              = 1*8DIGIT
asn-v4           = asn "(v4)"
asn-v6           = asn "(v6)"
~~~

## customer-as

This field represents the customerASID defined in section 3.3 of
[@?I-D.ietf-sidrops-aspa-profile]

## provider-as-set

This field represents the providerASSET defined in section 3.4 of
[@?I-D.ietf-sidrops-aspa-profile]. For the moment we assume that version -07
of this document will allow the use of an explicit empty provider AS set,
rather than using AS0 for this purpose.

### empty-list

If an ASPA has no providers this is explicitly specified by using the value "<none>".

### provider-list

If an ASPA has at least one provider then we need to specify them. I.e. we need
to mention the first provider-as and, if applicable, all following provider-as
values separated by a comma and space character. Note that provider-as values
MUST be sorted by their asn value.

### provider-as

A provider-as can be an 'asn', 'asn-v4' or 'asn-v6'. Note that the latter two
options include an 'asn', but limit the scope of the provider to an address family,
IPv4 and IPv6 respectively. A specification MUST NOT contain more than one provider-as
for the same 'asn' - e.g. a provider cannot be defined without address family limit
AND with such a limit.

## asn

This represents the decimal value of a 32 bit Autonomous System Number. Values
MUST be part of the range 0-4294967295.

## asn-v4

This represents a provider ASN which is authorised for IPv4 only.

## asn-v6

This represents a provider ASN which is authorised for IPv6 only.

# Example Notations

~~~
AS65000 => <none>
AS65000 => AS65001
AS65000 => AS65002(v4)
AS65000 => AS65001, AS65002(v4), AS65003(v6)
~~~

# IANA Considerations

This document has no IANA actions.

# Security Considerations

TBD

# Acknowledgements

TBD

{backmatter}
