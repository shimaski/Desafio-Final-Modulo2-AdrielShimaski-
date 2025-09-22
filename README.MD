![alt text](/imagens/logo.png)

# Proposta de Arquitetura de Defesa e Plano de Monitoramento/IR – Loja Zeta

Proposta técnica oficial para a implementação de uma arquitetura moderna, segura e escalável para aplicações web baseadas em **Node.js**, com integração a banco de dados relacional e monitoramento contínuo.

---

## Sumário Executivo

A arquitetura proposta oferece:

- **Proteção perimetral** com WAF/CRS  
- **Balanceamento de carga** para alta disponibilidade  
- **Camada de identidade** para autenticação e autorização  
- **Coleta e correlação de logs** para visibilidade e resposta a incidentes  
- **Banco de dados PostgreSQL** integrado à aplicação  
- **Escalabilidade horizontal** via múltiplas instâncias da aplicação  

---

## Documentação

- [Proposta Técnica Completa](docs/proposta-tecnica-completa.md)  
- [Tecnologias e Ferramentas](docs/tecnologias.md)  
- [Plano de Implementação](/docs/implamentacao-faseada.md)
- [Plano de Ação](/docs/plano-de-acao.md)   
- [Diagramas e Arquitetura](/docs/diagramas.md)  

---

## Estrutura do Projeto

A arquitetura da aplicação seguirá o modelo de **defesa em profundidade**, cobrindo múltiplas camadas:

- **Internet → WAF/CRS → Load Balancer** (proteção perimetral e disponibilidade)  
- **Aplicação Node.js** com escalabilidade horizontal e patching contínuo  
- **Banco de dados PostgreSQL** com controles de acesso e criptografia em repouso  
- **Camada de Identidade (IAM/Auth)** garantindo autenticação e autorização centralizada  
- **Módulo de Coleta/Correlação (SIEM)** recebendo logs da aplicação, identidade e banco, com métricas de monitoramento (MTTD, MTTR, % cobertura de logs)  
- **Plano de Resposta a Incidentes (NIST IR)** com runbooks para SQLi, XSS, brute-force e indisponibilidade  

---

## Monitoramento & SIEM

- Fontes de log: **Nginx, aplicação, PostgreSQL, sistema operacional e IAM**  
- Casos de uso principais:  
  - Tentativas de login maliciosas  
  - Erros de banco suspeitos (SQLi)  
  - Anomalias de tráfego na aplicação  
- KPIs/Métricas:  
  - **MTTD** (tempo médio de detecção)  
  - **MTTR** (tempo médio de resposta)  
  - **Cobertura de logs (% dos sistemas monitorados)**  

---

## Resposta a Incidentes (NIST IR)

Fluxo adotado:  
**Detecção → Contenção → Erradicação → Recuperação → Lições aprendidas**

Runbooks disponíveis:  
- **SQL Injection**  
- **XSS**  
- **Brute-force em login**  
- **Indisponibilidade do sistema**  

---

## Recomendações (80/20) e Roadmap

- **30 dias**: WAF ativo, regras de firewall ajustadas, isolamento de rede de visitantes  
- **90 dias**: SIEM integrado, alertas configurados, autenticação IAM centralizada  
- **180 dias**: Backup criptografado, redundância em múltiplas zonas, revisão completa de políticas  

---

## Riscos, Custos e Assunções

- **Riscos**: dependência de configuração incorreta em WAF, falhas de patching  
- **Custos**: licenciamento SIEM, horas de consultoria, infraestrutura de backup  
- **Assunções**: equipe de TI dedicada, orçamento aprovado e aderência às recomendações  

---

## Conclusão

A proposta oferece **defesa em profundidade**, visibilidade centralizada e preparação para incidentes, reduzindo riscos críticos da Loja Zeta e garantindo continuidade do negócio.

---

## Autor

**Adriel Shimaski**  
Curso: *Cybersec - Vai na Web / Kensei*  

---

![Status](https://img.shields.io/badge/status-em%20andamento-yellow)  
![Versão](https://img.shields.io/badge/versão-1.0-blue)  
![Licença](https://img.shields.io/badge/licença-MIT-green)
