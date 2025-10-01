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
### ✅ **Clash.Meta Supports**
 * sub-rules:
 * listeners:
 * proxy-groups:
---
### ❌ **Not Supported by Clash.Meta (things Clash.Meta doesn’t do natively)**
* Trojan inbound (only outbound)
* VLESS inbound (only outbound)
* gRPC inbound (only outbound, via VMess/VLESS over gRPC)
* Hysteria inbound (only outbound)
* WireGuard (no direct inbound)
* AnyVPN protocols (OpenVPN, L2TP, PPTP, IPSec, etc.)
* Non-proxy listeners (FTP, SMTP, etc.)
* domainStrategy
* outboundTag
* targetStrategy
* inbounds
* streamSettings
* tag
* inboundTag
---
https://github.com/djoeni/Clash.Meta
---
### 🔧 Routing Rule Types
Mihomo supports a variety of rule types to match different traffic characteristics:
* **DOMAIN**: Matches exact domain names.
* **DOMAIN-SUFFIX**: Matches domain suffixes (e.g., `.com`).
* **DOMAIN-KEYWORD**: Matches domains containing specific keywords.
* **DOMAIN-WILDCARD**: Matches domains using wildcard patterns.
* **DOMAIN-REGEX**: Matches domains using regular expressions.
* **GEOSITE**: Matches based on predefined geographic site lists.
* **IP-CIDR**: Matches IP address ranges (IPv4).
* **IP-CIDR6**: Matches IP address ranges (IPv6).
* **IP-SUFFIX**: Matches IP suffixes.
* **IP-ASN**: Matches IP addresses belonging to a specific ASN.
* **GEOIP**: Matches based on the country code of the IP address.
* **SRC-GEOIP**: Matches based on the source IP's country code.
* **SRC-IP-ASN**: Matches based on the source IP's ASN.
* **SRC-IP-CIDR**: Matches based on the source IP's CIDR range.
* **SRC-IP-SUFFIX**: Matches based on the source IP's suffix.
* **DST-PORT**: Matches destination ports.
* **SRC-PORT**: Matches source ports.
* **IN-PORT**: Matches inbound ports.
* **IN-TYPE**: Matches inbound connection types (e.g., SOCKS, HTTP).
* **IN-USER**: Matches inbound usernames.
* **IN-NAME**: Matches inbound connection names.
* **PROCESS-PATH**: Matches the full path of the process.
* **PROCESS-PATH-REGEX**: Matches the process path using regular expressions.
* **PROCESS-NAME**: Matches the process name.
* **PROCESS-NAME-REGEX**: Matches the process name using regular expressions.
* **UID**: Matches user IDs.
* **NETWORK**: Matches network protocols.
* **DSCP**: Matches Differentiated Services Code Point values.
* **RULE-SET**: Matches rules from an external rule set.
* **AND**, **OR**, **NOT**: Logical operators to combine multiple conditions.
* **SUB-RULE**: Matches sub-rules defined elsewhere.
* **MATCH**: Matches a specific condition.
---
### ⚙️ Additional Parameters
* **no-resolve**: Prevents domain name resolution.
* **src**: Specifies the source of the rule.
---
### 🧩 Example Rule Set
```yaml
rules:
  - DOMAIN,example.com,REJECT
  - DOMAIN-SUFFIX,example.org,PROXY
  - IP-CIDR,192.168.1.0/24,DIRECT
  - GEOIP,US,PROXY
  - IN-PORT,8080,PROXY
  - PROCESS-NAME,chrome.exe,PROXY
  - RULE-SET,custom-rules,PROXY
  - AND,((DOMAIN,example.net),(SRC-PORT,443)),PROXY
```
In this example:
* Traffic to `example.com` is rejected.
* Traffic to domains ending with `.org` is routed through a proxy.
* Traffic from the IP range `192.168.1.0/24` is directed without a proxy.
* Traffic from the US is routed through a proxy.
* Inbound traffic on port 8080 is routed through a proxy.
* Traffic from the `chrome.exe` process is routed through a proxy.
* Rules from an external rule set named `custom-rules` are applied.
* Traffic to `example.net` on port 443 is routed through a proxy.
---
For more detailed information and advanced configurations, you can refer to the official documentation: [Mihomo Routing Rules](https://wiki.metacubex.one/en/config/rules/).
---
The [Mihomo proxy configuration documentation](https://wiki.metacubex.one/config/proxies/) provides detailed information on configuring outbound proxy nodes in Mihomo. Here's an overview of the key fields and options available:
---
### 🔧 General Proxy Fields
* **name**: A unique identifier for the proxy node.
* **type**: The type of proxy, such as `ss` (Shadowsocks), `vmess`, `trojan`, etc.
* **server**: The proxy server's domain or IP address.
* **port**: The port number for the proxy server.
* **ip-version**: Specifies the IP version to use (`ipv4`, `ipv6`, `dual`, `ipv4-prefer`, `ipv6-prefer`).
* **udp**: Indicates whether UDP traffic is allowed through the proxy (`true` or `false`).
* **interface-name**: Specifies the network interface for the proxy connection.
* **routing-mark**: Assigns a routing mark to the proxy connection.
* **tfo**: Enables TCP Fast Open for the connection.
* **mptcp**: Enables MultiPath TCP for the connection.
* **dialer-proxy**: Specifies a proxy to use for establishing the connection.
* **smux**: Configures multiplexing settings for the proxy connection.
---
### 📦 SMUX Configuration
For proxies that support multiplexing, the `smux` field allows you to configure:
* **enabled**: Whether multiplexing is enabled (`true` or `false`).
* **protocol**: The multiplexing protocol to use (`smux`, `yamux`, `h2mux`).
* **max-connections**: The maximum number of connections.
* **min-streams**: The minimum number of streams per connection.
* **max-streams**: The maximum number of streams per connection.
* **statistic**: Whether to display multiplexing statistics.
* **only-tcp**: Whether to apply multiplexing only to TCP connections.
* **padding**: Whether to enable padding for the connection.
* **brutal-opts**: TCP Brutal settings, including upload and download bandwidth limits.
---
### 🧩 Example Proxy Configuration
```yaml
proxies:
  - name: "ss"
    type: ss
    server: server
    port: 443
    ip-version: ipv4
    udp: true
    interface-name: eth0
    routing-mark: 1234
    tfo: false
    mptcp: false
    dialer-proxy: ss1
    smux:
      enabled: true
      protocol: smux
      max-connections: 4
      min-streams: 4
      max-streams: 0
      statistic: false
      only-tcp: false
      padding: true
      brutal-opts:
        enabled: true
        up: 50
        down: 100
```
In this example, a Shadowsocks proxy named `ss` is configured with various settings, including multiplexing and TCP Fast Open options.
---
For more detailed information and advanced configurations, you can refer to the official documentation: [Mihomo Proxy Configuration](https://wiki.metacubex.one/config/proxies/).
---
The [Mihomo inbound listeners configuration](https://wiki.metacubex.one/config/inbound/listeners/) defines how Mihomo accepts incoming connections on various protocols, such as HTTP, Shadowsocks, and TUIC. Each listener specifies the protocol type, listening address, port, and optional settings like user authentication and encryption.
---
### 🔧 Common Listener Fields
* **name**: Unique identifier for the listener.
* **type**: Protocol type (e.g., `http`, `shadowsocks`, `tuic`).
* **listen**: Address to bind to (e.g., `0.0.0.0` for all interfaces).
* **port**: Port number or range to listen on.
* **users**: List of user credentials for authentication.
* **certificate**: Path to the TLS certificate file.
* **private-key**: Path to the TLS private key file.
* **rule**: Optional sub-rule for routing traffic.
* **proxy**: Optional outbound proxy or strategy group.
---
### 📡 Listener Types and Examples
#### 1. HTTP Listener
```yaml
listeners:
  - name: http-in
    type: http
    port: 7890
    listen: 0.0.0.0
    users:
      - username: user1
        password: pass1
    certificate: ./server.crt
    private-key: ./server.key
```
This listener accepts HTTP connections on port 7890, requires user authentication, and uses TLS encryption. ([wiki.metacubex.one][1])
#### 2. Shadowsocks Listener
```yaml
listeners:
  - name: ss-in
    type: shadowsocks
    port: 10001
    listen: 0.0.0.0
    cipher: aes-256-gcm
    password: password123
    udp: true
```
This listener accepts Shadowsocks connections on port 10001, supports UDP, and uses AES-256-GCM encryption. ([wiki.metacubex.one][2])
#### 3. TUIC v5 Listener
```yaml
listeners:
  - name: tuicv5-in
    type: tuic
    port: 10004
    listen: 0.0.0.0
    users:
      UUID1: PASSWORD1
      UUID2: PASSWORD2
    certificate: ./server.crt
    private-key: ./server.key
    alpn:
      - h3
```
This listener accepts TUIC v5 connections on port 10004, supports HTTP/3 (ALPN `h3`), and uses user-based authentication. ([wiki.metacubex.one][3])
---
### 🔐 User Authentication
User authentication can be configured globally or per listener. If the `users` field is specified in a listener, it overrides the global settings. To disable authentication for a listener, set `users: []`. ([wiki.metacubex.one][4])
---
For more detailed information and advanced configurations, please refer to the official documentation: [Mihomo Inbound Listeners](https://wiki.metacubex.one/config/inbound/listeners/).
[1]: https://wiki.metacubex.one/en/config/inbound/listeners/http/?utm_source=chatgpt.com "http - mihomo docs"
[2]: https://wiki.metacubex.one/en/config/inbound/listeners/ss/?utm_source=chatgpt.com "ShadowSocks - mihomo docs"
[3]: https://wiki.metacubex.one/en/config/inbound/listeners/tuic-v5/?utm_source=chatgpt.com "TUIC v5 - mihomo docs"
[4]: https://wiki.metacubex.one/en/config/general/?utm_source=chatgpt.com "General configuration - mihomo docs"
---
**text-based diagram** that shows how Mihomo handles listeners, users, and routing flow:
```
                     ┌─────────────────┐
                     │    CLIENTS      │
                     │ (Browsers/Apps) │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │   LISTENER      │
                     │ (Protocol/Port) │
                     └────────┬────────┘
                              │
                  ┌───────────┴───────────┐
                  │ Authentication Check │
                  │   (users / credentials) │
                  └───────────┬───────────┘
                              │
           ┌───────────────Allowed───────────────┐
           │                                      │
           ▼                                      ▼
    ┌─────────────┐                        ┌─────────────┐
    │ ROUTING /   │                        │   DENY /    │
    │ RULES       │                        │ REJECT      │
    │ (Domain/IP/ │                        │ CONNECTION  │
    │ GEOIP/etc.) │                        └─────────────┘
    └─────┬───────┘
          │
  ┌───────┴────────┐
  │   OUTBOUND     │
  │   PROXIES      │
  │ (Shadowsocks,  │
  │  V2Ray, TUIC)  │
  └───────┬────────┘
          │
          ▼
     ┌────────────┐
     │   TARGET   │
     │  INTERNET  │
     └────────────┘
```
### Explanation:
1. **Clients** connect to the listener.
2. **Listener** determines the protocol (HTTP, Shadowsocks, TUIC, etc.) and listens on a port.
3. **Authentication** checks if the client is allowed.
4. **Routing rules** decide where traffic goes:
   * Allowed traffic is forwarded to an **outbound proxy**.
   * Denied traffic is rejected.
5. **Outbound proxy** sends traffic to the internet.
---
detailed **text-based diagram** showing **multiple listeners, multiple users, and multiple outbound proxies**:
```
                       ┌───────────────────────────┐
                       │          CLIENTS          │
                       │  Browser1  Browser2 ...   │
                       └─────────────┬─────────────┘
                                     │
          ┌──────────────────────────┴─────────────────────────┐
          │                       LISTENERS                    │
          │ ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
          │ │ HTTP-LISTENER │  │ SS-LISTENER   │  │ TUIC-LIST │ │
          │ │ Port: 7890    │  │ Port: 10001   │  │ Port:10004│ │
          │ └─────┬─────────┘  └─────┬─────────┘  └─────┬─────┘ │
          └───────┼────────────────────┼───────────────┼───────┘
                  │                    │               │
        ┌─────────┴─────────┐  ┌───────┴─────────┐  ┌─┴─────────┐
        │ Authentication    │  │ Authentication  │  │ Auth      │
        │ Users: [u1,u2]    │  │ Users: [ss1]    │  │ Users:[tu1│
        └───────┬───────────┘  └───────┬─────────┘  └─────┬─────┘
                │ Allowed              │ Allowed          │ Allowed
                ▼                      ▼                  ▼
      ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
      │ ROUTING RULES   │   │ ROUTING RULES   │   │ ROUTING RULES   │
      │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │
      └─────────┬───────┘   └─────────┬───────┘   └─────────┬───────┘
                │                     │                     │
   ┌────────────┴───────────┐ ┌───────┴───────────┐ ┌───────┴───────────┐
   │ OUTBOUND PROXIES       │ │ OUTBOUND PROXIES   │ │ OUTBOUND PROXIES   │
   │ Proxy Group A          │ │ Proxy Group B      │ │ Proxy Group C      │
   │ (ss, vmess, tuic)     │ │ (ss, shadowsocks) │ │ (tuic only)       │
   └────────────┬───────────┘ └─────────┬─────────┘ └─────────┬─────────┘
                │                       │                     │
                ▼                       ▼                     ▼
           ┌───────────┐           ┌───────────┐         ┌───────────┐
           │ INTERNET  │           │ INTERNET  │         │ INTERNET  │
           └───────────┘           └───────────┘         └───────────┘
```
### Key Notes:
1. **Multiple Listeners**: Each listener handles a specific protocol and port.
2. **Authentication per Listener**: Each listener can have its own users.
3. **Routing Rules**: Traffic is filtered by domain, IP, GEOIP, or custom rules.
4. **Proxy Groups**: Different outbound proxies handle traffic based on rules. Some proxies may only support certain protocols.
5. **Internet**: Finally, traffic is forwarded to the internet.
---
This setup allows **flexible, multi-protocol traffic routing**, and you can mix and match listeners, users, and proxy groups.
---
a **text-based diagram showing an advanced setup with fallback, load balancing, and proxy chaining** in Mihomo:
```
                       ┌───────────────────────────┐
                       │          CLIENTS          │
                       │  Browser1  App1  App2 ... │
                       └─────────────┬─────────────┘
                                     │
          ┌──────────────────────────┴─────────────────────────┐
          │                       LISTENERS                    │
          │ ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
          │ │ HTTP-LISTENER │  │ SS-LISTENER   │  │ TUIC-LIST │ │
          │ │ Port: 7890    │  │ Port: 10001   │  │ Port:10004│ │
          │ └─────┬─────────┘  └─────┬─────────┘  └─────┬─────┘ │
          └───────┼────────────────────┼───────────────┼───────┘
                  │                    │               │
        ┌─────────┴─────────┐  ┌───────┴─────────┐  ┌─┴─────────┐
        │ Authentication    │  │ Authentication  │  │ Auth      │
        │ Users: [u1,u2]    │  │ Users: [ss1]    │  │ Users:[tu1│
        └───────┬───────────┘  └───────┬─────────┘  └─────┬─────┘
                │ Allowed              │ Allowed          │ Allowed
                ▼                      ▼                  ▼
      ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
      │ ROUTING RULES   │   │ ROUTING RULES   │   │ ROUTING RULES   │
      │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │
      └─────────┬───────┘   └─────────┬───────┘   └─────────┬───────┘
                │                     │                     │
      ┌─────────┴───────────┐ ┌───────┴───────────┐ ┌───────┴───────────┐
      │ PRIMARY PROXY GROUP  │ │ FALLBACK PROXY    │ │ CHAINED PROXY      │
      │ (ss, vmess, tuic)   │ │ Group: Proxy B    │ │ Group: Proxy C->D  │
      │ Load Balance Enabled │ │ Used if Primary   │ │ Sequential Chaining │
      │ Max Connections 4    │ │ fails             │ │ for encryption + routing │
      └─────────┬───────────┘ └─────────┬─────────┘ └─────────┬─────────┘
                │                       │                     │
                ▼                       ▼                     ▼
           ┌───────────┐           ┌───────────┐         ┌───────────┐
           │ INTERNET  │           │ INTERNET  │         │ INTERNET  │
           └───────────┘           └───────────┘         └───────────┘
```
### Key Concepts in this Advanced Setup:
1. **Primary Proxy Group**
   * Main outbound proxies that handle most traffic.
   * Supports **load balancing** to distribute traffic across multiple servers.
2. **Fallback Proxy Group**
   * Used automatically if the primary proxy group fails.
   * Ensures **high availability**.
3. **Chained Proxy Group**
   * Traffic goes through multiple proxies sequentially (A → B → C).
   * Useful for **enhanced privacy or multi-hop routing**.
4. **Routing Rules**
   * Traffic is matched by domain, IP, GEOIP, or custom rules before choosing a proxy.
5. **Listeners & Authentication**
   * Multiple listeners can handle different protocols.
   * Each listener can have its own **user authentication**.
---
This structure allows Mihomo to handle **complex routing scenarios**, including:
* Multi-protocol support (HTTP, Shadowsocks, TUIC)
* Automatic failover (fallback)
* Load balancing across multiple proxies
* Proxy chaining for privacy or advanced routing
---
a **comprehensive “all-in-one” text-based diagram** that shows **multiple listeners, multiple users, multiple proxy groups, load balancing, fallback, and chaining** in one view:
```
                             ┌─────────────────────────────┐
                             │          CLIENTS            │
                             │  Browser1  App1  App2 ...   │
                             └─────────────┬───────────────┘
                                           │
              ┌────────────────────────────┴─────────────────────────────┐
              │                          LISTENERS                       │
              │ ┌───────────────┐  ┌───────────────┐  ┌───────────────┐ │
              │ │ HTTP-LISTENER │  │ SS-LISTENER   │  │ TUIC-LISTENER │ │
              │ │ Port: 7890    │  │ Port: 10001   │  │ Port: 10004   │ │
              │ └─────┬─────────┘  └─────┬─────────┘  └─────┬─────────┘ │
              └───────┼────────────────────┼───────────────┼───────────┘
                      │                    │               │
        ┌─────────────┴────────────┐  ┌────┴─────────┐  ┌──┴───────────┐
        │ Authentication            │  │ Authentication│  │ Auth        │
        │ Users: [u1,u2,u3]         │  │ Users: [ss1] │  │ Users:[tu1]│
        └─────────────┬────────────┘  └────┬─────────┘  └──┬───────────┘
                      │ Allowed              │ Allowed      │ Allowed
                      ▼                      ▼              ▼
            ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
            │ ROUTING RULES   │   │ ROUTING RULES   │   │ ROUTING RULES   │
            │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │   │ Domain/IP/GEOIP │
            └─────────┬───────┘   └─────────┬───────┘   └─────────┬───────┘
                      │                     │                     │
      ┌───────────────┴─────────────┐  ┌────┴─────────────┐  ┌────┴────────────┐
      │ PRIMARY PROXY GROUP          │  │ FALLBACK PROXY    │  │ CHAINED PROXY   │
      │ Name: ProxyGroupA            │  │ Name: ProxyGroupB │  │ Name: ProxyGroupC│
      │ Protocols: ss, vmess, tuic  │  │ Protocols: ss    │  │ Sequential: D→E │
      │ Load Balance: Enabled        │  │ Auto-Fallback    │  │ Multi-hop chain │
      │ Max Connections: 4           │  │ Max Connections: 2│ │ Max Connections: 1│
      └───────────────┬─────────────┘  └────┬─────────────┘  └────┬────────────┘
                      │                     │                     │
              ┌───────┴───────┐       ┌─────┴─────┐         ┌─────┴─────┐
              │  INTERNET     │       │ INTERNET   │         │ INTERNET  │
              └───────────────┘       └───────────┘         └──────────┘
```
---
### ✅ Features Illustrated:
1. **Multiple Listeners**
   * HTTP, Shadowsocks, and TUIC protocols on separate ports.
2. **User Authentication per Listener**
   * Each listener can have its own users, enforcing access control.
3. **Routing Rules**
   * Traffic is matched based on domain, IP, or GEOIP before selecting a proxy.
4. **Primary Proxy Group**
   * Main group with multiple proxies.
   * Load balancing distributes traffic automatically.
5. **Fallback Proxy Group**
   * Used automatically if the primary group fails.
6. **Chained Proxy Group**
   * Sequential proxying (multi-hop) for privacy or advanced routing.
7. **Connection Flow to Internet**
   * Traffic passes through routing → proxy → finally reaches the internet.
---
This diagram captures **nearly all advanced Mihomo scenarios**: multiple protocols, authentication, routing, load balancing, failover, and multi-hop chaining.
---
### **What a listener is:**
A **listener** in Mihomo is basically **“the entry point for traffic coming into your proxy server.”**
Think of your computer or server as a **house**, and the **listener** is the **door**:
* **The door has a number:** The port (like 7890).
* **The door has a type:** The protocol (HTTP, Shadowsocks, TUIC).
* **The door has rules:** Only people with keys (users/passwords) can enter.
**Without a listener, your proxy server can’t receive any traffic**.
It doesn’t matter how many outbound proxies or routing rules you have—if nothing is “listening,” no client can connect.
---
### **Why you need it:**
1. **To accept traffic from clients**
   * Browsers, apps, or other devices need somewhere to send their requests.
   * That “somewhere” is the listener.
2. **To specify how traffic enters your server**
   * Protocol (HTTP, Shadowsocks, TUIC)
   * Port number
   * Authentication (optional)
3. **To link incoming traffic to routing and outbound proxies**
   * Once a listener accepts traffic, Mihomo decides **where to send it next** based on rules and proxy groups.
---
### **Super simple analogy:**

| Concept        | Real-life analogy               |
| -------------- | ------------------------------- |
| Listener       | Door to your house              |
| Port           | Door number (which door)        |
| Protocol       | Type of visitor allowed         |
| Users/password | Keys for people to enter        |
| Routing rules  | Directions inside the house     |
| Outbound proxy | Which car you use to go outside |

---
So in **one line**:
> **A listener is where your proxy server “listens” for incoming connections.**
---
### Scenario:
* You want your **computer to act as a proxy**.
* You want clients to connect via **Shadowsocks**.
* Only **one user/password** is allowed.
---
### Example `listener`:
```yaml
listeners:
  - name: ss-in
    type: shadowsocks
    listen: 0.0.0.0   # accept connections from any IP
    port: 10001        # the "door number"
    cipher: aes-256-gcm
    password: mypass123  # authentication key
    udp: true
```
---
### What this does:
1. **`name: ss-in`** → This is just a label for this listener.
2. **`type: shadowsocks`** → Only Shadowsocks clients can connect.
3. **`listen: 0.0.0.0`** → Accept traffic from any device.
4. **`port: 10001`** → Clients must connect to port **10001**.
5. **`password: mypass123`** → Only users who know this password can enter.
6. **`udp: true`** → Allows UDP traffic (needed for some apps).
---
### **Flow:**
```
Client (Shadowsocks app)
          │
          ▼
Listener ss-in (port 10001, password=mypass123)
          │
          ▼
Routing rules → Outbound proxy → Internet
```
* Without the **listener**, the client has **no way to connect**.
* With the listener, your proxy server **opens a “door”** for traffic, checks credentials, and then routes it.
---
### 🌐 **HTTP Listener**
* **Purpose**: Accepts standard HTTP(S) traffic.
* **Use Case**: Ideal for web browsers and HTTP-based clients.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: http-in
      type: http
      port: 7890
      listen: 0.0.0.0
      users:
        - username: user1
          password: pass1
      certificate: ./server.crt
      private-key: ./server.key
  ```
  *Note*: Supports ECH (Encrypted ClientHello) for enhanced privacy.
---
### 🧦 **SOCKS Listener**
* **Purpose**: Handles SOCKS5 proxy connections.
* **Use Case**: Suitable for applications like browsers or tools that support SOCKS5.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: socks-in
      type: socks
      port: 7891
      listen: 0.0.0.0
      udp: true
      users:
        - username: user1
          password: pass1
  ```
  *Note*: Can be configured with mTLS for mutual authentication.
---
### 🔀 **Mixed Listener**
* **Purpose**: Supports multiple protocols simultaneously (e.g., HTTP, SOCKS).
* **Use Case**: Useful when clients may connect using different protocols.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: mixed-in
      type: mixed
      port: 7892
      listen: 0.0.0.0
      udp: true
      users:
        - username: user1
          password: pass1
  ```
  *Note*: Allows flexibility in client connections.
---
### 🔁 **Redirect Listener**
* **Purpose**: Redirects incoming traffic to another destination.
* **Use Case**: Handy for traffic redirection purposes.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: redir-in
      type: redir
      port: 7893
      listen: 0.0.0.0
  ```
  *Note*: Typically used for transparent proxying scenarios.
---
### 🛡️ **TPROXY Listener**
* **Purpose**: Intercepts and redirects traffic at the kernel level.
* **Use Case**: Ideal for advanced routing and transparent proxy setups.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: tproxy-in
      type: tproxy
      port: 7894
      listen: 0.0.0.0
      udp: true
  ```
  *Note*: Requires kernel support for TPROXY.
---
### 🧭 **TUN Listener**
* **Purpose**: Creates a virtual network interface for routing traffic.
* **Use Case**: Suitable for VPN-like setups and advanced routing scenarios.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: tun-in
      type: tun
      device: tun0
      mtu: 1500
  ```
  *Note*: Provides low-level network control.
---
### 🔐 **Shadowsocks (SS) Listener**
* **Purpose**: Handles Shadowsocks proxy connections.
* **Use Case**: Commonly used for bypassing network restrictions.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: ss-in
      type: ss
      port: 10001
      listen: 0.0.0.0
      cipher: aes-256-gcm
      password: mypass123
      udp: true
  ```
  *Note*: Lightweight and widely supported.
---
### 🌐 **VMess Listener**
* **Purpose**: Accepts VMess protocol connections.
* **Use Case**: Designed for V2Ray clients.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: vmess-in
      type: vmess
      port: 10002
      listen: 0.0.0.0
      users:
        - id: uuid1
          alterId: 64
  ```
  *Note*: Supports advanced features like multiplexing.
---
### 🌐 **VLESS Listener**
* **Purpose**: Handles VLESS protocol connections.
* **Use Case**: Successor to VMess with improved security.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: vless-in
      type: vless
      port: 10003
      listen: 0.0.0.0
      users:
        - id: uuid1
          flow: none
  ```
  *Note*: Focuses on simplicity and security.
---
### 🔐 **AnyTLS Listener**
* **Purpose**: Supports any TLS handshake, useful for obfuscation.
* **Use Case**: Helps in bypassing deep packet inspection.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: anytls-in
      type: anytls
      port: 10005
      listen: 0.0.0.0
  ```
  *Note*: Provides flexibility in TLS configurations.
---
### 🚀 **TUIC v4/v5 Listener**
* **Purpose**: Implements TUIC protocol for ultra-low latency connections.
* **Use Case**: Optimized for high-speed, low-latency applications.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: tuic-v4-in
      type: tuic-v4
      port: 10006
      listen: 0.0.0.0
  ```
  *Note*: Version 5 introduces enhanced features.
---
### 🌀 **Hysteria2 Listener**
* **Purpose**: Supports the Hysteria2 protocol for high-speed connections.
* **Use Case**: Suitable for high-speed, low-latency applications.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: hysteria2-in
      type: hysteria2
      port: 10007
      listen: 0.0.0.0
  ```
  *Note*: Designed for high-performance scenarios.
---
### 🧩 **Tunnel Listener**
* **Purpose**: Provides a generic tunnel interface.
* **Use Case**: Useful for custom tunneling solutions.
* **Configuration Example**:
  ```yaml
  listeners:
    - name: tunnel-in
      type: tunnel
      port: 10008
      listen: 0.0.0.0
  ```
  *Note*: Offers flexibility for custom tunneling needs.
---
### ✅ **Direct (`DIRECT`)**
* **Description**: Allows data to exit directly without any proxy intervention.
* **Use Case**: Ideal for traffic that doesn't require proxying, ensuring direct access to the internet.
---
### ❌ **Reject (`REJECT`)**
* **Description**: Intercepts and blocks data from going out.
* **Use Case**: Useful for blocking specific traffic or destinations, enhancing security or compliance.
---
### 🔕 **Reject-Drop (`REJECT-DROP`)**
* **Description**: Silently drops the request without any response.
* **Use Case**: Prevents detection of blocked traffic, useful in restrictive environments.
---
### 🔓 **Pass (`PASS`)**
* **Description**: Skips this rule when matching, effectively bypassing the proxy.
* **Use Case**: Allows certain traffic to bypass proxy rules, ensuring direct access when needed.
---
### ⚙️ **Compatible (`COMPATIBLE`)**
* **Description**: Appears when no node is selected in the policy group filtering, equivalent to `DIRECT`.
* **Use Case**: Provides a fallback option when no specific proxy is selected, ensuring traffic isn't inadvertently blocked.
---
These strategies allow for granular control over how traffic is handled, enabling you to tailor your proxy configuration to meet specific needs and scenarios.
---
## 🌐 **DNS Proxy**
In Mihomo, the DNS proxy type is designed to handle DNS requests, allowing for domain resolution through specified servers. This is particularly useful when you want to control or route DNS queries separately from regular internet traffic.
### Key Features:
* **Domain Resolution**: Directs DNS queries to specified DNS servers.
* **Proxying DNS Traffic**: Allows DNS requests to be routed through a proxy, enabling DNS-over-HTTPS (DoH) or DNS-over-TLS (DoT) configurations.
* **Enhanced Privacy**: By routing DNS queries through a proxy, you can enhance privacy and security, preventing DNS leaks.
### Example Configuration:
```yaml
proxies:
  - name: "dns-out"
    type: dns
    server: "dns.example.com"
    port: 53
    udp: true
    tls: true
    dns-over-https: true
    dns-over-tls: true
```
In this configuration, DNS queries are sent to `dns.example.com` over both UDP and TLS, with DNS-over-HTTPS enabled for enhanced privacy.
---
## 🔗 **Direct Proxy**
The Direct proxy type in Mihomo is used to allow traffic to bypass the proxy and connect directly to the destination. This is useful for local network access or when certain traffic should not be proxied.
### Key Features:
* **Bypass Proxy**: Allows specified traffic to bypass the proxy and connect directly.
* **Local Network Access**: Useful for accessing local network resources without routing through the proxy.
* **Selective Routing**: Can be used in conjunction with routing rules to selectively bypass the proxy for specific destinations or protocols.
### Example Configuration:
```yaml
proxies:
  - name: "direct"
    type: direct
    udp: true
    ip-version: ipv4
    interface-name: eth0
    routing-mark: 1234
```
Here, traffic is routed directly through the `eth0` interface, with the specified routing mark ensuring that only matching traffic bypasses the proxy.
---
## 🔄 **Combining DNS and Direct Proxies**
You can combine DNS and Direct proxies to create a more granular and efficient routing setup. For instance, you might want DNS queries to be resolved through a secure DNS proxy while allowing the actual traffic to bypass the main proxy.
### Example Combined Configuration:
```yaml
proxies:
  - name: "dns-out"
    type: dns
    server: "dns.example.com"
    port: 53
    udp: true
    tls: true
    dns-over-https: true
    dns-over-tls: true

  - name: "direct"
    type: direct
    udp: true
    ip-version: ipv4
    interface-name: eth0
    routing-mark: 1234
```
In this setup, DNS queries are securely resolved through `dns.example.com`, while the actual traffic is routed directly through the `eth0` interface, ensuring efficient and secure connections.
---
## 🔄 What Are Proxy Groups?
In Mihomo, **Proxy Groups** are logical collections of outbound proxies that determine how traffic is routed based on specific strategies. They allow for flexible and dynamic management of outbound connections, enabling users to define rules for selecting proxies based on criteria like latency, availability, or geographical location.
---
## 🧩 Common Fields in Proxy Groups
Each proxy group in Mihomo can include the following fields:
* **name**: The name of the proxy group.
* **type**: The type of the proxy group, which dictates its selection strategy.
* **proxies**: A list of outbound proxies or other proxy groups included in this group.
* **use**: References to proxy sets that provide additional proxies.
* **url**: A health check test address to monitor the availability of proxies.
* **interval**: The health check interval, in seconds.
* **lazy**: If set to `true`, health checks are only performed when the proxy group is selected.
* **timeout**: The health check timeout, in milliseconds.
* **max-failed-times**: The maximum number of failures before triggering a forced health check.
* **disable-udp**: Disables UDP for this proxy group.
* **interface-name**: Specifies the outbound interface for the proxy group.
* **routing-mark**: The routing mark attached when the proxy group is outbound.
* **include-all**: Includes all outbound proxies and proxy sets, sorted by name.
* **include-all-proxies**: Includes all outbound proxies, sorted by name.
* **include-all-providers**: Includes all proxy sets, sorted by name.
* **filter**: Filters nodes that meet keywords or regular expressions.
* **exclude-filter**: Excludes nodes that meet keywords or regular expressions.
* **exclude-type**: Excludes nodes based on node type.
* **expected-status**: The expected HTTP response status code during health checks.
* **hidden**: Returns `hidden` status in the API to hide the display of this proxy group.
* **icon**: Returns the string input for `icon` in the API to display in this proxy group.
---
## 🧭 Types of Proxy Groups
Mihomo supports several types of proxy groups, each serving a different purpose:
* **select**: Allows manual selection of a proxy from the list.
* **url-test**: Automatically selects the proxy with the best performance based on URL testing.
* **fallback**: Provides a fallback proxy when the primary proxy fails.
* **load-balance**: Distributes traffic evenly across multiple proxies.
* **relay**: Chains multiple proxies together for traffic routing.
---
For detailed information and examples on configuring each type of proxy group, you can refer to the official Mihomo documentation on Proxy Groups: ([wiki.metacubex.one][1]).
[1]: https://wiki.metacubex.one/en/config/proxy-groups/?utm_source=chatgpt.com "proxy-groups configuration - mihomo docs"
---
In Mihomo, the **relay** proxy group enables the chaining of multiple proxy nodes, allowing traffic to traverse through a sequence of proxies before reaching its destination. This setup can enhance security, obfuscate traffic patterns, and bypass certain network restrictions.
---
### 🔗 **How Relay Works**
When configured, traffic flows through a series of proxies in the specified order. For example:
```
Clash → http → vmess → ss1 → ss2 → Internet
```
In this chain, each proxy node forwards the traffic to the next, with the final destination being the internet.
---
### ⚠️ **Important Considerations**
* **Deprecation**: The relay strategy is being deprecated. It is recommended to use the `dialer-proxy` strategy instead. ([wiki.metacubex.one][1])
* **UDP Support**: Relay supports UDP transmission, provided that both the head and tail nodes of the proxy chain support UDP over TCP. Currently, protocols like `vmess`, `vless`, `trojan`, `ss`, `ssr`, and `tuic` support UDP. ([wiki.metacubex.one][1])
* **WireGuard Compatibility**: WireGuard does not support usage in relay configurations. It's advisable to use `dialer-proxy` for such setups. ([wiki.metacubex.one][1])
---
### 🛠️ **Configuration Example**
```yaml
proxy-groups:
  - name: "relay"
    type: relay
    proxies:
      - http
      - vmess
      - ss1
      - ss2
```
In this configuration, the `relay` proxy group chains the `http`, `vmess`, `ss1`, and `ss2` proxies.
---
### 🔄 **Transition to dialer-proxy**
Given the deprecation of the relay strategy, it's recommended to transition to the `dialer-proxy` strategy for chaining proxies. This approach offers more flexibility and is better supported in recent versions of Mihomo.
---
[1]: https://wiki.metacubex.one/en/config/proxy-groups/relay/?utm_source=chatgpt.com "Relay - mihomo docs"
---
In Mihomo, **sub-rule** is a configuration option that allows you to define a set of routing rules under a specific name, which can then be referenced within other routing rules. This modular approach enhances the flexibility and maintainability of your routing configurations.
---
### 🔧 Structure of a Sub-Rule
A sub-rule is defined under the `sub-rules` section in your configuration file. Each sub-rule is given a unique name, followed by a list of routing rules. These rules can include domain-based matches, IP-based matches, or other criteria, each associated with a specific proxy or action.
**Example Configuration:**
```yaml
sub-rules:
  rule1:
    - DOMAIN-SUFFIX,baidu.com,DIRECT
    - MATCH,PROXY
  sub-rule2:
    - IP-CIDR,1.1.1.1/32,REJECT
    - IP-CIDR,8.8.8.8/32,ss1
    - DOMAIN,dns.alidns.com,REJECT
```
---
### 🧩 Key Components
* **rule1** and **sub-rule2**: These are the unique names assigned to each sub-rule.
* **DOMAIN-SUFFIX, baidu.com, DIRECT**: This rule directs traffic to `baidu.com` to bypass the proxy and connect directly.
* **MATCH, PROXY**: This default rule applies the `PROXY` strategy to all other traffic that doesn't match previous rules.
* **IP-CIDR, 1.1.1.1/32, REJECT**: This rule blocks traffic to the specific IP address `1.1.1.1`.
* **DOMAIN, dns.alidns.com, REJECT**: This rule blocks traffic to the domain `dns.alidns.com`.
---
### 🔄 Referencing Sub-Rules
Once defined, sub-rules can be referenced within other routing rules using the `SUB-RULE` match type. This allows for the inclusion of complex rule sets within a single routing rule.
**Example Usage:**
```yaml
rules:
  - DOMAIN-SUFFIX,google.com,PROXY
  - DOMAIN-KEYWORD,facebook,REJECT
  - SUB-RULE,rule1
  - SUB-RULE,sub-rule2
```
In this example, traffic to `google.com` is proxied, traffic with `facebook` in the domain is rejected, and then the rules from `rule1` and `sub-rule2` are applied.
---
### ⚠️ Important Considerations
* **Parentheses Usage**: When using logical operators like `AND`, `OR`, or `NOT` in conjunction with sub-rules, ensure that parentheses are used correctly to define the scope of operations.
* **Rule Matching Order**: The order of rules matters. Mihomo processes rules sequentially, and once a match is found, subsequent rules are not evaluated.
* **DNS Resolution**: Be cautious when using domain-based rules, as DNS resolution may occur before rule matching. This can lead to unexpected behavior if not properly configured.
---
For more detailed information and advanced configurations, you can refer to the official Mihomo documentation on sub-rules: ([wiki.metacubex.one][1])
[1]: https://wiki.metacubex.one/en/config/sub-rule/?utm_source=chatgpt.com "sub-rule - mihomo docs"
---
In Mihomo, **tunnels** facilitate the forwarding of TCP/UDP traffic through specified local and target addresses, optionally routing through a defined proxy. This feature is particularly useful for scenarios like DNS forwarding or creating custom proxy chains.
---
### 🔧 Tunnel Configuration
Tunnels can be defined in two formats: single-line and multi-line.
#### Single-Line Format
```yaml
tunnels:
  - tcp/udp,127.0.0.1:6553,8.8.8.8:53,proxy
```
In this format, the components are specified in a comma-separated list:
* **network**: `tcp` or `udp`
* **address**: Local listening address (e.g., `127.0.0.1:6553`)
* **target**: Target address to forward traffic to (e.g., `8.8.8.8:53`)
* **proxy**: Optional; name of the proxy or proxy group to route traffic through (e.g., `proxy`)
#### Multi-Line Format
```yaml
tunnels:
  - network: [tcp, udp]
    address: 127.0.0.1:6553
    target: 8.8.8.8:53
    proxy: proxy
```
In this format, each component is specified on a separate line for clarity:
* **network**: List of protocols (`tcp`, `udp`)
* **address**: Local listening address
* **target**: Target address
* **proxy**: Optional; proxy or proxy group
---
### 📌 Example Use Case
To forward DNS queries from your local machine to Google's DNS server via a proxy:
```yaml
tunnels:
  - network: [udp]
    address: 127.0.0.1:6553
    target: 8.8.8.8:53
    proxy: proxy
```
In this example, DNS requests sent to `127.0.0.1:6553` are forwarded to `8.8.8.8:53` through the specified `proxy`.
---
For more detailed information and advanced configurations, you can refer to the official Mihomo documentation on tunnels: ([wiki.metacubex.one][1])
[1]: https://wiki.metacubex.one/en/config/tunnels/?utm_source=chatgpt.com "tunnels - mihomo docs"
