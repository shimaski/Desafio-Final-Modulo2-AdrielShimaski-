# Proposta de Arquitetura de Defesa e Plano de Monitoramento/IR – Loja Zeta

**Cliente:** LojaZeta  
**Data:** 22/09/2025  
**Consultor:** Adriel Shimaski  

## Sumário Executivo

A Loja Zeta, enquanto operação de e-commerce, enfrenta riscos relevantes como ataques de injeção SQL, tentativas de força bruta contra autenticação, indisponibilidade por sobrecarga e exploração de vulnerabilidades não corrigidas.  

A arquitetura de defesa proposta adota o princípio de **defesa em profundidade**, criando múltiplas barreiras de proteção, visibilidade e resposta.  

Os principais pontos da solução são:

- **Proteção perimetral e disponibilidade**: uso de WAF/CRS integrado ao balanceador de carga para filtrar tráfego malicioso antes de chegar à aplicação.  
- **Aplicação resiliente e escalável**: instâncias Node.js distribuídas e atualizadas com patching contínuo.  
- **Banco de dados protegido**: PostgreSQL com criptografia em repouso, segregação de privilégios e monitoramento de consultas suspeitas.  
- **Identidade centralizada**: camada de autenticação e autorização (IAM/Auth) com políticas fortes de senhas e MFA.  
- **Monitoramento e correlação**: SIEM recebendo logs da aplicação, banco e identidade, com alertas em tempo real e métricas como MTTD e MTTR.  
- **Resposta estruturada a incidentes**: plano baseado no ciclo NIST IR (Detecção → Contenção → Erradicação → Recuperação → Lições).  

Com essa abordagem, a Loja Zeta alcançará:  

- Redução do tempo de detecção e resposta a ataques.  
- Maior disponibilidade da aplicação e experiência confiável para o cliente.  
- Conformidade com boas práticas de segurança aplicáveis ao setor de e-commerce.  
- Base sólida para futuras integrações em nuvem e expansão de mercado.  
---

## Objetivo e Escopo

### Objetivo
O objetivo desta proposta é fornecer à Loja Zeta uma arquitetura de defesa em profundidade que seja prática, escalável e aderente às necessidades de um e-commerce em crescimento.  
O foco principal é **proteger a aplicação web e os dados dos clientes**, aumentar a visibilidade sobre incidentes de segurança e garantir que a equipe, mesmo reduzida, consiga responder rapidamente a ataques e falhas.  

### Escopo
O trabalho cobre as seguintes áreas:

1. **Arquitetura de Defesa em Camadas**  
   - Proteção perimetral (WAF/CRS, firewall e segmentação de rede).  
   - Segurança da aplicação Node.js e banco de dados PostgreSQL.  
   - Controles de identidade e autenticação centralizada.  

2. **Monitoramento e SIEM**  
   - Centralização de logs (Nginx, aplicação, banco e sistema operacional).  
   - Criação de alertas acionáveis para ataques comuns (SQLi, XSS, brute-force).  
   - Métricas de eficácia: MTTD (tempo de detecção), MTTR (tempo de resposta), cobertura de logs.  

3. **Resposta a Incidentes (NIST IR)**  
   - Procedimentos de detecção, contenção, erradicação, recuperação e lições aprendidas.  
   - Runbooks específicos para ataques já observados: SQLi, XSS, brute-force em `/login` e indisponibilidade da aplicação.  

4. **Plano de Ação 80/20**  
   - Quick wins de até 30 dias (impacto imediato).  
   - Ações de médio prazo (90 dias).  
   - Estratégia de longo prazo (180 dias) para consolidação da segurança.  

### Fora do escopo
- Implementação de SOC 24/7 dedicado.  
- Conformidade regulatória específica (ex.: PCI-DSS), embora a arquitetura facilite adequações futuras.  
- Desenvolvimento de novas funcionalidades de aplicação.  
---

## Metodologia

A elaboração desta proposta seguiu uma abordagem consultiva baseada em boas práticas de segurança da informação e nas necessidades específicas da Loja Zeta.  

### Etapas da análise
1. **Levantamento de contexto**  
   - Análise do briefing fornecido (stack tecnológica: Nginx, Node.js e PostgreSQL).  
   - Consideração do histórico de incidentes recentes: tentativas de SQLi, XSS e brute-force em `/login`.  
   - Identificação das restrições: equipe reduzida (2 devs e 1 ops) e orçamento limitado.  

2. **Definição de prioridades (princípio 80/20)**  
   - Foco inicial em medidas de maior impacto e menor complexidade.  
   - Priorização da proteção da aplicação e identidade, pontos mais críticos do e-commerce.  

3. **Construção da arquitetura de defesa em camadas**  
   - Organização em perímetro, rede, aplicação, dados e identidade.  
   - Integração de mecanismos de monitoramento e resposta a incidentes.  

4. **Adesão a frameworks reconhecidos**  
   - NIST Cybersecurity Framework para ciclo de vida de resposta a incidentes.  
   - Boas práticas de hardening em sistemas Linux, Node.js e PostgreSQL.  

### Premissas consideradas
- O ambiente de produção está hospedado em nuvem (IaaS).  
- A equipe técnica tem conhecimentos básicos de administração de sistemas e bancos de dados.  
- O orçamento inicial será direcionado para soluções de código aberto ou baixo custo (ex.: WAF CRS, ferramentas de SIEM open source).  
- O crescimento do e-commerce exigirá escalabilidade horizontal, logo a arquitetura deve suportar múltiplas instâncias de aplicação.  
---

## Arquitetura de Defesa (Camadas)

A Loja Zeta adotará uma estratégia de **defesa em profundidade**, aplicando controles em múltiplos níveis para dificultar ataques, reduzir a superfície de exposição e aumentar a resiliência da infraestrutura.

### 1. Camada de Perímetro
- **Firewall de Aplicação Web (WAF/CRS)**: protege contra ataques de injeção (SQLi, XSS) e requisições maliciosas.  
- **Firewall de Rede**: restrição de portas e protocolos, permitindo apenas o tráfego necessário (HTTP/HTTPS para aplicação, portas seguras para administração).  
- **Balanceador de Carga**: distribui tráfego entre múltiplas instâncias da aplicação, garantindo disponibilidade e resiliência.  

### 2. Camada de Rede
- **Segmentação de Rede**: separação entre ambiente de produção, banco de dados e rede de administração.  
- **Isolamento de Rede para Visitantes/Wi-Fi**: visitantes ou dispositivos não corporativos não devem ter acesso à rede de produção.  
- **Monitoramento de tráfego**: integração com IDS/IPS (quando possível) para detectar acessos anômalos.  

### 3. Camada de Aplicação
- **Hardening da aplicação Node.js**: atualização contínua (patching) e uso de boas práticas de codificação segura.  
- **Proteção contra brute-force**: limitação de tentativas de login e uso de MFA (autenticação multifator).  
- **Proteção contra injeção**: validação de entrada e parametrização de queries.  

### 4. Camada de Dados
- **Banco de Dados PostgreSQL**: acesso restrito apenas à aplicação.  
- **Criptografia em repouso**: dados sensíveis armazenados de forma segura.  
- **Backups regulares**: já existentes, mas com plano de testes de restauração documentado.  

### 5. Camada de Identidade
- **Gerenciamento Centralizado de Identidade (IAM/Auth)**: autenticação e autorização centralizada.  
- **Políticas de privilégios mínimos (PoLP)**: cada usuário ou serviço só acessa o necessário.  
- **Auditoria de acessos**: registros de login, falhas e tentativas suspeitas coletados pelo SIEM.  

---

### Benefícios da Arquitetura
- Redução da superfície de ataque (menos portas e serviços expostos).  
- Maior resiliência contra ataques web (SQLi, XSS e brute-force).  
- Controle granular de identidades e acessos.  
- Visibilidade ampliada por meio da centralização de logs e monitoramento.  
- Preparação para crescimento e escalabilidade sem perda de segurança.  

---

## 5. Benefícios da Implementação

- Redução do risco de incidentes cibernéticos e perdas financeiras.  
- Maior confiança dos clientes e parceiros comerciais.  
- Cumprimento de legislações de proteção de dados (LGPD).  
- Ambiente de TI moderno, resiliente e escalável.  

# Resposta a Incidentes (NIST IR)

A Loja Zeta adotará o framework do **NIST (National Institute of Standards and Technology)** para resposta a incidentes, garantindo uma abordagem estruturada e eficiente.

---

## Ciclo de Vida do Incidente (NIST IR)

### 1. Preparação
- **Equipe de Resposta:** Designação de responsáveis (Ops + Devs)  
- **Documentação:** Runbooks detalhados para cada tipo de incidente  
- **Ferramentas:** SIEM configurado, ferramentas de análise forense básicas  
- **Comunicação:** Lista de contatos de emergência e procedimentos de escalação  

### 2. Detecção e Análise
**Fontes de Detecção:**  
- Alertas automáticos do SIEM  
- Relatos de usuários  
- Monitoramento proativo  

**Análise Inicial:**  
- Classificação da severidade (Crítico, Alto, Médio, Baixo)  
- Identificação do escopo e impacto potencial  

### 3. Contenção, Erradicação e Recuperação
**Contenção Imediata:**  
- Isolamento de sistemas afetados  
- Bloqueio de IPs maliciosos  
- Revogação de credenciais comprometidas  

**Erradicação:**  
- Remoção de malware/código malicioso  
- Aplicação de patches e correções  

**Recuperação:**  
- Restauração de sistemas a partir de backups  
- Validação da integridade dos sistemas  
- Retorno gradual aos serviços normais  

### 4. Lições Aprendidas
- **Post-Mortem:** Análise detalhada do incidente  
- **Documentação:** Atualização de runbooks e procedimentos  
- **Melhorias:** Implementação de controles preventivos adicionais  

---

## Runbooks Específicos

### Runbook 1: SQL Injection
- **Detecção:** Alertas do WAF + queries suspeitas no PostgreSQL  
- **Contenção:** Bloqueio imediato do IP no WAF  
- **Erradicação:** Correção da vulnerabilidade na aplicação  
- **Recuperação:** Restauração de dados se necessário  
- **Tempo Esperado:** Contenção < 30 min, Correção < 4h  

### Runbook 2: Cross-Site Scripting (XSS)
- **Detecção:** Logs da aplicação + relatos de usuários  
- **Contenção:** Remoção do conteúdo malicioso  
- **Erradicação:** Implementação de sanitização de inputs  
- **Recuperação:** Limpeza de caches e sessões afetadas  
- **Tempo Esperado:** Contenção < 1h, Correção < 8h  

### Runbook 3: Brute-Force em Login
- **Detecção:** Múltiplas falhas de login do mesmo IP  
- **Contenção:** Bloqueio temporário do IP + throttle de tentativas  
- **Erradicação:** Revisão de políticas de senha + implementação de MFA  
- **Recuperação:** Reset de senhas de contas potencialmente comprometidas  
- **Tempo Esperado:** Contenção < 15 min, Correção < 2h  

### Runbook 4: Indisponibilidade da Aplicação
- **Detecção:** Monitoramento de saúde dos serviços  
- **Contenção:** Failover para instância secundária  
- **Erradicação:** Identificação e correção da causa raiz  
- **Recuperação:** Restauração completa dos serviços  
- **Tempo Esperado:** Contenção < 5 min, Recuperação < 1h  

---

## Matriz de Severidade e Resposta

| Severidade | Impacto                                | Tempo de Resposta | Ações                              |
|------------|----------------------------------------|-------------------|------------------------------------|
| Crítico    | Sistema indisponível ou dados comprometidos | < 30 min          | Mobilização total da equipe        |
| Alto       | Funcionalidade crítica afetada         | < 2 horas         | Envolvimento de especialistas      |
| Médio      | Impacto limitado                       | < 4 horas         | Resolução durante horário comercial|
| Baixo      | Impacto mínimo                         | < 8 horas         | Resolução conforme disponibilidade |

---

## Comunicação Durante Incidentes

- **Interna:** Notificação imediata da equipe técnica e gestores  
- **Clientes:** Comunicação transparente sobre afetações (se aplicável)  
- **Regulatórias:** Notificação conforme exigências legais (LGPD)  

# Recomendações (80/20) e Roadmap

Seguindo o princípio **80/20**, priorizamos ações que oferecem maior impacto com menor esforço, organizadas em ordem de criticidade e facilidade de implementação.

---

## Plano de Ação Prioritário (80/20)

| Ordem | Ação                                      | Impacto | Dificuldade | Prazo   | Responsável |
|-------|-------------------------------------------|---------|-------------|---------|-------------|
| 1     | Configurar regras básicas no firewall     | Alto    | Baixa       | 7 dias  | Ops         |
| 2     | Isolar rede de visitantes da produção     | Alto    | Média       | 15 dias | Ops         |
| 3     | Implementar backups automáticos criptografados | Alto | Média       | 15 dias | Ops         |
| 4     | Ativar MFA para acessos administrativos   | Alto    | Média       | 15 dias | Ops/Devs    |
| 5     | Implementar WAF/CRS no Nginx              | Alto    | Alta        | 30 dias | Ops/Devs    |
| 6     | Criar políticas de acesso Wi-Fi seguras   | Médio   | Alta        | 30 dias | Ops         |
| 7     | Revisar acessos administrativos (RBAC)    | Médio   | Alta        | 45 dias | Ops         |
| 8     | Implementar campanhas de awareness        | Médio   | Baixa       | 60 dias | Todos       |

---

## Roadmap Detalhado

### Fase 1: Fundamentos (0-30 dias) - *Quick Wins*
**Objetivo:** Estabelecer proteções básicas e visibilidade mínima  

**Semana 1-2:**  
- Configuração de regras de firewall (bloqueio portas não essenciais)  
- Isolamento da rede de visitantes  
- Implementação de backups automáticos  

**Semana 3-4:**  
- Ativação de MFA para contas administrativas  
- Configuração inicial do WAF com regras OWASP CRS  
- Centralização de logs críticos (Nginx, auth, errors)  

**Entregáveis:**  
- Firewall configurado e testado  
- Rede segmentada e isolada  
- Backup automático funcionando  
- MFA ativo para admins  

---

### Fase 2: Fortalecimento (30-90 dias)
**Objetivo:** Expandir monitoramento e controles de segurança  

**Mês 2:**  
- Implementação de SIEM básico (Wazuh/Graylog)  
- Configuração de alertas para brute-force e SQLi  
- Políticas de acesso Wi-Fi empresarial  

**Mês 3:**  
- Revisão de acessos administrativos (princípio do menor privilégio)  
- Hardening básico do sistema operacional  
- Testes de restauração de backup  

**Entregáveis:**  
- SIEM operacional com alertas básicos  
- Políticas de acesso revisadas e documentadas  
- Primeiro teste de restauração realizado  

---

### Fase 3: Maturação (90-180 dias)
**Objetivo:** Consolidar programa de segurança e preparar para escala  

**Mês 4-5:**  
- Campanhas de conscientização em segurança  
- Implementação de IDS/IPS básico  
- Revisão de código da aplicação (SAST básico)  

**Mês 6:**  
- Simulação de incidente (*tabletop exercise*)  
- Auditoria de segurança interna  
- Planejamento para automação de respostas  

**Entregáveis:**  
- Programa de awareness iniciado  
- IDS/IPS implementado e configurado  
- Primeira simulação de incidente realizada  

---

## Métricas de Sucesso por Fase

**Fase 1 (30 dias):**  
- 100% dos sistemas com firewall configurado  
- Rede de visitantes isolada da produção  
- MFA implementado para contas críticas  
- Backups automáticos em execução  

**Fase 2 (90 dias):**  
- 80% dos logs centralizados no SIEM  
- Alertas configurados para ameaças críticas  
- Primeiro teste de restauração bem-sucedido  
- Políticas de acesso revisadas  

**Fase 3 (180 dias):**  
- Redução de 50% em tentativas de acesso não autorizado  
- MTTD < 15 minutos para incidentes críticos  
- MTTR < 2 horas para incidentes comuns  
- Equipe treinada em procedimentos de IR  

---

## Dependências e Pré-requisitos

**Críticas:**  
- Acesso administrativo à infraestrutura  
- Aprovação de budget para ferramentas essenciais  
- Comprometimento da equipe com os prazos  

**Importantes:**  
- Documentação atualizada da infraestrutura  
- Backup atualizado e testado antes de mudanças críticas  
- Janela de manutenção para implementações sensíveis  

# Riscos, Custos e Assunções

---

## Riscos Identificados

### Riscos Técnicos

**Configuração Incorreta do WAF**  
- **Impacto:** Bloqueio de tráfego legítimo ou falha na proteção  
- **Mitigação:** Testes em ambiente de homologação antes da produção  
- **Probabilidade:** Média  

**Falha na Restauração de Backup**  
- **Impacto:** Indisponibilidade prolongada em caso de incidente  
- **Mitigação:** Testes regulares de restauração  
- **Probabilidade:** Baixa  

**Resistência à Adoção de MFA**  
- **Impacto:** Segurança comprometida em acessos administrativos  
- **Mitigação:** Treinamento e comunicação clara dos benefícios  
- **Probabilidade:** Média  

---

### Riscos Operacionais

**Falta de Recursos Humanos Especializados**  
- **Impacto:** Atraso na implementação ou configuração inadequada  
- **Mitigação:** Consultoria externa pontual ou treinamento da equipe  
- **Probabilidade:** Alta  

**Indisponibilidade Não Planejada**  
- **Impacto:** Interrupção das vendas online  
- **Mitigação:** Janelas de manutenção bem comunicadas e rollback planejado  
- **Probabilidade:** Baixa  

---

## Estimativa de Custos

### Custos Iniciais (Primeiros 90 dias)

| Item                      | Tipo           | Custo Estimado        | Periodicidade |
|---------------------------|----------------|-----------------------|---------------|
| Ferramentas Open Source   | Licença        | R$ 0                  | -             |
| Consultoria Especializada | Serviço        | R$ 5.000 - R$ 10.000  | Pontual       |
| Treinamento da Equipe     | Capacitação    | R$ 2.000 - R$ 4.000   | Inicial       |
| Hardware/Cloud adicional  | Infraestrutura | R$ 500 - R$ 1.000/mês | Mensal        |
| **Total Estimado Fase 1** |                | **R$ 7.500 - R$ 15.000** | -          |

---

### Custos Recorrentes (Após 90 dias)

| Item               | Custo Mensal       | Custo Anual          |
|--------------------|--------------------|----------------------|
| Manutenção SIEM    | R$ 300 - R$ 600    | R$ 3.600 - R$ 7.200  |
| Monitoramento Cloud | R$ 200 - R$ 400   | R$ 2.400 - R$ 4.800  |
| Backup e Storage   | R$ 150 - R$ 300    | R$ 1.800 - R$ 3.600  |
| **Total Recorrente** | R$ 650 - R$ 1.300/mês | R$ 7.800 - R$ 15.600/ano |

---

### Custos de Incidente (Prevenção vs Correção)

| Cenário             | Custo de Prevenção         | Custo de Correção                                    |
|---------------------|-----------------------------|-----------------------------------------------------|
| Vazamento de Dados  | R$ 10.000 (implementação)  | R$ 50.000 - R$ 200.000 (multas + reputação)         |
| Ransomware          | R$ 5.000 (backup + proteção) | R$ 100.000+ (resgate + downtime)                   |
| Indisponibilidade   | R$ 3.000 (alta disponibilidade) | R$ 10.000 - R$ 50.000/dia (vendas perdidas)       |

---

## Assunções do Projeto

### Assunções Técnicas
- **Infraestrutura Existente:**  
  - A infraestrutura atual suporta a implementação das soluções propostas  
  - Os sistemas possuem capacidade para rodar ferramentas de segurança adicionais  
- **Compatibilidade:**  
  - As ferramentas open source selecionadas são compatíveis com o stack tecnológico atual  
  - Não há conflitos com sistemas existentes  

### Assunções Operacionais
- **Recursos Humanos:**  
  - A equipe atual tem capacidade de absorver novas responsabilidades  
  - Haverá disponibilidade para treinamentos e implementação  
- **Acesso e Permissões:**  
  - Será concedido acesso necessário para implementação e configuração  
  - Não haverá restrições burocráticas significativas  

### Assunções de Negócio
- **Orçamento:**  
  - O budget estimado será aprovado e disponibilizado conforme cronograma  
  - Não haverá cortes significativos durante a implementação  
- **Priorização:**  
  - A segurança será tratada como prioridade pela direção  
  - As janelas de manutenção necessárias serão concedidas  

---

## Planos de Contingência

### Para Riscos Técnicos
- **Backup de Configurações:** Todas as alterações serão documentadas e terão rollback planejado  
- **Ambiente de Testes:** Implementação primeiro em homologação antes de produção  
- **Monitoramento Intensivo:** Período de observação após cada mudança crítica  

### Para Riscos Operacionais
- **Plano de Transição:** Treinamento gradual e documentação detalhada  
- **Suporte Externo:** Contrato com consultoria para suporte em implementações complexas  
- **Comunicação Clara:** Alinhamento constante com stakeholders sobre progresso e desafios  

### Para Riscos de Budget
- **Faseamento:** Implementação em fases para distribuir custos  
- **Alternativas Open Source:** Sempre que possível, usar soluções gratuitas  
- **ROI Claro:** Demonstrar valor através de métricas de redução de risco  

# Conclusão e Próximos Passos

---

## Conclusão
A proposta apresentada oferece à **Loja Zeta** uma base sólida para construção de um programa de segurança cibernética eficaz e sustentável.  
Com a abordagem de **defesa em profundidade**, **monitoramento centralizado** e **resposta estruturada a incidentes**, a empresa estará significativamente mais preparada para enfrentar os desafios de segurança no ambiente de e-commerce.  

### Principais benefícios esperados:
- Redução de **70-80%** nas tentativas de ataques bem-sucedidos nos primeiros 90 dias  
- **MTTD < 15min**: detecção de incidentes em menos de 15 minutos  
- **MTTR < 2h**: resposta reduzida para menos de 2 horas em cenários críticos  
- Conformidade com boas práticas de segurança e requisitos regulatórios básicos  
- Preservação da **reputação e confiança dos clientes**  

A estratégia **80/20** garantiu que os recursos limitados sejam alocados nas medidas de maior impacto, enquanto o **roadmap** estabelece um caminho claro para evolução contínua da maturidade em segurança.  

---

## Próximos Passos Imediatos (Semana 1)

- [ ] **Reunião de Alinhamento Final**  
  - **Data Sugerida:** Próximos 2 dias úteis  
  - **Participantes:** Diretoria, TI, Segurança  
  - **Objetivo:** Aprovação formal da proposta e liberação de recursos  

- [ ] **Kick-off do Projeto**  
  - **Data:** Imediatamente após aprovação  
  - **Entregável:** Cronograma detalhado com marcos e responsáveis  
  - **Ações:** Designação formal da equipe de projeto  

- [ ] **Implementação das Ações Críticas**  
  - **Prioridade 1:** Configuração de firewall (Dias 1-7)  
  - **Prioridade 2:** Isolamento de rede (Dias 8-15)  
  - **Prioridade 3:** Backups automatizados (Dias 8-15)  

---

## Critérios de Sucesso Mensuráveis

### 30 Dias (Fase 1)
- Firewall configurado e testado  
- Rede de visitantes isolada  
- MFA ativo para contas administrativas  
- Backups automáticos em execução  

### 90 Dias (Fase 2)
- SIEM operacional com **>80% de cobertura de logs**  
- Alertas configurados para ameaças críticas  
- Primeiro teste de restauração bem-sucedido  
- Políticas de acesso revisadas e documentadas  

### 180 Dias (Fase 3)
- **MTTD < 15min** para incidentes críticos  
- **MTTR < 2h** para incidentes comuns  
- Equipe treinada em procedimentos de IR  
- Primeira simulação de incidente realizada  

---

## Recomendações Finais

### Comprometimento da Alta Direção
- A segurança deve ser tratada como prioridade estratégica  
- Alocação adequada de recursos humanos e financeiros  

### Cultura de Segurança
- Envolvimento de todas as áreas da empresa  
- Treinamentos regulares e conscientização contínua  

### Melhoria Contínua
- Revisão periódica do programa de segurança  
- Ajustes baseados em métricas e lições aprendidas  

### Preparação para Crescimento
- Arquitetura escalável para suportar expansão  
- Planejamento para futuras integrações em nuvem  

---

## Contato e Acompanhamento

- **Próxima Revisão Formal:** 30 dias após implementação inicial  
- **Métricas de Acompanhamento:** Relatório quinzenal de progresso  
- **Canal de Comunicação:** Reuniões semanais de alinhamento  

**Ponto Focal:**  
Adriel Shimaski  
[inserir email de contato]  
[inserir telefone para emergências]  

---

## 📋 Checklist de Início Rápido

### Antes de Começar
- [ ] Obter aprovação formal da diretoria  
- [ ] Designar equipe do projeto  
- [ ] Estabelecer canal de comunicação  
- [ ] Definir reuniões de acompanhamento  

### Primeira Semana
- [ ] Configurar acesso às ferramentas  
- [ ] Iniciar configuração do firewall  
- [ ] Agendar treinamentos da equipe  
- [ ] Documentar procedimentos básicos  

### Primeiro Mês
- [ ] Completar implementação da Fase 1  
- [ ] Realizar primeiro teste de backup  
- [ ] Configurar alertas básicos  
- [ ] Documentar lições aprendidas  
