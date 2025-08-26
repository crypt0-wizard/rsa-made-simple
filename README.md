# How Two Primes Changed the World

> Because cryptography doesn’t have to feel like punishment by math gods.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Why This Guide Exists

Most RSA explanations either gloss over the mathematics or get lost in implementation details. This guide focuses on the beautiful mathematical theory that makes RSA work - the number theory, the proofs, and the elegant connections between seemingly unrelated mathematical concepts.

## Mathematical Foundations

### Prime Numbers and Factorization

The security of RSA rests on a fundamental asymmetry in computational complexity:

**Easy Problem**: Given two prime numbers p and q, compute n = p × q
**Hard Problem**: Given n, find the prime factors p and q

This is the **Integer Factorization Problem**. For large enough numbers (1024+ bits), no efficient classical algorithm exists to factor n back into p and q.

### Modular Arithmetic

RSA operates entirely within modular arithmetic. Key concepts:

**Congruence**: a ≡ b (mod n) means n divides (a - b)

**Properties**:
- (a + b) mod n = ((a mod n) + (b mod n)) mod n  
- (a × b) mod n = ((a mod n) × (b mod n)) mod n
- (a^k) mod n can be computed efficiently using repeated squaring

**Multiplicative Group**: The set Z*_n = {x : 1 ≤ x < n, gcd(x,n) = 1} forms a group under multiplication modulo n.

### Euler's Totient Function

For any positive integer n, φ(n) counts the number of integers from 1 to n that are coprime to n.

**Key Properties**:
- If p is prime: φ(p) = p - 1
- If p, q are distinct primes: φ(pq) = φ(p)φ(q) = (p-1)(q-1)
- For any a coprime to n: a^φ(n) ≡ 1 (mod n) (Euler's theorem)

**Example**:
```
n = 15 = 3 × 5
φ(15) = φ(3)φ(5) = 2 × 4 = 8
Numbers coprime to 15: {1, 2, 4, 7, 8, 11, 13, 14}
```

### The Extended Euclidean Algorithm

Given integers a and b, the Extended Euclidean Algorithm finds integers x and y such that:
ax + by = gcd(a, b)

**RSA Application**: To find d such that ed ≡ 1 (mod φ(n)), we solve:
ed + kφ(n) = 1

This gives us the multiplicative inverse: d = e^(-1) mod φ(n)

## The RSA Algorithm

### Key Generation

**Input**: Desired key length k (in bits)

**Step 1**: Choose two random primes p and q of approximately k/2 bits each
- Ensure p ≠ q
- For security, |p - q| should not be too small

**Step 2**: Compute n = pq and φ(n) = (p-1)(q-1)

**Step 3**: Choose public exponent e such that:
- 1 < e < φ(n)
- gcd(e, φ(n)) = 1

Common choices: e = 3, 17, 65537 (2^16 + 1)

**Step 4**: Compute private exponent d such that:
- ed ≡ 1 (mod φ(n))
- This is done using the Extended Euclidean Algorithm

**Output**:
- Public key: (n, e)
- Private key: (n, d)
- Secret: p, q, φ(n) (must be destroyed)

### Encryption and Decryption

**Encryption**: Given message m where 0 ≤ m < n:
c = m^e mod n

**Decryption**: Given ciphertext c:
m = c^d mod n

### Correctness Proof

**Theorem**: RSA decryption correctly recovers the original message.

**Proof**: We need to show that (m^e)^d ≡ m (mod n)

This is equivalent to showing: m^(ed) ≡ m (mod n)

Since ed ≡ 1 (mod φ(n)), we have ed = 1 + kφ(n) for some integer k.

Therefore: m^(ed) = m^(1 + kφ(n)) = m · (m^φ(n))^k

**Case 1**: gcd(m, n) = 1
By Euler's theorem: m^φ(n) ≡ 1 (mod n)
So: m^(ed) ≡ m · 1^k ≡ m (mod n)

**Case 2**: gcd(m, n) > 1
Since n = pq with distinct primes, either p|m or q|m (but not both, as m < n).
Without loss of generality, assume p|m.

By Chinese Remainder Theorem and Fermat's Little Theorem:
- m^(ed) ≡ 0 ≡ m (mod p)
- m^(ed) ≡ m (mod q) (since gcd(m,q) = 1)

Therefore: m^(ed) ≡ m (mod n) ∎

## Digital Signatures

RSA supports digital signatures through mathematical duality:

**Signing**: s = m^d mod n (using private key)
**Verification**: m' = s^e mod n (using public key)

**Correctness**: (m^d)^e = m^(ed) ≡ m (mod n)

**Security Property**: Only the holder of the private key d can produce valid signatures, but anyone with the public key e can verify them.

## Mathematical Security Analysis

### The Factorization Assumption

RSA security relies on the assumption that factoring large composite numbers is computationally intractable.

**Best Known Algorithms**:
- Trial Division: O(√n)
- Pollard's ρ: O(n^(1/4))
- Quadratic Sieve: O(e^(√(ln n ln ln n)))
- General Number Field Sieve: O(e^((64/9)^(1/3)(ln n)^(1/3)(ln ln n)^(2/3)))

### Key Size Security Estimates

Based on GNFS complexity:

| Key Size | Security Level | Years to Factor* |
|----------|---------------|------------------|
| 512 bits | Broken | Minutes |
| 1024 bits | Weak | ~1 year |
| 2048 bits | Good | ~300,000 years |
| 3072 bits | Strong | 10^15+ years |
| 4096 bits | Very Strong | 10^20+ years |

*Using current best hardware and algorithms

### Attack Vectors

**Small e Attack**:
If e is small (e.g., e = 3) and the message m is small such that m^e < n, then the ciphertext c = m^e can be attacked by simply taking the e-th root over the integers.

**Common Modulus Attack**:
If the same modulus n is used with different exponent pairs (e₁, d₁) and (e₂, d₂), an attacker can recover plaintexts without factoring n.

**Chosen Ciphertext Attack**:
An attacker can potentially decrypt arbitrary ciphertexts by exploiting the multiplicative property of RSA:
Enc(m₁) · Enc(m₂) = Enc(m₁ · m₂)

**Low Private Exponent Attack (Wiener's Attack)**:
If d < n^(1/4)/3, then d can be recovered efficiently using continued fractions.

### Mathematical Countermeasures

**Optimal Asymmetric Encryption Padding (OAEP)**:
Instead of encrypting m directly, encrypt OAEP(m), which includes randomness and redundancy.

**Probabilistic Signature Scheme (PSS)**:
Similar to OAEP but for signatures, preventing existential forgery attacks.

**Chinese Remainder Theorem Optimization**:
For decryption, compute:
- m₁ = c^(d mod (p-1)) mod p
- m₂ = c^(d mod (q-1)) mod q
- Combine using CRT: m = CRT(m₁, m₂)

This is ~4x faster than direct computation of c^d mod n.

## Advanced Mathematical Topics

### Strong Prime Generation

A prime p is considered "strong" if:
- p - 1 has a large prime factor r
- p + 1 has a large prime factor s  
- r - 1 has a large prime factor t

This provides additional resistance against certain factoring algorithms.

### Quadratic Residues and RSA

An integer a is a quadratic residue modulo n if there exists x such that x² ≡ a (mod n).

The **Quadratic Residuosity Problem** is: given n = pq and a, determine if a is a quadratic residue mod n.

This problem is believed to be as hard as factoring and forms the basis of other cryptographic schemes.

### Primality Testing Theory

**Miller-Rabin Test**: A probabilistic test based on:
If p is prime and a^(p-1) ≡ 1 (mod p), then either:
- a^d ≡ 1 (mod p), or  
- a^(d·2^r) ≡ -1 (mod p) for some 0 ≤ r < s

where p - 1 = d · 2^s with d odd.

**Deterministic Tests**: For practical RSA key generation, combinations of Miller-Rabin with specific witnesses provide deterministic results for numbers below certain thresholds.

## Theoretical Limitations

### Information Theoretic Security

RSA is not information-theoretically secure - given unlimited computational resources, RSA can always be broken by factoring n.

Compare to **One-Time Pad**: c = m ⊕ k where k is truly random. This is information-theoretically secure because each plaintext is equally likely for any given ciphertext.

### Quantum Cryptanalysis

**Shor's Algorithm** (1994): A polynomial-time quantum algorithm for integer factorization.

On a sufficiently large quantum computer:
- Factoring n-bit numbers takes O(n³) time
- All current RSA keys become breakable

**Timeline**: Practical quantum computers capable of breaking RSA are estimated to arrive in 15-30 years.

### Mathematical Relationships

RSA connects several areas of mathematics:

**Number Theory**: Primes, factorization, modular arithmetic
**Abstract Algebra**: Groups, rings, Chinese Remainder Theorem  
**Computational Complexity**: P vs NP, reduction proofs
**Probability Theory**: Primality testing, random number generation

## Exercises and Problems

### Basic Theory
1. Prove that if gcd(a,n) = 1, then a^φ(n) ≡ 1 (mod n)
2. Show that φ(pq) = (p-1)(q-1) for distinct primes p, q
3. Verify RSA correctness for small examples (p=7, q=11, e=3)

### Advanced Problems  
4. Prove the Chinese Remainder Theorem
5. Analyze the complexity of the Extended Euclidean Algorithm
6. Show why Wiener's attack works when d < n^(1/4)/3

### Research Questions
7. What is the exact complexity of the General Number Field Sieve?
8. How do elliptic curve methods compare to GNFS for factoring?
9. What post-quantum alternatives preserve RSA's mathematical elegance?

## Historical Context

**1977**: Rivest, Shamir, and Adleman publish "A Method for Obtaining Digital Signatures and Public-Key Cryptosystems"

**Key Insight**: The first practical realization that trapdoor one-way functions could enable public-key cryptography.

**Mathematical Prerequisites**: Their work built on:
- Fermat's Little Theorem (1640)
- Euler's theorem and totient function (1760)  
- Extended Euclidean Algorithm (ancient)
- Modern computational number theory (1960s-70s)

## References and Further Reading

**Foundational Papers**:
- Rivest, Shamir, Adleman (1978): "A method for obtaining digital signatures and public-key cryptosystems"
- Wiener (1990): "Cryptanalysis of short RSA secret exponents"

**Mathematical Background**:
- Hardy & Wright: "An Introduction to the Theory of Numbers"
- Koblitz: "A Course in Number Theory and Cryptography"  
- Shoup: "A Computational Introduction to Number Theory and Algebra"

**Modern Cryptanalysis**:
- Boneh: "Twenty years of attacks on the RSA cryptosystem"
- Survey papers on integer factorization algorithms

## License

MIT License - This mathematical exposition is freely available for educational use.

---

*Mathematics is the foundation of cryptography. Understanding the theory makes you a better practitioner.*

**Questions? Issues? Ideas?** Open an issue or start a discussion! 

**Found this helpful?** Give it a ⭐ to help others discover it!

Made with ❤️ for the curious minds who want to understand the crypto that powers our digital world.
