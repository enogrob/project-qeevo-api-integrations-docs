
```mermaid
flowchart TD
    
    %% Green comment nodes based on the image
    COMMENT1@{ shape: comment, label: "Processo de matrícula padrão do QueroEdu" }
    COMMENT2@{ shape: comment, label: "Essa rotina roda a cada 3h pegando, do BD da IES, os dias de aula presencial dos cursos Semi." }
    COMMENT3@{ shape: comment, label: "Validação automática de documentos enviados pelo aluno" }
    COMMENT4@{ shape: comment, label: "Sistema de controle de erros e reenvio automático" }
    COMMENT5@{ shape: comment, label: "Processo de captação de leads para parceiros" }
    COMMENT6@{ shape: comment, label: "Sincronização de dados entre sistemas" }
    COMMENT7@{ shape: comment, label: "Verificação de integridade dos dados enviados" }
    
    subgraph SG1 ["🎯 Student Journey"]
        INICIO(("`🚀 **Início**
        Aluno interessado
        na bolsa`"))
        CADASTRO["`📝 **Cadastro**
        E-mail, CPF, Nome
        Nascimento, Celular, CEP`"]
        FIM(("`✅ **Fim**
        Processo Concluído`"))
        LEAD_SOLD(("`💰 **Lead Vendido**
        Conversão realizada`"))
    end
    
    subgraph SG2 ["💳 Payment & Admission Process"]
        PAYMENT_CHECK{"`💳 **Pagamento PEF?**
        Processo de pagamento`"}
        DIGITAL_ADMISSION{"`💻 **Admissão Digital?**
        Processo automatizado`"}
        API_CHECK{"`🔌 **API de Inscrições?**
        Integração disponível`"}
        
        subgraph SG3 ["📋 Document Management"]
            SEND_DOCS["`📄 **Enviar Documentos**
            Upload de arquivos`"]
            DOC_CHECK{"`✅ **Documentação OK?**
            Validação de documentos`"}
            REJECT_DOCS["`❌ **Rejeitar Docs**
            Documentos inválidos`"]
        end
        
        subgraph SG4 ["🎓 Enrollment Process"]
            ENROLL_DATA["`📊 **Matrícula com Dados**
            Processamento de inscrição`"]
            STUDENT_ENROLLED["`🎉 **Aluno Matriculado**
            Sucesso na matrícula`"]
            VOUCHER["`🎫 **Comprovante Bolsa**
            Documento de confirmação`"]
            CAMPUS_ENROLL["`🏛️ **Matrícula no Campus**
            Processo presencial`"]
        end
    end
    
    subgraph SG5 ["⚙️ Integration Systems"]
        INTEGRATION_TYPE{"`🔧 **Tipo de Integração**
        Sistema de destino`"}
        API_INTEGRATION{"`🔌 **API?**
        Integração automatizada`"}
        
        subgraph SG6 ["🔄 Kroton Integration"]
            KROTON_CHECK{"`🎓 **Kroton?**
            Sistema Kroton`"}
            KROTON_MODE{"`📚 **Modalidade Kroton**
            Tipo de curso`"}
            
            SYNC_COURSE["`🔄 **Cron Job sync_course**
            Sincronização de cursos`"]
            KROTON_BD_1["`🗄️ **Popula BD**
            Inscrições Kroton`"]
            KROTON_SEND_1["`📡 **Envio para IES**
            Dados do aluno`"]
            KROTON_ERROR_1{"`⚠️ **Erro no Envio?**
            Falha na transmissão`"}
            KROTON_RETRY["`🔄 **Reenvio Automático**
            Tentativa automática`"]
            
            KROTON_BD_2["`🗄️ **Popula BD**
            Inscrições presenciais`"]
            KROTON_SEND_2["`📡 **Envio para IES**
            Modalidade presencial`"]
        end
        
        subgraph SG7 ["🏛️ Estácio Integration"]
            ESTACIO_BD["`🗄️ **Popula BD**
            Inscrições Estácio`"]
            ONETRUST["`🛡️ **OneTrust**
            Compliance LGPD`"]
            ESTACIO_SEND["`📡 **Envio para IES**
            Sistema Estácio`"]
            ESTACIO_ERROR{"`⚠️ **Erro no Envio?**
            Falha na transmissão`"}
            MANUAL_SEND["`✋ **Envio Manual**
            Intervenção necessária`"]
        end
        
        subgraph SG8 ["🕷️ Crawler Integration"]
            CRAWLER_BD["`🗄️ **BD Subscribe Bot**
            Sistema de captura`"]
            CRAWLER_SEND["`📡 **Envio para IES**
            Via crawler`"]
        end
        
        subgraph SG9 ["📊 Lead Management"]
            LEAD_BD_KROTON["`🗄️ **BD Inscrições**
            Type: captação`"]
            LEAD_ONETRUST["`🛡️ **OneTrust Lead**
            LGPD para leads`"]
            LEAD_SEND_KROTON["`📡 **Envio Lead**
            codAgentPdv: 14412833`"]
            
            LEAD_BD_ESTACIO["`🗄️ **BD Inscrições**
            Separação por tipo`"]
            LEAD_SEND_ESTACIO["`📡 **Envio Lead**
            Sistema Estácio`"]
        end
    end
    
    subgraph SG10 ["🔧 Legacy Process"]
        CONFIG_CHECK{"`⚙️ **Config Admissão?**
        Configuração disponível`"}
        ADMISSION_CHECK{"`🎯 **admission_created?**
        Processo criado`"}
        ENROLL_CHECK{"`📋 **admission_enroll?**
        Matrícula disponível`"}
        
        CONTRACT_1["`📋 **Assinar Contrato**
        Primeira assinatura`"]
        SEND_DOCS_1["`📄 **Envio Documentos**
        Primeira remessa`"]
        SELECTIVE_1["`🎯 **Processo Seletivo**
        Primeira avaliação`"]
        
        CONTRACT_2["`📋 **Assinar Contrato**
        Segunda assinatura`"]
        SEND_DOCS_2["`📄 **Envio Documentos**
        Segunda remessa`"]
        SELECTIVE_2["`🎯 **Processo Seletivo**
        Segunda avaliação`"]
    end
    
    %% Main Flow
    INICIO --> CADASTRO
    CADASTRO --> PAYMENT_CHECK
    
    %% Payment Flow
    PAYMENT_CHECK -->|"`💳 **Sim**
    Processo PEF`"| DIGITAL_ADMISSION
    PAYMENT_CHECK -->|"`❌ **Não**
    Sem pagamento`"| INTEGRATION_TYPE
    
    %% Digital Admission Flow
    DIGITAL_ADMISSION -->|"`💻 **Sim**
    Processo digital`"| API_CHECK
    DIGITAL_ADMISSION -->|"`📋 **Não**
    Processo manual`"| API_CHECK
    
    API_CHECK -->|"`🔌 **Sim**
    API disponível`"| SEND_DOCS
    API_CHECK -->|"`❌ **Não**
    Sem API`"| CONFIG_CHECK
    
    %% Document Management Flow
    SEND_DOCS --> DOC_CHECK
    DOC_CHECK -->|"`✅ **Sim**
    Documentos OK`"| ENROLL_DATA
    DOC_CHECK -->|"`❌ **Não**
    Documentos inválidos`"| REJECT_DOCS
    REJECT_DOCS --> ENROLL_DATA
    
    %% Enrollment Flow
    ENROLL_DATA --> STUDENT_ENROLLED
    STUDENT_ENROLLED --> VOUCHER
    VOUCHER --> FIM
    
    %% Legacy Configuration Flow
    CONFIG_CHECK -->|"`❌ **Não**
    Sem configuração`"| ADMISSION_CHECK
    CONFIG_CHECK -->|"`✅ **Sim**
    Com configuração`"| ENROLL_CHECK
    
    ADMISSION_CHECK -->|"`✅ **Sim**
    Admissão criada`"| CONTRACT_1
    ADMISSION_CHECK -->|"`❌ **Não**
    Sem admissão`"| ENROLL_DATA
    
    CONTRACT_1 --> SEND_DOCS_1
    SEND_DOCS_1 --> SELECTIVE_1
    SELECTIVE_1 --> ENROLL_DATA
    
    ENROLL_CHECK -->|"`✅ **Sim**
    Matrícula habilitada`"| CONTRACT_2
    ENROLL_CHECK -->|"`❌ **Não**
    Matrícula direta`"| SELECTIVE_2
    
    CONTRACT_2 --> SEND_DOCS_2
    SEND_DOCS_2 --> SELECTIVE_2
    SELECTIVE_2 --> ENROLL_DATA
    
    %% Integration Flow
    INTEGRATION_TYPE --> API_INTEGRATION
    API_INTEGRATION -->|"`🔌 **Sim**
    API disponível`"| KROTON_CHECK
    API_INTEGRATION -->|"`🕷️ **Crawler**
    Captura automatizada`"| CRAWLER_BD
    
    %% Kroton Integration Flow
    KROTON_CHECK -->|"`🎓 **Kroton**
    Sistema Kroton`"| KROTON_BD_1
    KROTON_CHECK -->|"`🏛️ **Estácio**
    Sistema Estácio`"| ESTACIO_BD
    
    KROTON_BD_1 --> LEAD_ONETRUST
    LEAD_ONETRUST --> LEAD_SEND_KROTON
    LEAD_SEND_KROTON --> LEAD_SOLD
    
    ESTACIO_BD --> LEAD_BD_ESTACIO
    LEAD_BD_ESTACIO --> LEAD_SEND_ESTACIO
    LEAD_SEND_ESTACIO --> LEAD_SOLD
    
    %% Crawler Flow
    CRAWLER_BD --> CRAWLER_SEND
    CRAWLER_SEND --> LEAD_SOLD
    
    %% Legacy Kroton Flow for non-API
    API_CHECK -->|"`❌ **Não**
    Kroton sem API`"| KROTON_MODE
    KROTON_MODE -->|"`📚 **Semipresencial**
    Modalidade híbrida`"| SYNC_COURSE
    KROTON_MODE -->|"`🏛️ **Presencial**
    Modalidade campus`"| KROTON_BD_2
    
    SYNC_COURSE --> KROTON_BD_1
    KROTON_BD_1 --> KROTON_SEND_1
    KROTON_SEND_1 --> KROTON_ERROR_1
    KROTON_ERROR_1 -->|"`✅ **Não**
    Sucesso`"| IES_NOTIFICATION
    KROTON_ERROR_1 -->|"`⚠️ **Sim**
    Erro ocorrido`"| KROTON_RETRY
    KROTON_RETRY --> IES_NOTIFICATION
    
    KROTON_BD_2 --> KROTON_SEND_2
    KROTON_SEND_2 --> KROTON_ERROR_1
    
    %% Estácio Flow for non-API
    API_CHECK -->|"`❌ **Não**
    Estácio sem API`"| ESTACIO_BD
    ESTACIO_BD --> ONETRUST
    ONETRUST --> ESTACIO_SEND
    ESTACIO_SEND --> ESTACIO_ERROR
    ESTACIO_ERROR -->|"`✅ **Não**
    Sucesso`"| IES_NOTIFICATION
    ESTACIO_ERROR -->|"`⚠️ **Sim**
    Erro ocorrido`"| MANUAL_SEND
    MANUAL_SEND --> IES_NOTIFICATION
    
    %% Final notification
    IES_NOTIFICATION["`📢 **IES Notifica**
    Confirmação do processo`"]
    IES_NOTIFICATION --> CAMPUS_ENROLL
    CAMPUS_ENROLL --> FIM
    
    %% Final Lead Flow
    LEAD_SOLD --> FIM
    
    %% Green comment connections (dotted lines to show context)
    COMMENT1 -.-> SG2
    COMMENT2 -.-> SYNC_COURSE
    COMMENT3 -.-> DOC_CHECK
    COMMENT4 -.-> KROTON_ERROR_1
    COMMENT5 -.-> SG9
    COMMENT6 -.-> API_CHECK
    COMMENT7 -.-> ESTACIO_ERROR
    
    %% Styling for green comment nodes
    style COMMENT1 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT2 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT3 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT4 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT5 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT6 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    style COMMENT7 fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#ffffff
    
    %% Pastel Pink for IF/Decision nodes
    style PAYMENT_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style DIGITAL_ADMISSION fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style API_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style DOC_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style INTEGRATION_TYPE fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style API_INTEGRATION fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style KROTON_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style KROTON_MODE fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style KROTON_ERROR_1 fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style ESTACIO_ERROR fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style CONFIG_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style ADMISSION_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    style ENROLL_CHECK fill:#f8cecc,stroke:#b85450,stroke-width:2px,color:#000000
    
    %% Pastel Yellow for START/END nodes
    style INICIO fill:#fff2cc,stroke:#d6b656,stroke-width:2px,color:#000000
    style FIM fill:#fff2cc,stroke:#d6b656,stroke-width:2px,color:#000000
    style LEAD_SOLD fill:#fff2cc,stroke:#d6b656,stroke-width:2px,color:#000000
    
    %% Pastel Grey for ACTION nodes
    style CADASTRO fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SEND_DOCS fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style REJECT_DOCS fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style ENROLL_DATA fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style STUDENT_ENROLLED fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style VOUCHER fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style CAMPUS_ENROLL fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SYNC_COURSE fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style KROTON_BD_1 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style KROTON_SEND_1 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style KROTON_RETRY fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style KROTON_BD_2 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style KROTON_SEND_2 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style ESTACIO_BD fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style ONETRUST fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style ESTACIO_SEND fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style MANUAL_SEND fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style CRAWLER_BD fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style CRAWLER_SEND fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style LEAD_BD_KROTON fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style LEAD_ONETRUST fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style LEAD_SEND_KROTON fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style LEAD_BD_ESTACIO fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style LEAD_SEND_ESTACIO fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style CONTRACT_1 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SEND_DOCS_1 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SELECTIVE_1 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style CONTRACT_2 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SEND_DOCS_2 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style SELECTIVE_2 fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    style IES_NOTIFICATION fill:#f5f5f5,stroke:#666666,stroke-width:2px,color:#000000
    
    %% Enhanced Subgraph Border Lines Only
    style SG1 stroke:#daa520,stroke-width:3px
    style SG2 stroke:#4682b4,stroke-width:3px
    style SG3 stroke:#228b22,stroke-width:3px
    style SG4 stroke:#db7093,stroke-width:3px
    style SG5 stroke:#8b7d6b,stroke-width:3px
    style SG6 stroke:#20b2aa,stroke-width:3px
    style SG7 stroke:#ff8c00,stroke-width:3px
    style SG8 stroke:#9370db,stroke-width:3px
    style SG9 stroke:#cd5c5c,stroke-width:3px
    style SG10 stroke:#696969,stroke-width:3px
    ```