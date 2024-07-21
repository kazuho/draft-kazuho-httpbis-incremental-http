---
title: "Streaming Requests for HTTP"
docname: draft-kazuho-httpbis-streaming-requests-latest
category: std
wg: httpbis
ipr: trust200902
keyword: internet-draft
stand_alone: yes
pi: [toc, sortrefs, symrefs]
author:
-
    fullname: Kazuho Oku
    organization: Fastly
    email: kazuhooku@gmail.com

normative:

informative:


--- abstract

This document specifies the "Request-Streaming" HTTP request header field, which
instructs HTTP intermediaries to begin forwarding the request to downstream
servers before the request body has been received completely.


--- middle

# Introduction

HTTP is a request-response protocol that allows servers to start transmitting a
response before the entire HTTP request has been received. Some applications,
such as Chunked Oblivious HTTP Messages
{{?CHUNKED-OHTTP=I-D.ietf-ohai-chunked-ohttp}}, exploit this capability to turn
a single HTTP request-response pair into a medium for bi-directional
communication, on which multiple application-defined
request-response message pairs are exchanged.

However, this approach is fragile when HTTP intermediaries are involved. HTTP
intermediaries are not only allowed but are frequently deployed to buffer
complete HTTP requests before forwarding them to the backend server.

Should such a buffering HTTP intermediary be present between the client and the
server, the application in question fails to function as intended. Upon sending
the HTTP header section and the initial application message, the client awaits a
response while keeping the HTTP request open for subsequent application
messages. Concurrently, the intermediary delays forwarding until the HTTP
request is fully received. This misalignment leads to a deadlock that prevents
the exchange of application-defined messages, effectively disrupting the
intended bi-directional communication.


To prevent such deadlocks, this document specifies the "Request-Streaming" HTTP
request header field, that instructs HTTP intermediaries to begin forwarding the
HTTP request downstream before receiving the complete request.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# The Request-Streaming Header Field

The Request-Streaming request header field expresses the client's intent for
HTTP intermediaries to start forwarding the request to downstream servers before
the entire request is received.

This header field has just one valid value: "1". Its syntax is defined by the
following ABNF {{!ABNF=RFC5234}}:

~~~
Request-Streaming = 1
~~~

Upon receiving a header section that includes the Request-Streaming header
field, HTTP intermediaries SHOULD NOT buffer the entire request before
forwarding it. Instead, intermediaries SHOULD establish an HTTP channel to
downstream servers, transmit the header section, and continuously forward the
bytes of the request body as they arrive.

Similarly, intermediaries SHOULD forward the bytes of the response body to the
clients as they are received.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
