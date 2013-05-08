---
layout: post
title: "Writing Good Serial Protocol"
description: ""
category: HowTo 
tags: [serial, protocol]
---
{% include JB/setup %}

### General principle
- Always return a response.
- If you can set any value,  make sure that there's some way to query that same value. Even taking a while to query them all.
- Information(version, copyright, etc) can be queried instead of just display them once at start or reset.
- Document it.
- Use absolute command instead of toggle values.
- Responses should include infomation on the command they are responding to.
- Wrap your message in an envelope, just like: start length message checksum end
- Make sure it works that way you expect to.
- Use ASCII if possible.
- Provide examples.
* Make it easy to debug.(Not require any specific software on PC)

### Google these to find more
- ModBus
- PPP
- SDLC, HDLC: Data link protocol
- Hamming
- AT Command(Hayes command set)
- Firmata
- SCIP
- CRC(MD5, SHA1)
- Protocol Buffers: protobuf protobuf-c
- zmodem
- lwip
- uip
- canfestival
- Open Source HDLC

