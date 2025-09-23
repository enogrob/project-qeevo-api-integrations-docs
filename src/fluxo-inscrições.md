# Fluxo de Inscrições - Integração com IES

Este documento apresenta o fluxo completo de inscrições e integrações com as Instituições de Ensino Superior (IES), incluindo os processos para Kroton e Estácio.

## Visão Geral

O fluxo de inscrições gerencia todo o processo desde o interesse inicial do aluno até a efetivação da matrícula, incluindo diferentes tipos de integração (API e Crawler) e modalidades de ensino.

## Diagrama do Fluxo

```mermaid
flowchart TD
    %% Início do processo
    INICIO(["Aluno se interessa pela bolsa (CTA - Quero esta bolsa)"])
    FIM(["Fim"])
    LEAD(["Lead 'Vendido'"])
    
    %% Ações principais
    AC7["Cadastro (E-mail, CPF, Nome, Nascimento, Celular e CEP)"]
    AC8["Aluno envia documentos"]
    AC9["Rejeitar documentos"]
    AC10["Matricula com dados do aluno"]
    AC11["Aluno matriculado"]
    AC23["Aluno recebe comprovante da bolsa"]
    AC24["Matricula no balcão da IES"]
    
    %% Processo de contrato e documentos
    AC1["Assina o contrato"]
    AC2["Envio dos documentos"]
    AC3["Processo Seletivo"]
    AC4["Assina o contrato"]
    AC5["Envia dos documentos"]
    AC6["Processo Seletivo"]
    
    %% Processos de integração Kroton
    AC12["Cron Job 'sync_course'"]
    AC13["Popula BD de inscrições"]
    AC14["Envio dos dados do Aluno para IES"]
    AC15["Popula BD de inscrições"]
    AC16["Envio dos dados do Aluno para IES"]
    AC17["Reenvio dos dados automáticamente"]
    
    %% Processos de integração Estácio
    AC18["Popula BD de inscrições"]
    AC19["Envio dos dados do Aluno para Onetrust"]
    AC20["Envio dos dados do Aluno para IES"]
    AC21["IES nos avisa"]
    AC22["Envio manual"]
    
    %% Processos de captação/leads
    AC25["Popula BD de inscrições, mas separa com type captação"]
    AC26["Envio dos leads para IES"]
    AC27["Popula BD de inscrições, mas separa com codAgentPdv = 14412833"]
    AC28["Envia dados do lead para Onetrust"]
    AC29["Envio dos dados dos leads para IES"]
    AC30["Popula banco 'subscribe_bot'"]
    AC31["Envio do lead para a IES"]
    
    %% Decisões principais
    IF1{admission_created?}
    IF2{Config de Admissão?}
    IF3{admission_enroll?}
    IF4{Pagamento PEF?}
    IF5{Admissão Digital?}
    IF6{API de inscrições aluno?}
    IF7{Documentação correta?}
    IF8{Modalidade Kroton}
    IF9{Erro no envio?}
    IF10{API de inscrições aluno?}
    IF11{Estácio}
    IF12{Erro no envio?}
    IF13{Tipo de Integração}
    IF14{API}
    IF15{Kroton}
    IF16{Estácio}
    IF17{Crawler}
    
    %% Fluxo principal
    INICIO --> AC7
    AC7 --> IF4
    
    %% Fluxo PEF (Pagamento)
    IF4 -->|Sim| IF5
    IF5 -->|Sim| IF6
    IF6 -->|Sim| AC8
    AC8 --> IF7
    IF7 -->|Sim| AC10
    IF7 -->|Não| AC9
    AC9 --> AC10
    AC10 --> AC11
    AC11 --> AC23
    AC23 --> FIM
    
    %% Fluxo sem PEF
    IF4 -->|Não| IF13
    
    %% Configuração de admissão
    IF6 -->|Não| IF2
    IF2 -->|Não| IF1
    IF1 -->|Sim| AC1
    AC1 --> AC2
    AC2 --> AC3
    AC3 --> AC10
    IF1 -->|Não| AC10
    
    IF2 -->|Sim| IF3
    IF3 -->|Sim| AC4
    IF3 -->|Não| AC6
    AC4 --> AC5
    AC5 --> AC6
    AC6 --> AC10
    
    %% Admissão não digital
    IF5 -->|Não| IF10
    IF10 -->|Kroton| IF8
    IF8 -->|Semipresencial| AC12
    AC12 --> AC13
    AC13 --> AC14
    AC14 --> IF9
    IF9 -->|Sim| AC17
    IF9 -->|Não| AC21
    AC17 --> AC21
    
    IF8 -->|Presencial| AC15
    AC15 --> AC16
    AC16 --> IF9
    
    IF10 -->|Estácio| AC18
    AC18 --> AC19
    AC19 --> AC20
    AC20 --> IF12
    IF12 -->|Sim| AC22
    IF12 -->|Não| AC21
    AC22 --> AC21
    AC21 --> AC24
    AC24 --> FIM
    
    %% Tipos de integração
    IF13 --> IF14
    IF14 -->|Sim| IF15
    IF15 -->|Kroton| AC27
    AC27 --> AC28
    AC28 --> AC29
    AC29 --> LEAD
    
    IF15 -->|Estácio| AC25
    AC25 --> AC26
    AC26 --> LEAD
    
    IF14 -->|Crawler| IF17
    IF17 --> AC30
    AC30 --> AC31
    AC31 --> LEAD
    
    LEAD --> FIM
```

## Principais Componentes do Fluxo

### 1. Processo Inicial
- **Cadastro**: Coleta de dados básicos do aluno (E-mail, CPF, Nome, Nascimento, Celular e CEP)
- **Verificação PEF**: Determina se há pagamento PEF envolvido

### 2. Fluxos de Admissão
- **Admissão Digital**: Processo online com API de inscrições
- **Admissão Tradicional**: Processo com diferentes configurações baseadas em admission_created e admission_enroll

### 3. Integrações por IES

#### Kroton
- **Modalidade Semipresencial**: Utiliza Cron Job 'sync_course' e população do BD
- **Modalidade Presencial**: Processo direto de envio de dados
- **Tratamento de Erros**: Reenvio automático em caso de falha

#### Estácio
- **Integração com Onetrust**: Envio de dados para plataforma intermediária
- **Envio para IES**: Comunicação direta com a instituição
- **Fallback Manual**: Envio manual em caso de erro

### 4. Tipos de Integração
- **API**: Integração automática via interfaces de programação
- **Crawler**: Integração via automação web para captação de leads

### 5. Gestão de Leads
- Separação por tipo de captação
- Códigos específicos para diferentes agentes (codAgentPdv = 14412833)
- População de banco subscribe_bot para gestão de leads

## Pontos de Decisão Importantes

1. **Pagamento PEF**: Determina o fluxo principal vs. fluxo de integração
2. **Admissão Digital**: Define se usa API ou processo tradicional
3. **Modalidade de Ensino**: Influencia o processo de integração (Kroton)
4. **Documentação**: Validação para aprovação ou rejeição
5. **Tratamento de Erros**: Mecanismos de reenvio automático e manual

## Resultados Finais

O fluxo pode terminar em três estados principais:
1. **Aluno Matriculado**: Processo completo de admissão digital
2. **Matrícula Presencial**: Processo que requer presença na IES
3. **Lead Vendido**: Processo de captação para posterior conversão

---

*Este diagrama representa o fluxo atual das integrações com IES e pode ser atualizado conforme novas funcionalidades são implementadas.*
