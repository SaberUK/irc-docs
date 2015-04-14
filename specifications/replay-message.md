# Replay Message Capability

Copyright (C) 2014-2015 Peter "SaberUK" Powell &lt;petpow@saberuk.com&gt;

Unlimited redistribution and modification is allowed provided that the above
copyright notice and this permission notice remains intact.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.

## About

This specification defines the InspIRCd-specific `inspircd.org/replay-message`
capability as implemented by the `m_replaymsg` module.

When a client sends a `NOTICE` or `PRIVMSG` with this capability enabled it
should not automatically display the message to the user. Instead, it should
assume that the server will respond with it as if it was a `NOTICE` or `PRIVMSG`
received from another user but with their `nick!user@host` mask as the source.

If a user receives a `NOTICE` or `PRIVMSG` from their current nick they should
treat it as if they had sent the message to the server.

## Rationale

There are two main reasons for this capability.

  1. If you are using a connection which can have a large amount of latency,
     such as GPRS, then it can be many minutes before a loss of connection can
     be detected by the client. This can result in a user chatting away without
     realising that their messages are going nowhere.

  2. If the server has a feature which modifies user messages (e.g. InspIRCd's
     `m_stripcolor`) then the client has no reasonable way of knowing that the
     user's message has been modified. This was traditionally done using ERR_\*
     numerics but due to the amount of non-standard extensions to the IRC
     protocol it is no longer sanely possible to rely on this method.

## Example Traffic

### Basic Replay

```
[C] PRIVMSG #example :I like turtles.
[S] SaberUK!petpow@saberuk.com PRIVMSG #example :I like turtles.
```

### Modified Replay (hate &#8594; love)

```
[C] PRIVMSG Attila :I hate turtles.
[S] SaberUK!petpow@saberuk.com PRIVMSG Attila :I love turtles.
```

### Message Blocked

```
[C] NOTICE Adam :I hate turtles.
```

## IRCv3

This capability has been adopted in IRCv3.2 under the name `echo-message`. You
can find this on the [IRCv3 website][IRCv3]. It is strongly recommended that
implementers use that capability for future development.

[IRCv3]: https://github.com/ircv3/ircv3-specifications/blob/master/extensions/echo-message-3.2.md "IRCv3 `echo-message` Specification"
