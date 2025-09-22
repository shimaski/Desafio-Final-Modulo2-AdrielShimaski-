

![alt text](</docs/diagrama.png>)

# Projeto de Arquitetura Segura – Aplicação Node.js

Proposta técnica oficial para a implementação de uma arquitetura moderna, segura e escalável para aplicações web baseadas em **Node.js**, com integração a banco de dados relacional e monitoramento contínuo.

---

##  Sumário Executivo

A arquitetura proposta oferece:

- **Proteção perimetral** com WAF/CRS  
- **Balanceamento de carga** para alta disponibilidade  
- **Camada de identidade** para autenticação e autorização  
- **Coleta e correlação de logs** para visibilidade e resposta a incidentes  
- **Banco de dados PostgreSQL** integrado à aplicação  
- **Escalabilidade horizontal** via múltiplas instâncias da aplicação  

---

##  Documentação

- [ Proposta Técnica Completa](docs/proposta-tecnica-completa.md)  
- [ Tecnologias e Ferramentas](docs/tecnologias.md)  
- [ Plano de Implementação](docs/implementacao/plano-acao.md)  
- [ Diagramas e Arquitetura](docs/diagramas.md)  
- [ Documento em PDF](docs/pdf/arquitetura-nodejs.pdf)  

---

##  Estrutura do Projeto

A arquitetura da aplicação será baseada em:

- **Internet → WAF/CRS → Load Balancer**  
- **Aplicação Node.js** com escalabilidade horizontal  
- **Banco de dados PostgreSQL** para persistência de dados  
- **Camada de Identidade** garantindo segurança de acesso  
- **Módulo de Coleta/Correlação** recebendo logs da aplicação e do banco  
- **Integração com pipelines de segurança e monitoramento**  

---

##  Destaques Técnicos

- **Segurança de ponta a ponta** com WAF, identidade e monitoramento  
- **Escalabilidade horizontal** para lidar com aumento de usuários  
- **Alta disponibilidade** com load balancer e redundância  
- **Centralização de logs** para análise e auditoria  
- **Banco de dados relacional confiável** com PostgreSQL  
- Preparação para **integração futura com serviços em nuvem**  

---

##  Autor

**Adriel Shimaski**  
Curso: *Cybersec - Vai na Web / Kensei*  


---

![Status](https://img.shields.io/badge/status-em%20andamento-yellow)
![Versão](https://img.shields.io/badge/versão-1.0-blue)
![Licença](https://img.shields.io/badge/licença-MIT-green)
