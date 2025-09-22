# Fluxo de Inscrições 📝

Este diagrama representa o fluxo completo do processo de inscrições, desde o interesse inicial do aluno até a finalização do processo.

```mermaid
flowchart TD
    %% Definindo estilos com cores pastel
    classDef startEnd fill:#FFE4E1,stroke:#FF69B4,stroke-width:2px,color:#000
    classDef decision fill:#FFB6C1,stroke:#FF1493,stroke-width:2px,color:#000
    classDef process fill:#E0FFE0,stroke:#32CD32,stroke-width:2px,color:#000
    classDef integration fill:#E6E6FA,stroke:#9370DB,stroke-width:2px,color:#000
    classDef warning fill:#FFF8DC,stroke:#DAA520,stroke-width:2px,color:#000
    
    %% Início do fluxo
    START["🎯 Aluno se interessa pela bolsa<br/>(CTA - Quero essa bolsa)"]:::startEnd
    
    %% Subgrafo para processo inicial
    subgraph INITIAL ["📋 Processo Inicial"]
        D1{"🤔 Dados do aluno<br/>estão completos?"}:::decision
        FORM["📝 Preencher<br/>formulário"]:::process
        VALIDATE["✅ Validar<br/>dados"]:::process
    end
    
    %% Subgrafo para verificações
    subgraph VERIFICATION ["🔍 Verificações"]
        D2{"📊 Atende aos<br/>critérios?"}:::decision
        D3{"💰 Possui<br/>bolsa ativa?"}:::decision
        D4{"🎓 Curso<br/>disponível?"}:::decision
    end
    
    %% Subgrafo para integrações
    subgraph INTEGRATIONS ["🔗 Integrações"]
        INT1["🏫 Integração<br/>Estácio"]:::integration
        INT2["📚 Integração<br/>Kroton"]:::integration
        LEAD1["👤 Envio Lead<br/>Estácio"]:::integration
        LEAD2["👤 Envio Lead<br/>Kroton"]:::integration
    end
    
    %% Subgrafo para processamento
    subgraph PROCESSING ["⚙️ Processamento"]
        PROC1["📤 Processar<br/>solicitação"]:::process
        PROC2["📨 Gerar<br/>protocolo"]:::process
        PROC3["🔔 Enviar<br/>notificação"]:::process
    end
    
    %% Subgrafo para finalização
    subgraph FINALIZATION ["✅ Finalização"]
        D5{"📋 Inscrição<br/>aprovada?"}:::decision
        SUCCESS["🎉 Sucesso<br/>Inscrição realizada"]:::process
        REJECT["❌ Rejeição<br/>Critérios não atendidos"]:::warning
        CONTACT["📞 Contato<br/>para esclarecimentos"]:::process
    end
    
    %% Final
    END["🏁 Fim"]:::startEnd
    
    %% Fluxo principal
    START --> D1
    D1 -->|❌ Não| FORM
    D1 -->|✅ Sim| VALIDATE
    FORM --> VALIDATE
    VALIDATE --> D2
    
    D2 -->|❌ Não| REJECT
    D2 -->|✅ Sim| D3
    
    D3 -->|❌ Não| D4
    D3 -->|✅ Sim| CONTACT
    
    D4 -->|❌ Não| CONTACT
    D4 -->|✅ Sim| INT1
    D4 -->|✅ Sim| INT2
    
    INT1 --> LEAD1
    INT2 --> LEAD2
    
    LEAD1 --> PROC1
    LEAD2 --> PROC1
    
    PROC1 --> PROC2
    PROC2 --> PROC3
    PROC3 --> D5
    
    D5 -->|✅ Sim| SUCCESS
    D5 -->|❌ Não| REJECT
    
    SUCCESS --> END
    REJECT --> END
    CONTACT --> END
    
    %% Estilos dos subgrafos
    style INITIAL fill:#F0F8FF,stroke:#4169E1,stroke-width:2px
    style VERIFICATION fill:#F5FFFA,stroke:#2E8B57,stroke-width:2px
    style INTEGRATIONS fill:#FFF5EE,stroke:#FF4500,stroke-width:2px
    style PROCESSING fill:#F8F8FF,stroke:#6A5ACD,stroke-width:2px
    style FINALIZATION fill:#FFFACD,stroke:#DAA520,stroke-width:2px
```

## 📝 Descrição do Fluxo

### 🎯 Início
O processo inicia quando o aluno demonstra interesse pela bolsa através do CTA "Quero essa bolsa".

### 📋 Processo Inicial
1. **Verificação de dados**: Verifica se os dados do aluno estão completos
2. **Preenchimento de formulário**: Caso necessário, solicita preenchimento de dados faltantes
3. **Validação**: Valida os dados fornecidos

### 🔍 Verificações
- **Critérios**: Verifica se o aluno atende aos critérios estabelecidos
- **Bolsa ativa**: Confirma se não possui bolsa ativa
- **Disponibilidade do curso**: Verifica se o curso está disponível

### 🔗 Integrações
O sistema integra com:
- **Estácio**: Envio de leads para a instituição Estácio
- **Kroton**: Envio de leads para a instituição Kroton

### ⚙️ Processamento
1. **Processamento da solicitação**: Processa os dados da inscrição
2. **Geração de protocolo**: Cria protocolo de acompanhamento
3. **Envio de notificação**: Notifica o aluno sobre o status

### ✅ Finalização
- **Aprovação**: Inscrição aprovada com sucesso
- **Rejeição**: Inscrição rejeitada por não atender critérios
- **Contato**: Necessidade de esclarecimentos adicionais

### 🏁 Fim
Término do processo de inscrição.

---

*Este fluxo representa o processo completo de inscrições, garantindo que todas as etapas sejam seguidas corretamente e que as integrações funcionem adequadamente.*
