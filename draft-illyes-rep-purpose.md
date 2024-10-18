---
title: "Robots Exclusion Protocol User Agent Purpose Extension"
abbrev: "REP purpose"
category: info

docname: draft-illyes-rep-purpose-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date: 2024-10-18
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
 - robotstxt
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "garyillyes/ietf-rep-purpose"
  latest: "https://garyillyes.github.io/ietf-rep-purpose/draft-illyes-rep-purpose.html"

author:
 -
    fullname: "Gary Illyes"
    organization: Google LLC.
    email: "garyillyes@google.com"

normative:
  RFC9110
  RFC9309
  
informative:
  RFC523

--- abstract

The Robots Exclusion Protocol defined in RFC9309 specifies the user-agent
rule for targeting automatic clients either by prefix matching their 
self-defined product token or by a global rule * that matches all clients.

This document extends RFC9309 by defining a new rule for targeting 
automatic clients based on the clients' purpose for accessing the service.

--- middle

# Introduction

[fill in]

# Specification

We define user-agent-purpose as the new rule with a predefined set of 
values. The values are registered with IANA at ...
Below is an Augmented Backus-Naur Form (ABNF) description, as described 
in RFC5234.

~~~~~~~~~~
purpose = *WS "user-agent-purpose" *WS ":" *WS purpose-token NL
purpose-token = "EXAMPLE-PURPOSE-1" /"EXAMPLE-PURPOSE-2" / "EXAMPLE-PURPOSE-3" ; but check IANA for full list
NL = %x0D / %x0A / %x0D.0A
WS = %x20 / %x09
~~~~~~~~~~

## user-agent-purpose

The `user-agent-purpose` rule is semantically equivalent to the 
`user-agent` rule defined in Section 2.2.1. of RFC9309. As the 
`user-agent` rule, `user-agent-purpose` acts as a starter of rule 
groups. 

## user-agent-purpose tokens

The `user-agent-purpose` token MUST be a substring of the 
identification string that the automatic client sends to the service. 
For example, in the case of HTTP RFC9110, the purpose token MUST be 
a substring in the User-Agent header, along with the product token. 
Here's an example of a User-Agent HTTP request header with the 
purpose token by the product token: 

~~~~~~~~~~
User-Agent: Mozilla/5.0 (compatible; ExampleBot/0.1; ExamplePurpose; https://www.example.com/bot.html)
~~~~~~~~~~

The purpose token MUST be one of the tokens registered with IANA. 
Unrecognized tokens MAY be discarded by parsers. Crawlers MUST use 
case-insensitive matching to find the group that matches the purpose 
token and obey the rules of the group. If there's a group that 
matches the product token of the automatic client, the client SHOULD 
obey that group. If no matching group exists, crawlers MUST obey the 
group with a user-agent line with the "*" value, if present.
If there is more than one group matching the `user-agent-purpose`, 
the matching groups' rules MUST be combined into one group and parsed
according to Section X.

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

The security considerations are the same as in the parent RFC9309.


# IANA Considerations

The vocabulary used as purpose tokens are registered at IANA-URL.

# Examples
~~~~~~~~~~
# robots.txt with purpose
# FooBot and all bots that are crawling for EXAMPLE-PURPOSE-1 are disallowed.
User-Agent: FooBot
User-Agent-Purpose: EXAMPLE-PURPOSE-1
Disallow: / 
# EXAMPLE-PURPOSE-2 crawlers are allowed.
User-Agent-Purpose: EXAMPLE-PURPOSE-2
~~~~~~~~~~

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
