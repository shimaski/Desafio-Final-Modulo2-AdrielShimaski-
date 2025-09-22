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

## Especificações de Configuração

### Nginx Security Configuration
1. Nginx Security Configuration

---
   /etc/nginx/nginx.conf
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
### Explicação:

Security headers

  X-Frame-Options DENY: Bloqueia o carregamento do site em frames/iframes, prevenindo ataques de clickjacking.

 X-Content-Type-Options nosniff: Evita que navegadores interpretem arquivos como um tipo diferente do que foi declarado pelo servidor, prevenindo alguns ataques de MIME-type.
 
 X-XSS-Protection "1; mode=block": Habilita proteção contra Cross-Site Scripting (XSS) em navegadores que suportam este header.

 Rate limiting

limit_req_zone $binary_remote_addr zone=login:10m rate=10r/m;: Define uma zona de limite de requisições por IP ($binary_remote_addr) chamada login. Limita cada IP a 10 requisições por minuto, ajudando a prevenir ataques de força bruta.

WAF configuration (Web Application Firewall)

modsecurity on;: Ativa o ModSecurity, um firewall de aplicações web.

modsecurity_rules_file /etc/nginx/modsecurity.conf;: Define o arquivo de regras que o ModSecurity vai usar para filtrar ataques.
----

---

2. PostgreSQL Hardening
-- Security configurations
ALTER SYSTEM SET password_encryption = 'scram-sha-256';
ALTER SYSTEM SET ssl = on;
ALTER SYSTEM SET log_statement = 'ddl';
ALTER SYSTEM SET log_connections = on;
ALTER SYSTEM SET log_disconnections = on;

-- User and privilege management
CREATE ROLE app_user WITH LOGIN PASSWORD 'secure_password';
GRANT CONNECT ON DATABASE lojazeta TO app_user;

### Explicação:

Configurações de segurança

password_encryption = 'scram-sha-256': Configura criptografia forte para senhas, usando SCRAM-SHA-256.

ssl = on: Habilita conexões criptografadas via SSL/TLS.

log_statement = 'ddl': Loga apenas comandos DDL (CREATE, ALTER, DROP), útil para auditoria.

log_connections = on e log_disconnections = on: Loga todas as conexões e desconexões ao banco, ajudando na detecção de acessos suspeitos.

Gerenciamento de usuários e privilégios

CREATE ROLE app_user WITH LOGIN PASSWORD 'secure_password';: Cria um usuário com login e senha segura.

GRANT CONNECT ON DATABASE lojazeta TO app_user;: Permite que esse usuário se conecte ao banco lojazeta, controlando o acesso.

---

3. Node.js Security Settings

Dependências no package.json

{
  "dependencies": {
    "helmet": "^7.0.0",
    "express-rate-limit": "^6.7.0",
    "bcryptjs": "^2.4.3",
    "express-validator": "^7.0.1"
  }
}

---

### Explicação :

helmet: Adiciona headers HTTP de segurança automaticamente (como os do Nginx) para prevenir ataques comuns.

express-rate-limit: Limita o número de requisições por IP para prevenir ataques de força bruta.

bcryptjs: Biblioteca para hash seguro de senhas.

express-validator: Valida e sanitiza dados de entrada do usuário para prevenir ataques de injeção.
---

Configuração no app.js

const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

app.use(helmet());
app.use(rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutos
    max: 100, // Máximo de 100 requisições por IP
    message: 'Too many requests from this IP'
}));

---
### Explicação:

app.use(helmet()): Ativa todos os headers de segurança do Helmet.

app.use(rateLimit(...)): Configura o limite de requisições por IP. Aqui, cada IP pode fazer até 100 requisições a cada 15 minutos. Se exceder, retorna a mensagem "Too many requests from this IP".