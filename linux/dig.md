
# 🧪 **Übung: DNS-Abfragen mit `dig` (Beispiele mit google.com)**

## 📌 **1. Installation von `dig` (dnsutils)**

```bash
sudo apt update
sudo apt install -y dnsutils
```

---

## 📘 **2. Die wichtigsten DNS-Record-Typen**

* **A** → IPv4-Adresse eines Hosts
* **AAAA** → IPv6-Adresse
* **CNAME** → Alias-Eintrag
* **MX** → Mailserver
* **NS** → autoritative Nameserver
* **TXT** → Textinformationen (SPF, Google-Site-Verification usw.)
* **SOA** → Start of Authority (Zonen-Infos)
* **PTR** → Reverse DNS Lookup (IP → Name)

---

## 🛠️ **3. Praktische Abfragen mit `dig`**

### 👉 **A-Record**

```bash
dig A google.com
```

### 👉 **AAAA-Record (IPv6)**

```bash
dig AAAA google.com
```

### 👉 **MX-Record (Mailserver)**

```bash
dig MX google.com
```

### 👉 **CNAME-Record für www**

```bash
dig CNAME www.google.com
```

### 👉 **Nameserver der Domain**

```bash
dig NS google.com
```

### 👉 **TXT-Records (SPF, Google-Verifikation, etc.)**

```bash
dig TXT google.com
```

### 👉 **SOA-Record**

```bash
dig SOA google.com
```

---

## 🔍 **4. Reverse DNS Lookup (PTR) für Google DNS**

```bash
dig -x 8.8.8.8
```

---

## 📄 **5. Vollständige, gut lesbare Antwort**

```bash
dig google.com any +multiline +answer
```

