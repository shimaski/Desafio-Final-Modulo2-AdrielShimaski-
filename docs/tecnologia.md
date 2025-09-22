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
# /etc/nginx/nginx.conf
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

### Explicação:

Security headers

##  X-Frame-Options DENY: Bloqueia o carregamento do site em frames/iframes, prevenindo ataques de clickjacking.

##  X-Content-Type-Options nosniff: Evita que navegadores interpretem arquivos como um tipo diferente do que foi declarado pelo servidor, prevenindo alguns ataques de MIME-type.
 
## X-XSS-Protection "1; mode=block": Habilita proteção contra Cross-Site Scripting (XSS) em navegadores que suportam este header.

### Rate limiting

# limit_req_zone $binary_remote_addr zone=login:10m rate=10r/m;: Define uma zona de limite de requisições por IP ($binary_remote_addr) chamada login. Limita cada IP a 10 requisições por minuto, ajudando a prevenir ataques de força bruta.

WAF configuration (Web Application Firewall)

modsecurity on;: Ativa o ModSecurity, um firewall de aplicações web.

modsecurity_rules_file /etc/nginx/modsecurity.conf;: Define o arquivo de regras que o ModSecurity vai usar para filtrar ataques.
