# Plano de Ação - Implementação Segurança Loja Zeta

## Visão Geral do Projeto

**Duração Total**: 180 dias (6 meses)  
**Fases**: 3 (Fundamentos, Monitoramento, Otimização)  
**Equipe**: 2 Devs + 1 Ops + Consultor externo  
**Orçamento**: R$ 15.000 (fase inicial) + R$ 1.300/mês (recorrente)

---

## Fase 1: Fundamentos (Dias 1-30)

### Semana 1: Preparação e Planejamento

**Objetivo**: Estabelecer base do projeto e configurações iniciais

| Data      | Atividade              | Responsável | Status | Entregável          |
|----------|-----------------------|------------|--------|-------------------|
| Dia 1-2  | Kick-off do projeto    | Gerente    | [ ]    | Acta de kick-off   |
| Dia 3-4  | Análise ambiente atual | Ops        | [ ]    | Relatório de baseline |
| Dia 5    | Definição de métricas  | Todos      | [ ]    | KPIs documentados  |

**Checklist Semana 1**:
- [ ] Ambiente de desenvolvimento configurado
- [ ] Acesso a todos os sistemas concedido
- [ ] Documentação atualizada
- [ ] Cronograma detalhado aprovado

---

### Semana 2-3: Configuração de Rede

**Objetivo**: Implementar segmentação e controles básicos de rede

| Atividade           | Complexidade | Prazo | Dependências       |
|--------------------|-------------|-------|------------------|
| Configurar VLANs    | Média       | 5 dias | Diagrama de rede  |
| Implementar firewall| Alta        | 7 dias | Regras de negócio |
| Isolar rede visitantes | Baixa   | 3 dias | -                |

**Riscos Identificados**:
- Impacto na conectividade durante implementação
- Necessidade de rollback planejado

---

### Semana 4: WAF e Proteção Básica

**Objetivo**: Ativar proteções perimetrais

```bash
# Comandos de implementação WAF
sudo apt install nginx-module-security
sudo cp owasp-crs.conf /etc/nginx/conf.d/
sudo nginx -t && sudo systemctl reload nginx
```

# Testes a Realizar

- WAF bloqueia payloads SQLi  
- Rate limiting funciona  
- Logs são gerados corretamente  

---

# Fase 2: Monitoramento (Dias 31-90)

## Mês 2: SIEM e Centralização de Logs

### Atividades Principais

### Instalação Wazuh (Dias 31-45)

```bash
# Instalação servidor Wazuh
curl -sO https://packages.wazuh.com/4.5/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

## Configuração Agentes (Dias 46-60)

- Agente no servidor web  
- Agente no banco de dados  
- Configuração de regras personalizadas  

---

## Alertas e Dashboards (Dias 61-90)

- Configuração de alertas por email  
- Dashboards Grafana para segurança  
- Treinamento da equipe  

---

## Métricas de Sucesso Fase 2

- 80% dos logs centralizados  
- Alertas funcionando para ameaças críticas  
- Equipe treinada no uso do SIEM  

---

## Fase 3: Otimização (Dias 91-180)

### Mês 4-5: Automação e Melhorias

**Objetivos:**
- Automação de respostas a incidentes  
- Otimização de performance  
- Revisão de políticas  

**Atividades:**
- Desenvolvimento de scripts de automação  
- Análise de performance e ajustes  
- Revisão e atualização de documentação  

### Mês 6: Validação e Entrega

**Atividades Finais:**
- Testes de penetração internos  
- Simulação de incidente  
- Documentação final do projeto  
- Treinamento de handover  

---

## Matriz de Riscos e Mitigações

### Riscos Técnicos

| Risco                     | Probabilidade | Impacto | Mitigação                |
|----------------------------|---------------|---------|--------------------------|
| Configuração incorreta WAF | Alta          | Alto    | Testes em homologação    |
| Indisponibilidade durante implantação | Média | Alto | Janela de manutenção     |
| Performance impactada      | Baixa         | Médio   | Monitoramento contínuo   |

### Riscos Operacionais

| Risco                     | Probabilidade | Impacto | Mitigação                   |
|----------------------------|---------------|---------|-----------------------------|
| Resistência da equipe       | Média         | Médio   | Treinamento e comunicação   |
| Falta de recursos           | Baixa         | Alto    | Planejamento contingencial  |
| Mudanças de escopo          | Baixa         | Médio   | Controle de mudanças        |

----

### Cronograma Detalhado

![alt text](/imagens/conos.png)

