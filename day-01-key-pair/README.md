# 📅 Day 1: Create Key Pair (AWS)

---

## 🎯 Task

Create an AWS key pair with:

* **Name:** `datacenter-kp`
* **Type:** `RSA`
* **Region:** `us-east-1`

---

## 🧠 What is a Key Pair?

A key pair is used to securely connect to EC2 instances.

* Public key → Stored in AWS
* Private key → Downloaded (`.pem` file)

---

## 🚀 Steps

1. Login to AWS Console
2. Select region → **us-east-1**
3. Go to **EC2 Dashboard**
4. Click **Key Pairs → Create Key Pair**
5. Enter:

   * Name: `datacenter-kp`
   * Type: `RSA`
6. Click **Create** and download the `.pem` file

---
# RSA vs ED25519 SSH Keys

| Feature | RSA | ED25519 |
|----------|----------|----------|
| Security | Good (with 3072/4096 bits) | Very strong |
| Key Size | Large | Small |
| Performance | Slower | Faster |
| Key Generation | Slower | Faster |
| Public Key Length | Longer | Shorter |
| Compatibility | Supported everywhere | Supported on modern systems |
| Recommended Today | Yes (legacy compatibility) | ✅ Preferred |

## RSA

### Generate RSA Key

```bash
ssh-keygen -t rsa -b 4096
```

### Generated Files

```text
id_rsa
id_rsa.pub
```

### Advantages

- Supported by almost all systems
- Good compatibility with older servers and applications
- Widely used in enterprise environments

### Disadvantages

- Larger key size
- Slower authentication and signing
- Requires 3072 or 4096-bit keys for strong security

---

## ED25519

### Generate ED25519 Key

```bash
ssh-keygen -t ed25519
```

### Generated Files

```text
id_ed25519
id_ed25519.pub
```

### Advantages

- Smaller key size
- Faster authentication
- Strong modern cryptography
- Better security-to-key-size ratio
- Faster key generation

### Disadvantages

- May not be supported on very old systems
- Some legacy devices and SSH clients may not support it

---

## AWS EC2 Recommendation

### Use ED25519 When

- Deploying modern Linux servers
- Using recent OpenSSH versions
- Security and performance are priorities

### Use RSA When

- Working with legacy systems
- Compatibility is required
- Older SSH clients or devices are involved

---

## Interview Answer

**RSA** is based on integer factorization and typically uses 2048, 3072, or 4096-bit keys. It offers broad compatibility but requires larger key sizes and has slower performance.

**ED25519** is based on elliptic curve cryptography and provides stronger security with smaller keys, faster authentication, and better performance. It is the recommended SSH key algorithm for modern environments.

## ⚠️ Important

* Use **us-east-1 region only**
* Name must be exactly **datacenter-kp**
* Select **RSA**, not ED25519

---

## ❌ Common Mistakes

* Wrong region
* Incorrect key name
* Wrong key type

---

## 💡 What I Learned Today

### What a Key Pair is and why it's used 🔑

A key pair is a secure authentication method used to connect to EC2 instances without passwords. It ensures safe access by using cryptographic keys.

### Difference between public key and private key

* **Public key:** Stored in AWS and attached to EC2 instances
* **Private key (.pem):** Downloaded by the user and used to securely connect to the instance

### Importance of regions in AWS

AWS resources are region-specific. Selecting the correct region (like `us-east-1`) ensures resources are created in the expected location and avoids configuration issues.

### Basics of EC2 security and access

EC2 instances are accessed securely using key pairs and controlled using security groups, ensuring only authorized users can connect.

---

## 🛠️ What I Built / Practiced

* Logged into AWS Console
* Navigated to EC2 dashboard
* Created a key pair → `datacenter-kp` (RSA)
* Downloaded and securely stored the `.pem` file

---

## ⚠️ Challenges

* Initially ignored the region selection ⚠️
* Confused between RSA and ED25519 key types
---

## 🔧 Fix / Learning

* Understood that region matters for resource creation
* Learned RSA is widely used and expected in most labs

---

## 🧩 Key Takeaway

Even a simple step like creating a key pair is crucial for secure cloud access.

---

## 🧩 Summary

```bash
Create an RSA key pair named datacenter-kp in us-east-1 using EC2
```

---

## ✅ Outcome

Key pair successfully created and ready for EC2 usage 🚀
