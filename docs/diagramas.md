# Diagramas e Arquitetura - Loja Zeta

---


## Diagrama de Arquitetura Principal

# Fluxo de Dados e Proteção

```mermaid
flowchart TB
    Internet[Internet] --> CDN[CDN Cloudflare]
    CDN --> WAF[WAF/ModSecurity]
    WAF --> LB[Load Balancer<br/>Nginx]
    
    LB --> Web1[Web Server 1<br/>Node.js]
    LB --> Web2[Web Server 2<br/>Node.js]
    
    Web1 --> DB[(PostgreSQL<br/>Primary)]
    Web2 --> DB
    DB --> DB_Replica[(PostgreSQL<br/>Replica)]
    
    subgraph Monitoring [Monitoramento e Segurança]
        SIEM[SIEM Wazuh]
        Grafana[Grafana Dashboards]
        Alertas[Sistema de Alertas]
    end
    
    Web1 -->|logs| SIEM
    Web2 -->|logs| SIEM
    DB -->|logs| SIEM
    WAF -->|logs| SIEM
    
    SIEM --> Grafana
    SIEM --> Alertas
    
    subgraph Identity [Gestão de Identidade]
        IAM[Serviço IAM]
        MFA[MFA/2FA]
        RBAC[Controle de Acessos]
    end
    
    IAM --> Web1
    IAM --> Web2
```
   
## Arquitetura de Rede

# Segmentação e Zonas de segurança
```mermaid
graph TB
    Internet[Internet] --> DMZ[DMZ Zone]
    
    subgraph DMZ [Zona Desmilitarizada]
        WAF[WAF/Proxy]
        LB[Load Balancer]
    end
    
    subgraph APP [Zona Aplicação]
        Web1[Web Server 1]
        Web2[Web Server 2]
        Cache[Redis Cache]
    end
    
    subgraph DATA [Zona Dados]
        DB_Primary[(DB Primary)]
        DB_Replica[(DB Replica)]
        Backup[Backup Server]
    end
    
    subgraph MGMT [Zona Gerência]
        SIEM[SIEM Server]
        Monitor[Monitoring]
        Admin[Admin Workstation]
    end
    
    DMZ --> APP
    APP --> DATA
    MGMT --> APP
    MGMT --> DATA
```

## Diagrama de Monitoramento

# Fluxo de Coleta e Análise
```mermaid
flowchart LR
    subgraph DataSources [Fontes de Dados]
        A1[Aplicação Node.js]
        A2[Serviços Nginx]
        A3[Banco PostgreSQL]
        A4[Sistema Operacional]
    end
    
    subgraph Collection [Coleta]
        B1[Filebeat]
        B2[Metricbeat]
        B3[Wazuh Agent]
    end
    
    subgraph Processing [Processamento]
        C1[Logstash]
        C2[Elasticsearch]
        C3[Wazuh Server]
    end
    
    subgraph Visualization [Visualização]
        D1[Grafana]
        D2[Kibana]
        D3[Alertas]
    end
    
    A1 --> B1
    A2 --> B2
    A3 --> B3
    A4 --> B3
    
    B1 --> C1
    B2 --> C1
    B3 --> C3
    
    C1 --> C2
    C3 --> C2
    C2 --> D1
    C2 --> D2
    C2 --> D3
```
## Topologia Física

# Distribuição de Servidores
```mermaid
graph TB
    subgraph DC_Primary [Data Center Primário]
        FW1[Firewall Primary]
        SW1[Switch Core]
        
        subgraph Rack1 [Rack 1 - Web]
            WEB1[Web 1]
            WEB2[Web 2]
            LB1[Load Balancer]
        end
        
        subgraph Rack2 [Rack 2 - Data]
            DB1[DB Primary]
            DB2[DB Replica]
            ST1[Storage]
        end
    end
    
    subgraph DC_Backup [Data Center Backup]
        FW2[Firewall Backup]
        WEB3[Web Hot Standby]
        DB3[DB Async Replica]
    end
    
    Internet --> FW1
    FW1 --> SW1
    SW1 --> Rack1
    SW1 --> Rack2
    DB1 --> DB3
```