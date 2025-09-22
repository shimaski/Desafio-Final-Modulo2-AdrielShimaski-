# Tecnologias e Ferramentas - Loja Zeta

---

## Stack Tecnológico Principal

### Camada de Aplicação
| Componente  | Versão    | Finalidade                 | Observações                          |
|-------------|-----------|----------------------------|--------------------------------------|
| **Node.js** | 18.x LTS  | Runtime da aplicação       | Versão LTS com suporte até 2025      |
| **Express.js** | 4.x    | Framework web              | Configuração de segurança reforçada  |
| **Nginx**   | 1.22+     | Reverse proxy + Load balancer | Com módulo WAF integrado          |

### Banco de Dados
| Componente   | Versão | Configuração          |
|--------------|--------|----------------------|
| **PostgreSQL** | 15.x | Cluster ativo-standby |
| **pgBouncer**  | 1.18+| Connection pooler     |
| **Wal-G**      | 2.0+ | Backup e recovery     |

### Segurança e Monitoramento
| Categoria        | Ferramenta                  | Licença      | Uso                           |
|------------------|-----------------------------|--------------|-------------------------------|
| **WAF**          | ModSecurity + OWASP CRS     | Open Source  | Proteção contra ataques web   |
| **SIEM**         | Wazuh                       | Open Source  | Correlação de logs e alertas  |
| **Monitoring**   | Prometheus + Grafana        | Open Source  | Métricas de performance       |
| **Vulnerability**| Trivy                       | Open Source  | Scan de containers e código   |

---
# ESPECIFICAÇÕES DE CONFIGURAÇÃO

---

## 1. Componentes

- **WAF/CRS:** ModSecurity com regras OWASP Core Rule Set  
- **Rate Limiting:** Limitação de requisições por IP/endpoint  
- **Geo-blocking:** Bloqueio de tráfego de regiões de alto risco  

---

## 2. Camada de Rede

### 2.1 Segmentação

- **VLAN 10:** Rede pública (DMZ)  
- **VLAN 20:** Rede de aplicação  
- **VLAN 30:** Rede de banco de dados  
- **VLAN 40:** Rede administrativa  

### 2.2 Controles

- **ACLs (Access Control Lists)** entre VLANs  
- **NAC (Network Access Control)** para dispositivos  
- **VPN site-to-site** para acesso remoto seguro  

---

## 3. Camada de Dados

### 3.1 Estratégia de Backup

- **Backup completo:** Diário (23:00)  
- **Backup incremental:** Horário  
- **Retenção:** 30 dias local + 90 dias na nuvem  
- **Testes de restauração:** Semanal  

### 3.2 Criptografia

- **Dados em repouso:** LUKS + PostgreSQL TDE  
- **Dados em trânsito:** TLS 1.3  

---

## 4. Especificações Técnicas

### 4.1 Requisitos de Hardware/Software

| Componente       | Especificação             | Quantidade           |
|-----------------|---------------------------|--------------------|
| Servidor Web     | 4vCPU, 8GB RAM, 100GB SSD | 2 (ativo/standby) |
| Banco de Dados   | 8vCPU, 16GB RAM, 200GB SSD | 1 (com replicação) |
| SIEM             | 4vCPU, 8GB RAM, 500GB HDD | 1                  |


## Camada de Aplicação
---

## NGINX SECURITY CONFIGURATION

**Arquivo:** /etc/nginx/nginx.conf

```nginx
http {
    # Security headers
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";
    
    # Rate limiting
    limit_req_zone $binary_remote_addr zone=login:10m rate=10r/m;
    
    # WAF configuration
    modsecurity on;
    modsecurity_rules_file /etc/nginx/modsecurity.conf;
}

---
Explicação:

Security headers:

X-Frame-Options DENY: Bloqueia o carregamento do site em frames/iframes, prevenindo ataques de clickjacking.

X-Content-Type-Options nosniff: Evita que navegadores interpretem arquivos como um tipo diferente do que foi declarado pelo servidor.

X-XSS-Protection "1; mode=block": Habilita proteção contra Cross-Site Scripting (XSS).

Rate limiting:

limit_req_zone $binary_remote_addr zone=login:10m rate=10r/m;: Define uma zona de limite de requisições por IP chamada "login". Limita cada IP a 10 requisições por minuto.

WAF configuration (Web Application Firewall):

modsecurity on;: Ativa o ModSecurity, firewall de aplicações web.

modsecurity_rules_file /etc/nginx/modsecurity.conf;: Define o arquivo de regras do ModSecurity.

---


NODE.JS SECURITY SETTINGS

Dependências no package.json:

{
  "dependencies": {
    "helmet": "^7.0.0",
    "express-rate-limit": "^6.7.0",
    "bcryptjs": "^2.4.3",
    "express-validator": "^7.0.1"
  }
}


Explicação :

helmet: Adiciona headers HTTP de segurança automaticamente.

express-rate-limit: Limita o número de requisições por IP.

bcryptjs: Biblioteca para hash seguro de senhas.

express-validator: Valida e sanitiza dados de entrada do usuário.

Configuração no app.js:

const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

app.use(helmet());
app.use(rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutos
    max: 100, // Máximo de 100 requisições por IP
    message: 'Too many requests from this IP'
}));


Explicação:

app.use(helmet()): Ativa todos os headers de segurança do Helmet.

app.use(rateLimit(...)): Configura limite de requisições por IP. Cada IP pode fazer até 100 requisições a cada 15 minutos. Se exceder, retorna a mensagem "Too many requests from this IP".