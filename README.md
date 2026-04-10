# 🛡️ Guia de Firewall: Resolução (UFW & iptables)

---

### 1) Verificar status
* **UFW:** `ufw status`
* **iptables:** `iptables -L -v -n`

### 2) Habilitar/desabilitar
* **UFW:** `ufw enable` / `ufw disable`
* **iptables:** `iptables -F`

### 3) Política padrão de INPUT
* **UFW:** `ufw default deny incoming`
* **iptables:** `iptables -P INPUT DROP`

### 4) Política padrão de OUTPUT
* **UFW:** `ufw default allow outgoing`
* **iptables:** `iptables -P OUTPUT ACCEPT`

### 5) SSH (Porta 22)
* **UFW:** `ufw allow 22/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`

### 6) HTTP (Porta 80)
* **UFW:** `ufw allow 80/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 80 -j ACCEPT`

### 7) Porta 8080
* **UFW:** `ufw allow 8080/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 8080 -j ACCEPT`

### 8) DNS (Porta 53)
* **UFW:** `ufw allow 53/udp`
* **iptables:** `iptables -A INPUT -p tcp --dport 53 -j ACCEPT`

### 9) Regras Essenciais
* **Loopback:** `iptables -A INPUT -i lo -j ACCEPT`
* **Estabelecidas:** `iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`

### 10) Bloquear 3306 (MySQL)
* **UFW:** `ufw deny 3306/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 3306 -j DROP`

### 11) Rejeitar 5432 (PostgreSQL)
* **UFW:** `ufw reject 5432/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 5432 -j REJECT`

### 12) Bloquear IP
* **UFW:** `ufw deny from 192.168.0.5`
* **iptables:** `iptables -A INPUT -s 1.2.3.4 -j DROP`

### 13) SSH para IP específico
* **UFW:** `ufw allow from 192.168.1.100 to any port 22 proto tcp`
* **iptables:** `iptables -A INPUT -p tcp -s 192.168.1.100 --dport 22 -j ACCEPT`

### 14) MYSQL para interface eth1
* **UFW:** `ufw allow in on eth1 to any port 3306`
* **iptables:** `iptables -A INPUT -i eth1 -p tcp --dport 3306 -j ACCEPT`

### 15) Bloquear pacotes inválidos
* **iptables:** `iptables -A INPUT -m state --state INVALID -j DROP`

### 16) Listar regras numeradas
* **UFW:** `ufw status numbered`
* **iptables:** `iptables -L INPUT --line-numbers`

### 17) Deletar regra 5
* **UFW:** `ufw delete 5`
* **iptables:** `iptables -D INPUT 5`

### 18) Resetar tudo
* **UFW:** `ufw reset`
* **iptables:**
  iptables -F
  iptables -X
  iptables -Z
  iptables -P INPUT ACCEPT
  iptables -P OUTPUT ACCEPT
  iptables -P FORWARD ACCEPT

### 19) Rate limit SSH
* **UFW:** `ufw limit 22/tcp`
* **iptables:** `iptables -A INPUT -p tcp --dport 22 -m limit --limit 5/minute -j ACCEPT`

### 20) Logging + bloqueio
* **UFW:**
  ufw logging on
  ufw deny 5432/tcp
* **iptables:**
  iptables -A INPUT -p tcp --dport 5432 -j LOG --log-prefix "DB ATTEMPT: "
  iptables -A INPUT -p tcp --dport 5432 -j DROP

### 21) Port forwarding
* **UFW:** Descomentar linha em `/etc/ufw/before.rules` (seção NAT)
* **iptables:** `iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080`
