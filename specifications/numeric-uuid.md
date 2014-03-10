# Numeric UUIDs

Copyright (C) 2014 Peter "SaberUK" Powell &lt;petpow@saberuk.com&gt;

Unlimited redistribution and modification is allowed provided that the above
copyright notice and this permission notice remains intact.

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
  on an IRC server.

* UUID &mdash; an unsigned 64-bit integer which uniquely identifies a user
  on an IRC network. The first 16 bits are the SID and the rest is the UID.

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
std::to_string(user.UUID);
```

### Deserialising

```cpp
user.UUID = strtoull("2752546", (char**)NULL, 10);
```

[TS6]: https://github.com/grawity/irc-docs/blob/03ba884a54f1cef2193cd62b6a86803d89c1ac41/server/ts6.txt "TS6 Specification"
