
### ✅ **Clash.Meta Supported Rule Types**

* DOMAIN
* DOMAIN-SUFFIX
* DOMAIN-KEYWORD
* DOMAIN-WILDCARD
* DOMAIN-REGEX
* GEOIP / GEOIP6
* IP-ASN
* IP-CIDR / IP-CIDR6
* SRC-IP-CIDR / SRC-IP-CIDR6
* DST-PORT
* SRC-PORT
* PROCESS-NAME
* PROCESS-PATH
* NETWORK (TCP/UDP)
* PROTOCOL (HTTP / TLS / QUIC …)
* AND / OR / NOT (logical rules)
* SUB-RULE
* RULE-SET
* SCRIPT
* MATCH

---

### ✅ **Clash.Meta Supported Inbounds**

* SOCKS5
* HTTP
* Mixed (SOCKS + HTTP)
* Redir
* TProxy
* Shadowsocks (2022 / AEAD ciphers)
* Vmess
* Tuic
* Tunnel
* TUN

---

### ❌ **Not Supported by Clash.Meta (things Clash.Meta doesn’t do natively)**

* Trojan inbound (only outbound)
* VLESS inbound (only outbound)
* gRPC inbound (only outbound, via VMess/VLESS over gRPC)
* Hysteria inbound (only outbound)
* WireGuard (no direct inbound)
* AnyVPN protocols (OpenVPN, L2TP, PPTP, IPSec, etc.)
* Non-proxy listeners (FTP, SMTP, etc.)

---

