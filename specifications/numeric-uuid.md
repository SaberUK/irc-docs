# Numeric UUIDs

Copyright (C) 2014 Peter "SaberUK" Powell &lt;petpow@saberuk.com&gt;

Unlimited redistribution and modification is allowed provided that the above
copyright notice and this permission notice remains intact.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.

## About

Numeric UUIDs are a method of uniquely identifying users on IRC which is in my
opinion vastly superior to the traditional [TS6 UUID format][TS6]. This is
because they can be:

* Generated using the native addition operator of your implementation language.

* Serialised using the native stringifying function of your implementation
  language.

* Deserialised using the native integer parsing function of your implementation
  language.

## Terminology

* SID &mdash; an unsigned 16-bit integer which uniquely identifies a server
  on an IRC network.

* UID &mdash; an unsigned 48-bit integer which uniquely identifies a user
  on an IRC server. UID '0' is reserved and should not be assigned to users.

* UUID &mdash; an unsigned 64-bit integer which uniquely identifies an entity
  on an IRC network. The first 16 bits are the SID and the rest is the UID. If
  the UID is '0' then the UUID identifies a server rather than a user.

## Endianness

In cases where it is necessary to store or transmit numeric UUIDs in a portable
format then network byte order must be used.

## Example C++ Code

### Storage

```cpp
struct User {
	union {	
		struct {
			uint16_t SID;
			uint64_t UID:48;
		};
		uint64_t UUID;
	};
};
```

### Serialising

```cpp
std::to_string(htobe64(user.UUID));
```

### Deserialising

```cpp
user.UUID = be64toh(strtoull("2752546", (char**)NULL, 10));
```

[TS6]: https://github.com/grawity/irc-docs/blob/03ba884a54f1cef2193cd62b6a86803d89c1ac41/server/ts6.txt "TS6 Specification"
