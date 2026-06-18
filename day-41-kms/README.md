---

# 📅 Day 41: Securing Data with AWS KMS

---

## 🧠 Task

* Create KMS key → `datacenter-KMS-Key`
* Encrypt file → `/root/SensitiveData.txt`
* Encode encrypted data → base64
* Save output → `/root/EncryptedData.bin`
* Decrypt and verify data integrity

---

## 🎯 Objective

* Understand AWS KMS (Key Management Service)
* Perform encryption and decryption using KMS
* Handle sensitive data securely
* Validate encryption workflow

---

## ☁️ AWS Details

* Service: AWS KMS
* Region: us-east-1
* KMS Key: `datacenter-KMS-Key`
* File: `/root/SensitiveData.txt`

---

# 🚀 PART 1: Create KMS Key

---

## 🔹 Step 1: Create Key (Console)

1. Go to **KMS → Customer Managed Keys → Create key**
2. Key type → **Symmetric**
3. Key usage → **Encrypt and decrypt**
4. Alias → `datacenter-KMS-Key`
5. Keep default permissions
6. Click **Create key**

---

## 🔹 Step 2: Note Key ID / ARN

👉 Required for CLI operations

---

# 🟢 PART 2: Encrypt File

---

## 🔹 Step 3: Encrypt Data using CLI

```bash id="kms1"
aws kms encrypt \
--key-id alias/datacenter-KMS-Key \
--plaintext fileb:///root/SensitiveData.txt \
--output text \
--query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
```

---

## 🔹 Step 4: Verify File Created

```bash id="kms2"
ls -l /root/EncryptedData.bin
```

---

# 🟡 PART 3: Decrypt File

---

## 🔹 Step 5: Decrypt Data

```bash id="kms3"
aws kms decrypt \
--ciphertext-blob fileb:///root/EncryptedData.bin \
--output text \
--query Plaintext | base64 --decode > /root/DecryptedData.txt
```

---

## 🔹 Step 6: Verify Decryption

```bash id="kms4"
cat /root/DecryptedData.txt
```

---

# 🔍 PART 4: Validate Integrity

---

## 🔹 Step 7: Compare Files

```bash id="kms5"
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

👉 Expected:

```text id="kms6"
(no output → files match)
```

---

# 💡 Key Learning

* AWS KMS manages encryption keys securely
* Symmetric keys are used for encryption/decryption
* Data must be base64 encoded/decoded properly
* Encryption ensures data confidentiality

---

# ⚠️ Challenges Faced

* Confusion with base64 encoding/decoding
* Using correct file path format (`fileb://`)
* Identifying correct KMS key alias

---

# 🔧 Fix / Learning

* Used proper CLI syntax for binary data
* Verified key alias before encryption
* Ensured encoding/decoding steps are correct

---

# 🧩 Summary

Successfully created KMS key `datacenter-KMS-Key`, encrypted sensitive file `/root/SensitiveData.txt`, stored encrypted output as `/root/EncryptedData.bin`, and verified decryption integrity.

---
