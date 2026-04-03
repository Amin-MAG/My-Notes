---
title: Diffie-Hellman
draft: true
tags: [cryptography, security, networking, reference]
---
# Diffie-Hellman Key Exchange

> **The art of agreeing on a secret in public.**

Diffie-Hellman (DH) is a cryptographic protocol that allows two parties to establish a shared secret key over an insecure, fully public channel — without ever transmitting the secret itself. Introduced by Whitfield Diffie and Martin Hellman in 1976, it is the backbone of TLS, SSH, Signal, and virtually every secure protocol in use today.

---

## Table of Contents

- [The Problem](#the-problem)
- [The Intuition: Paint Analogy](#the-intuition-paint-analogy)
- [The Math: Modular Exponentiation](#the-math-modular-exponentiation)
- [The Protocol](#the-protocol)
- [Why Eve Can't Break It](#why-eve-cant-break-it)
- [Known Attacks on the Discrete Logarithm Problem](#known-attacks-on-the-discrete-logarithm-problem)
- [Security Recommendations](#security-recommendations)
- [ECDH: The Modern Variant](#ecdh-the-modern-variant)
- [Limitations](#limitations)

---

## The Problem

Alice and Bob want to communicate securely. They need a shared encryption key. But:

- They have never met in person.
- Every message they send is visible to Eve (an eavesdropper).
- They cannot physically hand each other a key.

How do you agree on a secret when everything you say is public?

---

## The Intuition: Paint Analogy

The core insight is that **mixing colors is easy, but unmixing them is hard**. This is a physical analogy for a one-way mathematical function.

```
Step 1 — Both agree on a public color (yellow):

        Alice                   Bob
          |                      |
          +----<  yellow  >------+
          |    (public, known)   |

Step 2 — Each picks a private secret color:

      [red] Alice            Bob [blue]

Step 3 — Each mixes their secret with the public color and sends the result:

      Alice sends: orange (yellow + red)  ──────►  Bob receives orange
      Bob sends:   green  (yellow + blue) ◄──────  Alice receives green

Step 4 — Each adds their own secret to what they received:

      Alice: orange + red  = brown (the shared secret)
      Bob:   green  + blue = brown (the same shared secret!)

Eve sees: yellow, orange, green — but cannot unmix them to find red or blue.
```

The shared secret (brown) was **never transmitted**. It was independently derived by both parties.

---

## The Math: Modular Exponentiation

DH replaces paint mixing with **modular exponentiation**, which has the same one-way property.

### Modular Arithmetic

The modulo operation returns the remainder of division:

```
17 mod 5 = 2
```

### The Trapdoor Function

Given a generator `g`, a prime `p`, and a private key `a`:

```
A = g^a mod p
```

- Computing `A` from `g`, `p`, and `a` is **fast** (polynomial time).
- Computing `a` from `g`, `p`, and `A` is the **Discrete Logarithm Problem (DLP)** — computationally infeasible for large primes.

This asymmetry is the entire security foundation of Diffie-Hellman.
