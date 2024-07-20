---
title: "Streaming Reqeuests for HTTP"
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

This document specifies the "Request-Streaming" HTTP request header field, that
instructs HTTP intermediaries to start forwarding the request to downstream
before receiving a compelete request.


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
the initial message, the client awaits a response while keeping the request
channel open for subsequent messages. Concurrently, the intermediary delays
forwarding until the full request is received. This misalignment leads to a
deadlock that prevents the exchange of application-defined messages, effectively
disrupting the intended bi-directional communication.


To prevent such deadlocks, this document specifies the "Request-Streaming" HTTP
request header field.

This HTTP field instructs HTTP intermediarires to start forwarding the HTTP
request downstream before receiving the complete request.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
