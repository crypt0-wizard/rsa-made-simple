# RSA Made Simple 🔐

> Everything you need to understand, implement, and master RSA encryption - from math to code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](CONTRIBUTING.md)

## What's This All About? 🤔

RSA encryption doesn't have to be mysterious! This repo breaks down one of the most important cryptographic algorithms in history into digestible, implementable pieces. Whether you're a student trying to wrap your head around public-key cryptography, a developer wanting to implement RSA from scratch, or just curious about how secure communication works under the hood - we've got you covered.

## What You'll Find Here 📚

### 🧮 **The Math Made Easy**
- Prime number generation and testing
- Modular arithmetic explained with examples  
- Euler's totient function (φ) demystified
- Extended Euclidean algorithm walkthrough

### 💻 **Clean, Commented Code**
- Pure Python implementations (no external crypto libs!)
- Step-by-step key generation
- Encryption and decryption functions
- Digital signature examples

### 🛠️ **Practical Tools**
- RSA key pair generator
- Message encryption/decryption utility
- Performance benchmarking scripts
- Security analysis tools

### 📖 **Learning Resources**
- Interactive Jupyter notebooks
- Visual explanations with diagrams
- Common attack vectors and defenses
- Real-world use cases and limitations

## Quick Start 🚀

```bash
# Clone the repo
git clone https://github.com/yourusername/rsa-made-simple.git
cd rsa-made-simple

# Install dependencies (minimal!)
pip install -r requirements.txt

# Generate your first RSA key pair
python src/keygen.py --bits 1024

# Encrypt a message
python src/encrypt.py "Hello, RSA!" --public-key keys/public.pem

# Decrypt it back
python src/decrypt.py encrypted_message.txt --private-key keys/private.pem
```

## Repository Structure 🗂️

```
rsa-made-simple/
├── 📁 src/
│   ├── keygen.py          # RSA key pair generation
│   ├── encrypt.py         # Message encryption
│   ├── decrypt.py         # Message decryption
│   ├── rsa_core.py        # Core RSA algorithms
│   └── utils.py           # Helper functions
├── 📁 notebooks/
│   ├── 01-math-primer.ipynb     # Mathematical foundations
│   ├── 02-key-generation.ipynb  # Building RSA keys
│   ├── 03-encryption.ipynb      # Encryption process
│   └── 04-attacks-defense.ipynb # Security considerations
├── 📁 tests/
│   ├── test_keygen.py
│   ├── test_encryption.py
│   └── test_security.py
├── 📁 examples/
│   ├── basic_usage.py
│   ├── performance_test.py
│   └── digital_signatures.py
└── 📁 docs/
    ├── math-explained.md
    ├── security-best-practices.md
    └── faq.md
```

## How RSA Works (The 30-Second Version) ⚡

1. **Pick two big prime numbers** (p and q)
2. **Multiply them** to get n = p × q
3. **Calculate φ(n)** = (p-1) × (q-1)
4. **Choose e** (usually 65537) where gcd(e, φ(n)) = 1
5. **Find d** where e × d ≡ 1 (mod φ(n))
6. **Public key**: (n, e) | **Private key**: (n, d)
7. **Encrypt**: c = m^e mod n
8. **Decrypt**: m = c^d mod n

*Want the full story? Check out our [detailed explanation](docs/math-explained.md)!*

## Key Features ✨

- **🎓 Educational First**: Every line of code is commented and explained
- **🔧 Production Ready**: Includes proper error handling and security checks  
- **📊 Benchmarking**: Performance testing across different key sizes
- **🛡️ Security Focused**: Covers common vulnerabilities and mitigations
- **🐍 Pure Python**: No black-box crypto libraries - see exactly how it works
- **📱 Interactive**: Jupyter notebooks for hands-on learning

## Learning Path 🎯

New to RSA? Follow this suggested order:

1. Start with `notebooks/01-math-primer.ipynb` for the mathematical foundation
2. Work through `notebooks/02-key-generation.ipynb` to understand key creation
3. Explore `notebooks/03-encryption.ipynb` for the encryption/decryption process
4. Review `notebooks/04-attacks-defense.ipynb` for security considerations
5. Try the examples in `examples/` directory
6. Build your own implementations using our core functions

## Security Warning ⚠️

**This is an educational implementation!** While the algorithms are correct, this code lacks many security features required for production use:

- No protection against timing attacks
- No secure random number generation
- No padding schemes (OAEP, PKCS#1)
- Limited prime generation methods

For real applications, use established libraries like `cryptography` or `pycryptodome`.

## Contributing 🤝

We love contributions! Whether it's:

- 🐛 Bug fixes
- 📚 Documentation improvements
- ✨ New examples or tutorials
- 🔧 Performance optimizations
- 💡 Educational content

Check out our [Contributing Guide](CONTRIBUTING.md) to get started.

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
