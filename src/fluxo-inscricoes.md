# Fluxo de Inscrições - Sistema Integrado QueroEdu

## Descrição do Processo

Este diagrama representa o **fluxo completo de inscrições** do ecossistema QueroEdu, desde o interesse inicial do aluno até a finalização da matrícula ou captação de leads. O processo contempla múltiplas modalidades de integração com diferentes Instituições de Ensino Superior (IES), incluindo integrações diretas via API, processamento por crawler e envios manuais.

### Principais Características:

- **🎯 Processo Unificado**: Consolida diferentes jornadas do aluno em um fluxo único
- **🔄 Integrações Automatizadas**: APIs diretas com instituições parceiras
- **📊 Controle de Status**: Rastreamento completo do ciclo de vida das inscrições
- **🤖 Processamento em Lote**: Jobs automatizados para sincronização e envio de dados
- **📋 Conformidade LGPD**: Integração obrigatória com plataformas de compliance

### Integrações Contempladas:

#### 🏫 Integração Kroton
Para detalhes completos sobre a integração Kroton, consulte: [Kroton Lead Integration](kroton-lead-integration.md)
- **Modalidades**: Presencial e Semi-presencial 
- **Tecnologia**: API REST + OAuth2 + Elasticsearch
- **Características**: Rate limiting (100 req/5min), sincronização de cursos automática
- **Jobs**: `sync_course`, populamento de BD, envio automático de dados

#### 🎓 Integração Estácio
Para detalhes completos sobre a integração Estácio, consulte: [Estácio Lead Integration](estacio-lead-integration.md)
- **Compliance**: Integração obrigatória com OneTrust (LGPD)
- **Tecnologia**: API Direta + OneTrust
- **Características**: Processamento em chunks, retry automático
- **Jobs**: Sync LGPD (a cada 2h), registro de inscrições (10h-14h UTC)

#### 🤖 Integração via Crawler
- **IES Atendidas**: Belas Artes, Kroton Pós, FMU, Anima (Presencial e EaD)
- **Tecnologia**: Bot automatizado único para todas as IES
- **Processo**: Populamento do banco `subscribe_bot` + envio automatizado

### Fluxos de Processo:

1. **💳 Fluxo PEF (Pagamento)**: Processo completo com admissão digital e validação de documentos
2. **🔄 Fluxo de Integração**: Direcionamento baseado no tipo de integração disponível
3. **📝 Fluxo de Captação**: Geração e envio de leads para IES parceiras
4. **⚠️ Fluxo de Erro**: Tratamento e reenvio automático em caso de falhas

```mermaid
%%{init: {
    "theme": "default",
    "themeVariables": {
        "primaryColor": "#ffffff",
        "primaryTextColor": "#000000",
        "primaryBorderColor": "#666666",
        "lineColor": "#333333",
        "sectionBkgColor": "#f8f9fa",
        "altSectionBkgColor": "#e9ecef",
        "gridColor": "#cccccc"
    }
}}%%

flowchart TD
    %% 💬 Green comment nodes based on the image
    COMMENT1@{ shape: comment, label: "🔄 Essa rotina roda a cada 3h pegando, do BD da IES, os dias de aula presencial dos cursos Semi" }
    COMMENT2@{ shape: comment, label: "📊 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição" }
    COMMENT3@{ shape: comment, label: "📤 Cron job que roda script que envia os alunos para a IES. Frequência: a cada 3h" }
    COMMENT4@{ shape: comment, label: "💾 Sistema de persistência de dados" }
    COMMENT5@{ shape: comment, label: "🚀 Processo de envio automático" }
    COMMENT6@{ shape: comment, label: "✅ Verificação de status do aluno" }
    COMMENT7@{ shape: comment, label: "💿 Backup de dados de inscrição" }
    COMMENT8@{ shape: comment, label: "🔐 Integração com plataforma LGPD" }
    COMMENT9@{ shape: comment, label: "📋 Envio de dados específicos da IES" }
    COMMENT10@{ shape: comment, label: "🎯 Sistema de captação de leads" }

    %% 🟡 START/END nodes
    START(["🚀 Início do Processo"])
    END1(["✅ Processo Finalizado"])
    END2(["💡 Lead Capturado"])
    
    %% 🌸 Main decision points
    PAYMENT_CHECK{"💳 Pagamento aprovado?"}
    DIGITAL_ADMISSION{"📱 Admissão Digital?"}
    API_CHECK{"🔌 Tem API?"}
    DOC_CHECK{"📋 Docs OK?"}
    INTEGRATION_TYPE{"🔄 Tipo Integração?"}
    API_INTEGRATION{"🔌 API Integration?"}
    KROTON_CHECK{"🏫 É Kroton?"}
    KROTON_MODE{"📚 Modalidade?"}
    KROTON_ERROR_1{"⚠️ Erro no envio?"}
    ESTACIO_ERROR{"⚠️ Erro Estácio?"}
    CONFIG_CHECK{"⚙️ Config Admissão?"}
    ADMISSION_CHECK{"❓ admission_created?"}
    ENROLL_CHECK{"📝 admission_enroll?"}

    %% ⚪ Process nodes
    CADASTRO["📋 Cadastro do Aluno"]
    SEND_DOCS["📤 Envio de Documentos"]
    REJECT_DOCS["❌ Rejeitar Documentos"]
    ENROLL_DATA["✏️ Dados de Matrícula"]
    STUDENT_ENROLLED["🎓 Aluno Matriculado"]
    VOUCHER["📄 Comprovante de Bolsa"]
    MANUAL_ENROLL["🏢 Matrícula Presencial"]
    
    %% Contract process
    SIGN_CONTRACT1["✍️ Assinar Contrato"]
    SEND_DOCS1["📤 Enviar Documentos"]
    SELECTION_PROCESS1["🎯 Processo Seletivo"]
    SIGN_CONTRACT2["✍️ Assinar Contrato 2"]
    SEND_DOCS2["📤 Enviar Documentos 2"]
    SELECTION_PROCESS2["🎯 Processo Seletivo 2"]
    
    %% Kroton Integration
    SYNC_COURSE["⏰ Cron Job 'sync_course'"]
    POPULATE_BD1["💾 Popular BD Inscrições"]
    SEND_STUDENT_DATA1["📤 Enviar Dados Aluno"]
    POPULATE_BD2["💾 Popular BD Inscrições 2"]
    SEND_STUDENT_DATA2["📤 Enviar Dados Aluno 2"]
    AUTO_RESEND["🔄 Reenvio Automático"]
    
    %% Estácio Integration
    POPULATE_BD3["💾 Popular BD Inscrições 3"]
    SEND_ONETRUST["🔐 Enviar para Onetrust"]
    SEND_STUDENT_DATA3["📤 Enviar Dados Aluno 3"]
    IES_NOTIFY["📞 IES Notifica"]
    MANUAL_SEND["✋ Envio Manual"]
    
    %% Lead Capture
    POPULATE_LEADS1["💾 Popular BD - type captação"]
    SEND_LEADS1["📤 Enviar Leads"]
    POPULATE_LEADS2["💾 Popular BD - codAgentPdv"]
    SEND_ONETRUST2["🔐 Enviar Lead Onetrust"]
    SEND_LEADS2["📤 Enviar Dados Leads"]
    POPULATE_BOT["💾 Popular subscribe_bot"]
    SEND_LEAD_IES["📤 Enviar Lead para IES"]

    %% Main Flow
    START --> CADASTRO
    CADASTRO --> PAYMENT_CHECK
    
    %% Payment Flow (YES)
    PAYMENT_CHECK -->|Sim| DIGITAL_ADMISSION
    DIGITAL_ADMISSION -->|Sim| API_CHECK
    API_CHECK -->|Sim| SEND_DOCS
    SEND_DOCS --> DOC_CHECK
    DOC_CHECK -->|Sim| ENROLL_DATA
    DOC_CHECK -->|Não| REJECT_DOCS
    REJECT_DOCS --> ENROLL_DATA
    ENROLL_DATA --> STUDENT_ENROLLED
    STUDENT_ENROLLED --> VOUCHER
    VOUCHER --> END1
    
    %% No Payment Flow
    PAYMENT_CHECK -->|Não| INTEGRATION_TYPE
    
    %% Config and Admission Flow
    API_CHECK -->|Não| CONFIG_CHECK
    CONFIG_CHECK -->|Não| ADMISSION_CHECK
    ADMISSION_CHECK -->|Sim| SIGN_CONTRACT1
    SIGN_CONTRACT1 --> SEND_DOCS1
    SEND_DOCS1 --> SELECTION_PROCESS1
    SELECTION_PROCESS1 --> ENROLL_DATA
    ADMISSION_CHECK -->|Não| ENROLL_DATA
    
    CONFIG_CHECK -->|Sim| ENROLL_CHECK
    ENROLL_CHECK -->|Sim| SIGN_CONTRACT2
    ENROLL_CHECK -->|Não| SELECTION_PROCESS2
    SIGN_CONTRACT2 --> SEND_DOCS2
    SEND_DOCS2 --> SELECTION_PROCESS2
    SELECTION_PROCESS2 --> ENROLL_DATA
    
    %% Non-Digital Admission
    DIGITAL_ADMISSION -->|Não| API_INTEGRATION
    API_INTEGRATION -->|Kroton| KROTON_CHECK
    KROTON_CHECK -->|Semipresencial| SYNC_COURSE
    SYNC_COURSE --> POPULATE_BD1
    POPULATE_BD1 --> SEND_STUDENT_DATA1
    SEND_STUDENT_DATA1 --> KROTON_ERROR_1
    KROTON_ERROR_1 -->|Sim| AUTO_RESEND
    KROTON_ERROR_1 -->|Não| IES_NOTIFY
    AUTO_RESEND --> IES_NOTIFY
    
    KROTON_CHECK -->|Presencial| POPULATE_BD2
    POPULATE_BD2 --> SEND_STUDENT_DATA2
    SEND_STUDENT_DATA2 --> KROTON_ERROR_1
    
    API_INTEGRATION -->|Estácio| POPULATE_BD3
    POPULATE_BD3 --> SEND_ONETRUST
    SEND_ONETRUST --> SEND_STUDENT_DATA3
    SEND_STUDENT_DATA3 --> ESTACIO_ERROR
    ESTACIO_ERROR -->|Sim| MANUAL_SEND
    ESTACIO_ERROR -->|Não| IES_NOTIFY
    MANUAL_SEND --> IES_NOTIFY
    IES_NOTIFY --> MANUAL_ENROLL
    MANUAL_ENROLL --> END1
    
    %% Integration Types
    INTEGRATION_TYPE --> API_INTEGRATION
    API_INTEGRATION -->|Sim| KROTON_CHECK
    KROTON_CHECK -->|Kroton| POPULATE_LEADS2
    POPULATE_LEADS2 --> SEND_ONETRUST2
    SEND_ONETRUST2 --> SEND_LEADS2
    SEND_LEADS2 --> END2
    
    KROTON_CHECK -->|Estácio| POPULATE_LEADS1
    POPULATE_LEADS1 --> SEND_LEADS1
    SEND_LEADS1 --> END2
    
    API_INTEGRATION -->|Crawler| POPULATE_BOT
    POPULATE_BOT --> SEND_LEAD_IES
    SEND_LEAD_IES --> END2
    
    END2 --> END1

    %% Comment connections
    COMMENT1 -.-> SYNC_COURSE
    COMMENT2 -.-> POPULATE_BD1
    COMMENT3 -.-> SEND_STUDENT_DATA1
    COMMENT4 -.-> POPULATE_BD2
    COMMENT5 -.-> SEND_STUDENT_DATA2
    COMMENT6 -.-> AUTO_RESEND
    COMMENT7 -.-> POPULATE_BD3
    COMMENT8 -.-> SEND_ONETRUST
    COMMENT9 -.-> SEND_STUDENT_DATA3
    COMMENT10 -.-> POPULATE_LEADS1

    %% Styling
    classDef pinkDecision fill:#ffb3d9,stroke:#ff69b4,stroke-width:2px,color:#000000
    classDef yellowStartEnd fill:#fff2cc,stroke:#d6b656,stroke-width:2px,color:#000000
    classDef greenComment fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#2d5a3d
    classDef greyAction fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000

    class PAYMENT_CHECK,DIGITAL_ADMISSION,API_CHECK,DOC_CHECK,INTEGRATION_TYPE,API_INTEGRATION,KROTON_CHECK,KROTON_MODE,KROTON_ERROR_1,ESTACIO_ERROR,CONFIG_CHECK,ADMISSION_CHECK,ENROLL_CHECK pinkDecision
    class START,END1,END2 yellowStartEnd
    class COMMENT1,COMMENT2,COMMENT3,COMMENT4,COMMENT5,COMMENT6,COMMENT7,COMMENT8,COMMENT9,COMMENT10 greenComment
    class CADASTRO,SEND_DOCS,REJECT_DOCS,ENROLL_DATA,STUDENT_ENROLLED,VOUCHER,MANUAL_ENROLL,SIGN_CONTRACT1,SEND_DOCS1,SELECTION_PROCESS1,SIGN_CONTRACT2,SEND_DOCS2,SELECTION_PROCESS2,SYNC_COURSE,POPULATE_BD1,SEND_STUDENT_DATA1,POPULATE_BD2,SEND_STUDENT_DATA2,AUTO_RESEND,POPULATE_BD3,SEND_ONETRUST,SEND_STUDENT_DATA3,IES_NOTIFY,MANUAL_SEND,POPULATE_LEADS1,SEND_LEADS1,POPULATE_LEADS2,SEND_ONETRUST2,SEND_LEADS2,POPULATE_BOT,SEND_LEAD_IES greyAction
```

---

## Referências Técnicas

- **[Kroton Lead Integration](kroton-lead-integration.md)**: Documentação completa da integração com APIs Kroton, incluindo OAuth2, Elasticsearch e processamento de matrículas
- **[Estácio Lead Integration](estacio-lead-integration.md)**: Documentação detalhada da integração Estácio com compliance LGPD via OneTrust
- **Databricks**: Importação diária de dados de alunos e ordens
- **APIs de Terceiros**: Integrações diretas com sistemas das IES parceiras