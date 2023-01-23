# Modern Zero Knowledge Cryptography

Notes based on lectures from [zkiap](https://zkiap.com/).

## Additional Resources

[https://learn.0xparc.org/](https://learn.0xparc.org/)

[ZK Proofs](https://crypto.stanford.edu/pbc/notes/crypto/zk.html)

[ZK Repl](https://zkrepl.dev/) is a web IDE for ZK programming

## Lecture 1: Intro to ZK

### Pre-Requisites

- Elementary number theory and group theory
- Basic cryptographic primitives
- Basic algebraic concepts, esp. polynomials

### Basics

What is a Zero Knowledge Protocol? It lets someone prove to someone else that they know a fact without telling them that fact.

An example of this is a cryptographic key. I know the value of a private key, but won't tell you what it is.

The way a ZK Protocol works is as follows:

**A Setup:** a prover wants to convince a verifier that they know something, without revealing the underlying information.

**Verifier:** Asks questions/issues

**Prover:** Submits answers to questions

A classic example of a ZK protocol is a prover claims they can color in a map with n number of colors such that no two adjacent countries will have the same color.

The verifier will examine two random adjacent countries a sufficient number of times and increase their confidence in the prover's claim overtime.

ZK Protocols have 3 properties:

1. Prover's responses don't reveal the underlying information. This is called **Zero Knowledge**.
2. If the Prover knows the underlying information they are always are able to answer satisfactorily. This is called **Completeness**.
3. If the prover doesn't know the underlying information they'll get discovered eventually. This is called **Soundness**.

In the past for each ZK problem there would need to be a bespoke solution to setup a protocol. What would be nice is a more generic approach such that `f(x) = y` e.g. arbitrary computation.

### Anonymous Voting

Setup: 5 people with known public/private keys voting on a question.

Along with my vote, attach a proof:

- I know some secret (sk) such that
    - pubkeygen(sk) = pk
    - (pk - pk1)(pk - pk2)(pk - pk3)(pk - pk4)(pk - pk5) = 0

This would prove that I belong to a set.

Is this enough? No, how does this stop one person voting multiple times? So lets modify the above statement:

For some public nullifier nf, I know some secret sk such that:
    - pubkeygen(sk) = pk
    - (pk - pk1)(pk - pk2)(pk - pk3)(pk - pk4)(pk - pk5) = 0
    - hash(sk) = nf

### zkSNARKS

SNARKS a Succinct Noninteractive ARgument of Knowledge.

zkSNARKS are a new cryptographic tool that can efficiently generate a zk protocol for any problem or function.

Properties:
- zk: hides input
- succinct: generates short proofs that can be verified quickly
- noninteractive: doesn't require a back-and-forth
- argument of knowledge: proves you know the input

It looks like `f(x, w) = bool` where x is a public input, and w is private input (w stands for witness).

High-level idea:

- Turn your problem into a function whose inputs you want to hide
- Turn that function into an equivalent set of "R1CS" (or other) equations.
    - Basically, an arithmetic circuit - a bunch of + and * operations on prime field elements
    - Simplified: equations of the firm x_i + x_j = x_k, or x_i * x_j = x_k
- Generate a ZKP for satisfiability of the R1CS

So, take some function inputs: x1, x2, x3, x4
OUT = f(x) = (x1 + x2) * x3 - x4

- zkSNARK: I know some secret (x1, x2, x3, x4) such that the result of this computation is OUT. Here's a signature that proves that I know such a tuple, without telling you what the tuple actually is.
