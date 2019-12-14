# Cryptocurrency Engineering and Design

## Lecture 1: Signatures, Hashing, Hash Chains, ecash, and Motivation

A cryptocurrency is a p2p electronic currency.

### Primitives for a Cyrptocurrency

#### Hash Functions

`hash(data) -> output`

Data can be any size, but the output is a fixed size. The output will look like noise despite being non-deterministic. It has an "avalanche effect" where changing 1 bit of input changes half the bits of the output.

Hash functions should not have preimage resistance. That is, given y, you cannot find any x such that `hash(x) == y` short of 2<sup>256</sup> operations i.e. (10<sup>78</sup>).

Hash functions should also not have 2nd preimage resistance. Thjat is, given x, y, such that `hash(x) == y`, you cannot find x' where `x' != x` and `hash(x') = y`

Hash functions should also not have collision resistance. That is, nobody should find any x, z such that `x != z` where `hash(x) == hash(z)`. In fact this is possible within 2<sup>256</sup> because of the birthday effect.

Examples of hash functions that have broken collision resistance include sha-1 and md5 (however they have not violated the preimage resistance).

There are no mathematical proofs for hash functions at the moment.

Usages for hash functions include names and references, like for minified JS bundles. They can also be used for references and pointers in programming. Finally they can be used as a sort of commitment where one value is revealed, and the other side of that equation is revealed at a future time.

#### Signatures

What is a signature? It's a way to verify a message encrypted using a public and private key pair. To receive messages from someone you need 3 functions:

`generateKeys()` returns a privateKey and a publicKey pair.

`sign(secretKey, message)` Signs a message given a secretKey and returns a signature.

`verify(publicKey, message, signature)` Verifies the signature on a message from a public key. Returns a boolean value.

"Lamport signatures" is one of the earliest implementations of cryptographic signatures.


