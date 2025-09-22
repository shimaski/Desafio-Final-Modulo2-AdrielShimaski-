# Proposta de Arquitetura de Defesa e Plano de Monitoramento/IR – Loja Zeta

**Cliente:** LojaZeta  
**Data:** 22/09/2025  
**Consultor:** Adriel Shimaski  

## 1. Sumário Executivo

A Loja Zeta busca modernizar sua infraestrutura de TI e fortalecer a segurança cibernética para proteger dados de clientes, operações internas e ativos digitais. Este documento propõe uma arquitetura de defesa em camadas, combinando prevenção, detecção e resposta a incidentes, com foco em aplicações web, sistemas internos e canais de vendas online.

**Objetivos principais:**

- Garantir confidencialidade, integridade e disponibilidade das informações.  
- Minimizar riscos de ataques cibernéticos.  
- Cumprir requisitos regulatórios de proteção de dados.  
- Estabelecer um plano de resposta rápida a incidentes (IR).  

---

## 2. Arquitetura de Defesa Proposta

### 2.1 Camadas de Segurança

**Perímetro de Rede**  
- Firewall de próxima geração (NGFW).  
- Sistema de prevenção de intrusão (IPS).  
- Segmentação de rede para isolar sistemas críticos.  

**Segurança de Aplicações Web**  
- WAF (Web Application Firewall) para filtrar tráfego malicioso.  
- Monitoramento de vulnerabilidades em aplicações (scanner automático).  
- Políticas de desenvolvimento seguro (DevSecOps).  

**Proteção de Endpoints**  
- Antivírus/EDR em todos os dispositivos da loja e home office.  
- Controle de dispositivos móveis e laptops corporativos (MDM).  
- Atualizações automáticas de sistemas operacionais e softwares.  

**Controle de Identidade e Acesso**  
- Autenticação multifator (MFA) para todos os sistemas críticos.  
- Política de privilégios mínimos.  
- Monitoramento de logins suspeitos e tentativas de acesso.  

**Monitoramento e SIEM**  
- Coleta de logs de sistemas, firewalls, servidores e endpoints.  
- Sistema SIEM para correlação de eventos e alertas.  
- Dashboards em tempo real para acompanhamento da segurança.  

---

## 3. Plano de Monitoramento

- **Alertas em tempo real:** Notificação imediata para eventos críticos (tentativas de intrusão, malware, acessos anômalos).  
- **Análise de logs:** Revisão semanal e mensal para identificar padrões e vulnerabilidades.  
- **Relatórios gerenciais:** Painéis resumidos para tomada de decisão executiva.  
- **Auditoria de segurança:** Testes trimestrais de vulnerabilidades e pen tests periódicos.  

---

## 4. Plano de Resposta a Incidentes (IR)

### Preparação
- Treinamento da equipe para identificar e reportar incidentes.  
- Criação de playbooks para tipos comuns de ataques.  

### Detecção e Análise
- Monitoramento contínuo de SIEM e alertas de EDR.  
- Classificação do incidente: crítico, médio ou baixo impacto.  

### Contenção
- Isolamento de sistemas afetados.  
- Bloqueio de contas comprometidas e endereços IP maliciosos.  

### Erradicação e Recuperação
- Remoção de malware e vulnerabilidades exploradas.  
- Restauração de sistemas a partir de backups seguros.  
- Validação da integridade dos dados antes de voltar à operação.  

### Lições Aprendidas
- Relatório detalhado do incidente.  
- Atualização de políticas e ferramentas de segurança.  

---

## 5. Benefícios da Implementação

- Redução do risco de incidentes cibernéticos e perdas financeiras.  
- Maior confiança dos clientes e parceiros comerciais.  
- Cumprimento de legislações de proteção de dados (LGPD).  
- Ambiente de TI moderno, resiliente e escalável.  
