Note: This specification was proposed for inclusion with IRCv3.3 but was
rejected due to lack of interest.

# IRCv3.3 Language Negotiation

Copyright (C) 2014-2015 Peter "SaberUK" Powell &lt;petpow@saberuk.com&gt;

Unlimited redistribution and modification is allowed provided that the above
copyright notice and this permission notice remains intact.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.


## About

This specification allows clients to request that messages from the server are
sent in a specified language. It does not define any translation behaviour for
`NOTICE` or `PRIVMSG` except in the case of network pseudoclients such as
services.

### Capability

Available languages are sent using the `languages` capability. The value of the
capability is a comma delimited list of tokens. The first token is an positive
integer which specifies the number of language codes which can be requested.
All tokens after that are BCP 47 language codes that the server supports in the
order they are used by default.

### Command

Languages are requested with the `LANGUAGE <code>[ <code>]+` command.

It is recommended that clients request similar language codes (e.g. requesting
`es` as well as `es-419`) in case the server does not support phrases in the
preferred language.

If a language request is successful then the server should respond with the
`RPL_YOURLANGUAGESARE` numeric. It is recommended that the user's language
preferences be broadcast to other servers on the network.

If a language request fails due to too many language codes being requested then
the server should respond with `ERR_TOOMANYLANGUAGES` numeric.

If a language request fails due to the client requesting one or more
unsupported language codes then the server should respond with the
`ERR_NOLANGUAGE` numeric.

### Numerics

#### RPL_YOURLANGUAGESARE

`:<server-name> 687 <nick> <code>[ <code>]+ :Language preferences have been set.`

#### RPL_WHOISLANGUAGE

`:<server-name> 690 <nick> <code>[ <code>]+ :can speak these languages.`

#### ERR_TOOMANYLANGUAGES

`:<server-name> 981 <nick> <count> :You specified too many languages.`

#### ERR_NOLANGUAGE

`:<server-name> 982 <nick> <code>[ <code>]+ :Languages are not supported by this server.`

## Example Traffic

### Valid language negotiation

    [S] :irc.example.com CAP * LS :languages=5,en-GB,en-US,fr-CA,de,nl
    [C] CAP REQ language
    [C] LANGUAGE en-GB en-US
    [S] :irc.example.com 687 NickName en-GB en-US :Language preferences have been set.

### Client requested too many languages

    [S] :irc.example.com CAP * LS :languages=2,en-GB,en-US,fr-CA,de,nl
    [C] CAP REQ language
    [C] LANGUAGE fr-CA en-GB en-US
    [S] :irc.example.com 981 NickName 2 :You requested too many languages


### Client requested an invalid language

    [S] :irc.example.com CAP * LS :languages=3,en-GB,de,nl
    [C] CAP REQ language
    [C] LANGUAGE fr-CA en-GB en-US
    [S] :irc.example.com 982 NickName fr-CA en-US :Languages are not supported by this server.

## References

1. [BCP 47](http://tools.ietf.org/rfc/bcp/bcp47.txt)
