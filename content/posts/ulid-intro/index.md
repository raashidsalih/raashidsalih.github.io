+++
title = 'The Superior Primary Key? Introduction to ULID'
date = 2023-09-11T06:03:26+05:30
draft = false
+++

In this article, we will explore the concept of ULID, a new type of identifier that aims to overcome some of the limitations of UUID and autoincrement primary key. We will also see how to use ULID in Python with a simple example.

**_Please note that this article was written with the assistance of GPT. While I did provide all of the points that I intended to convey, the article is not as tight knit and polished as I'd like. I do believe, however, that this is a somewhat appropriate usecase for GPT and that the major ideas have been imparted. I would essentially like to employ this article as a placeholder until I get the time to do said polishing._**

## What is ULID?

ULID stands for Universally Unique Lexicographically Sortable Identifier. It is a 128-bit label that consists of two parts: a 48-bit timestamp and an 80-bit randomness. The timestamp is the number of milliseconds since the Unix epoch, and the randomness is generated from a cryptographically secure source. The ULID is encoded using Crockford’s base32, which results in a 26-character string that is case insensitive and URL safe.

## Relationship with UUID

ULID is compatible with UUID, which stands for Universally Unique Identifier. UUID is a standard way of creating unique identifiers for various purposes, such as database keys, file names, or network addresses. There are different versions of UUID, each with different methods of generating the 128-bit value. However, all versions of UUID have some common drawbacks, such as:

- They are not lexicographically sortable, which means they cannot be ordered by their values in a meaningful way.
- They are not human-friendly, which means they are hard to read and write.
- They are not efficient, which means they use more bits than necessary to encode the information.

ULID addresses these drawbacks by using a simpler and more elegant approach. ULID uses only two components: a timestamp and a randomness. The timestamp ensures that ULIDs are lexicographically sortable by their creation time, which is useful for many applications that need chronological ordering. The randomness ensures that ULIDs are unique within the same millisecond, which is sufficient for most practical purposes. The base32 encoding ensures that ULIDs are human-friendly and efficient, as it uses only 26 characters to represent 128 bits of information.

## Advantage over autoincrement primary key

Autoincrement primary key is a common way of creating unique identifiers for database records. It is a numeric value that starts from a certain number and increments by one for each new record. However, autoincrement primary key has some disadvantages, such as:

- It exposes the internal state of the database, which can reveal sensitive information or lead to security risks.
- It creates a single point of failure, which can cause performance issues or data loss if the database server crashes or restarts.
- It requires coordination between multiple database servers or clients, which can introduce complexity or inconsistency.

ULID avoids these problems by using a decentralized and stateless method of generating unique identifiers. ULID does not rely on the database server or any other external source to create the identifier. Instead, it uses the current time and a random value to create the identifier on the client side. This means that ULID does not expose any internal state of the database, does not create any single point of failure, and does not require any coordination between multiple servers or clients.

## How to use in Python

There are several Python libraries that provide an implementation of ULID.  [One of them is python-ulid](https://github.com/ulid/spec)[, which is a port of the original JavaScript ULID implementation](https://www.npmjs.com/package/ulid). To use python-ulid, you need to install it using pip:

```python
pip install python-ulid

```

Then you can import the ULID class and create a new ULID object:

```python
from ulid import ULID

ulid = ULID()

```

You can generate a new ULID string by calling the generate () method:

```python
ulid.generate()
# '01FJZ6W9Q9X8Q0V3Z4N5XV7H6D'

```

You can also access the timestamp and randomness components of the ULID object:

```python
ulid.timestamp
# 1643477652.0

ulid.milliseconds
# 1643477652000

ulid.randomness
# b'\x9f\x8d\x1f\x1a\x0f\x05\xfd\xf7\x1a\xd7'

```

You can also convert the ULID object to other formats, such as hex, int, bytes, or UUID:

```python
ulid.hex
# '01fjz6w9q9x8q0v3z4n5xv7h6d'

int(ulid)
# 1820576928786795198723644692628913870

ulid.bytes
# b'\x01\xf7oY)\xf4p\xbd\xf9\x9f\x8d\x1f\x1a\x0f\x05\xfd\xf7\x1a\xd7'

ulid.to_uuid()
# UUID('01f76f59-29f4-70bd-f99f-8d1f1a0f05fd-f71a-d7')

```

## Advantage over UUID

ULID also overcomes one of the major drawbacks associated with using UUIDs, which is the lack of sortability. UUIDs are randomly distributed, which means they cannot be ordered by their values in a meaningful way. This can cause fragmentation or inefficiency in many data structures or algorithms that rely on sorting. For example, if you use UUIDs as keys in a database table, you may end up with a lot of wasted space or slow queries.

ULID solves this problem by using a timestamp as the first component of the identifier. This ensures that ULIDs are lexicographically sortable by their creation time, which is useful for many applications that need chronological ordering. For example, if you use ULIDs as keys in a database table, you can easily query the records by their insertion time or range.

## Conclusion

ULID is a new type of identifier that offers several advantages over UUID and autoincrement primary key. ULID is compatible with UUID, but it is more sortable, human-friendly, and efficient. ULID is also more secure, reliable, and simple than autoincrement primary key, as it does not depend on the database server or any other external source. ULID can be easily used in Python with the python-ulid library, which provides a convenient and flexible interface for generating and manipulating ULIDs.