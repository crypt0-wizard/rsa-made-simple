# RSA Made Simple

> Everything you need to understand, implement, and master RSA encryption - from math to code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](CONTRIBUTING.md)

## Why This Exists

I got tired of RSA explanations that either dumbed things down to uselessness or assumed you had a PhD in mathematics. This repo sits somewhere in the middle - it's for people who want to actually understand how RSA works, not just use a library that does magic behind the scenes.

Whether you're cramming for a cryptography exam, building something that needs custom RSA implementation, or just genuinely curious about how your HTTPS connection stays secure, this should help.

## What's Inside

**Mathematical Foundation**
- Prime number generation that actually makes sense
- Modular arithmetic with real examples you can follow
- Euler's totient function (without the academic jargon)
- Extended Euclidean algorithm walkthrough

**Working Code**
- Pure Python implementations with extensive comments
- Complete key generation process
- Encryption and decryption that you can trace step-by-step
- Digital signature examples

**Practical Tools**
- Command-line utilities for key generation
- Message encryption/decryption tools
- Performance testing across different key sizes
- Security analysis helpers

**Learning Materials**
- Interactive Jupyter notebooks
- Diagrams that actually clarify instead of confuse
- Common attack explanations
- Real-world limitations and when NOT to use RSA

## Getting Started

```bash
# Grab the code
git clone https://github.com/yourusername/rsa-made-simple.git
cd rsa-made-simple

# Install the few dependencies we need
pip install -r requirements.txt

# Create your first RSA key pair
python src/keygen.py --bits 1024

# Encrypt something
python src/encrypt.py "Secret message" --public-key keys/public.pem

# Decrypt it
python src/decrypt.py encrypted_message.txt --private-key keys/private.pem
```

## How Everything is Organized

```
rsa-made-simple/
├── src/
│   ├── keygen.py          # Creates RSA key pairs
│   ├── encrypt.py         # Handles message encryption
│   ├── decrypt.py         # Handles message decryption
│   ├── rsa_core.py        # The actual RSA math
│   └── utils.py           # Helper functions
├── notebooks/
│   ├── 01-math-basics.ipynb      # Mathematical groundwork
│   ├── 02-building-keys.ipynb    # Key generation process
│   ├── 03-encryption-flow.ipynb  # How encryption works
│   └── 04-security-stuff.ipynb   # Attacks and defenses
├── tests/
│   ├── test_keygen.py
│   ├── test_encryption.py
│   └── test_security.py
├── examples/
│   ├── basic_demo.py
│   ├── performance_check.py
│   └── digital_signatures.py
└── docs/
    ├── math-walkthrough.md
    ├── security-guide.md
    └── common-questions.md
```

## RSA in Plain English

Here's the basic idea without getting lost in notation:

1. Pick two large prime numbers (call them p and q)
2. Multiply them together to get n
3. Calculate phi(n) = (p-1) * (q-1)
4. Pick a number e (usually 65537) that doesn't share factors with phi(n)
5. Find d such that e * d leaves remainder 1 when divided by phi(n)
6. Your public key is (n, e), private key is (n, d)
7. To encrypt: raise message to power e, mod n
8. To decrypt: raise ciphertext to power d, mod n

The math works because of some clever number theory. Check out the notebooks for the full story.

## Key Features

**Educational Focus**: Every implementation prioritizes understanding over performance. You can follow the logic from start to finish.

**No Magic Libraries**: Uses pure Python so you see exactly what's happening. No mysterious crypto functions that you have to trust.

**Security Awareness**: Covers what makes RSA vulnerable and why production systems need additional protections.

**Interactive Learning**: Jupyter notebooks let you experiment with parameters and see how changes affect security and performance.

**Practical Examples**: Includes real scenarios where you might actually use these techniques.

## Learning Path

If you're new to this stuff, try this order:

1. **Math Basics** (`notebooks/01-math-basics.ipynb`) - Get comfortable with the underlying mathematics
2. **Key Generation** (`notebooks/02-building-keys.ipynb`) - Learn how RSA keys are created
3. **Encryption Process** (`notebooks/03-encryption-flow.ipynb`) - Understand the actual encryption/decryption
4. **Security Considerations** (`notebooks/04-security-stuff.ipynb`) - Learn what can go wrong
5. **Examples** - Run through the practical examples
6. **Build Something** - Use the core functions to create your own tools

## Important Security Note

This is educational code. I've prioritized clarity over security, which means it's missing several things you'd need for production:

- Timing attack protections
- Cryptographically secure random number generation
- Proper padding schemes (OAEP, PKCS#1)
- Side-channel attack mitigations

Use established libraries like `cryptography` or `pycryptodome` for anything real. This repo is for understanding how those libraries work under the hood.

## Contributing

Found a bug? Have a clearer way to explain something? Want to add an example? Pull requests welcome. 

Before contributing:
- Make sure examples still work
- Add tests for new functionality  
- Keep explanations accessible
- Update documentation

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Why RSA Still Matters

Even though elliptic curve cryptography is becoming more popular, RSA remains fundamental to understanding public-key cryptography. It's also still widely used in TLS, code signing, and many other applications. Learning RSA gives you the foundation to understand more advanced cryptographic concepts.

## License 📄

MIT License - see [LICENSE](LICENSE) file for details.

## Acknowledgments 🙏

- Rivest, Shamir, and Adleman for inventing RSA
- The open-source cryptography community
- Everyone who's contributed to making crypto education accessible

---

**Questions? Issues? Ideas?** Open an issue or start a discussion! 

**Found this helpful?** Give it a ⭐ to help others discover it!

Made with ❤️ for the curious minds who want to understand the crypto that powers our digital world.
