# gentlespool

Feed it domains and it will use posttls-finger to generate a tls_policy suitable
for postfix.
<br>Usable when doing enforced TLS to multiple organizations.

gentlespool will try to detect anomalies, such as when a domain has a MX pointing to a hostname, which is not matchable by the peer name, either in CommonName or subjectAltName in the certificate. If this is detected, gentlespool will try to build a sane match= directive and set it to the extracted subjectAltName and CommonName. It will also switch from verified TLS mode to oppurtunistic TLS, by changing "verify" to "may".

## Syntax
```$ gentlespool domain1.cc domain2.cc```
<br>
```$ gentlespool --file file-with-domains.txt```

## Example
```$ gentlespool xpd.se kirei.se romab.com```
```
#
# xpd.se
#
# using DANE RR: _25._tcp.smtp.xpd.se -> 3.1.1._dane.xpd.se IN TLSA 3 1 1 E4:98:52:C1:27:CA:C7:5A:54:74:9C:57:6A:16:B3:6C:56:A5:5D:1D:48:F6:1C:35:68:8A:F1:D7:24:A2:88:C2
# smtp.xpd.se[2001:16d8:c016::200]:25: depth=0 matched end entity public-key sha256 digest=E4:98:52:C1:27:CA:C7:5A:54:74:9C:57:6A:16:B3:6C:56:A5:5D:1D:48:F6:1C:35:68:8A:F1:D7:24:A2:88:C2
# smtp.xpd.se[2001:16d8:c016::200]:25: Matched subjectAltName: *.xpd.se
# smtp.xpd.se[2001:16d8:c016::200]:25: Matched subjectAltName: xpd.se
# smtp.xpd.se[2001:16d8:c016::200]:25 CommonName *.xpd.se
# smtp.xpd.se[2001:16d8:c016::200]:25: subject_CN=*.xpd.se, issuer_CN=Go Daddy Secure Certificate Authority - G2, fingerprint=BB:C9:6E:02:2A:02:2A:0C:29:AE:1D:3D:96:66:69:56:01:9C:39:49, pkey_fingerprint=A9:2D:D7:B1:75:42:50:C6:E9:05:AE:7C:38:5E:C0:7E:5F:B1:2D:FE
# Verified TLS connection established to smtp.xpd.se[2001:16d8:c016::200]:25: TLSv1.2 with cipher ECDHE-RSA-AES256-GCM-SHA384 (256/256 bits)
xpd.se			verify protocols=TLSv1 ciphers=high

#
# kirei.se
#
# using DANE RR: _25._tcp.spg.kirei.se IN TLSA 3 1 1 7C:CC:81:42:B6:7F:AB:0F:21:8C:C2:EC:FB:1C:83:F0:74:00:11:DE:CC:2C:F9:20:DE:44:B2:8F:F7:C3:9F:B8
# spg.kirei.se[2001:67c:394:15::9]:25: depth=0 matched end entity public-key sha256 digest=7C:CC:81:42:B6:7F:AB:0F:21:8C:C2:EC:FB:1C:83:F0:74:00:11:DE:CC:2C:F9:20:DE:44:B2:8F:F7:C3:9F:B8
# spg.kirei.se[2001:67c:394:15::9]:25: Matched subjectAltName: spg.kirei.se
# spg.kirei.se[2001:67c:394:15::9]:25 CommonName spg.kirei.se
# spg.kirei.se[2001:67c:394:15::9]:25: subject_CN=spg.kirei.se, issuer_CN=CAcert Class 3 Root, fingerprint=0B:4B:44:13:E8:9F:56:E9:AA:AD:E9:D1:11:91:A3:38:B0:90:1E:DF, pkey_fingerprint=99:86:7E:47:3E:55:4D:CD:0F:DB:56:6F:72:B8:75:C0:0B:C5:A5:1D
# Verified TLS connection established to spg.kirei.se[2001:67c:394:15::9]:25: TLSv1.2 with cipher ECDHE-RSA-AES256-GCM-SHA384 (256/256 bits)
kirei.se			verify protocols=TLSv1 ciphers=high

#
# romab.com
#
# rot13.romab.com[192.195.142.6]:25: Matched subjectAltName: rot13.romab.com
# rot13.romab.com[192.195.142.6]:25: subjectAltName: www.rot13.romab.com
# rot13.romab.com[192.195.142.6]:25 CommonName rot13.romab.com
# rot13.romab.com[192.195.142.6]:25: subject_CN=rot13.romab.com, issuer_CN=Go Daddy Secure Certification Authority, fingerprint=98:E0:D2:50:33:B2:FA:18:F1:A7:FC:4C:0B:10:66:96:BD:DA:AD:D3, pkey_fingerprint=78:0C:85:F0:5A:E1:F5:31:54:8E:BB:4B:59:17:E2:D4:C1:FA:38:C4
# Verified TLS connection established to rot13.romab.com[192.195.142.6]:25: TLSv1.2 with cipher ECDHE-RSA-AES256-GCM-SHA384 (256/256 bits)
romab.com			verify protocols=TLSv1 ciphers=high
```
