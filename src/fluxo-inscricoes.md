```mermaid
flowchart LR
    %% 💬 Green comment nodes based on the image
    COMMENT1["📝 🔄 Essa rotina roda a cada 3h pegando, do BD da IES, os dias de aula presencial dos cursos Semi"]
    COMMENT2["📝 📊 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?"]
    COMMENT3["📝 📤 Cron job que roda script que envia os alunos para a IES. Frequência: a cada 3h em minuto 30. Envia dados do aluno + course_id e os dias de presencial"]
    COMMENT4["📝 💾 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?"]
    COMMENT5["📝 🚀 Cron job que roda script que envia os alunos para a IES. Frequência: a cada 3h em minuto 30. Envia dados do aluno + course_id"]
    COMMENT6["📝 ✅ Existe um cron job 'checker' que verifica o status do aluno na IES"]
    COMMENT7["📝 💿 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?"]
    COMMENT8["� �🔐 Cron job que roda a cada 2h entre as 6h e 18, responsável por enviar dados para plataforma de LGPD (Onetrust)"]
    COMMENT9["📝 📋 Cron job que roda a cada 1h entre as 10h e 14h. Enviamos os dados do aluno + cod_campus cod_turno, cod_curso, cod_forma_ingresso"]
    COMMENT10["📝 🎯 Define type captação com base no checkout_step. Se initiated ou registered = captação"]
    COMMENT11["📝 📤 Cron job com envio diário as 8h. Envia course_offer (dado feito com base em algumas queries)"]
    COMMENT12["📝 🤖 IES que usam Crawler: Belas Artes, Kroton Pós, FMU e Anima Presencial e EaD. Usamos o mesmo bot todas as IES"]
    COMMENT13["📝 🔍 Processo de verificação e envio automático"]

    INICIO(["🚀 Aluno se interessa pela bolsa (CTA - Quero esta bolsa)"])
    FIM(["✅ Fim"])
    LEAD(["💡 Lead 'Vendido'"])
    
    %% Subgraph for initial decision
    subgraph SG1 ["🚀 Início do Processo"]
        IF1{"❓ admission_created"}
    end

    %% Subgraph for admission configuration
    subgraph SG2 ["⚙️ Configuração de Admissão"] 
        IF2{"⚙️ Config de Admissão"}
        IF5{"� Admissão Digital?"}
    end

    %% Subgraph for document handling
    subgraph SG3 ["📄 Processamento de Documentos"]
        AC1["✍️ Assina o contrato"]
        AC2["📤 Envio dos documentos"]
        AC8["📄 Aluno envia documentos"]
        AC9["❌ Rejeitar documentos"]
        IF7{"📋 Documentação correta?"}
    end

    %% Subgraph for enrollment process
    subgraph SG4 ["📝 Processo de Matrícula"]
        AC3["🎯 Processo Seletivo"]
        IF3{"📝 admission_enroll"}
        IF4{"💳 Pagamento PEF?"}
        AC10["✏️ Matrícula com dados do aluno"]
        AC11["🎓 Aluno matriculado"]
    end

    %% Subgraph for API decisions
    subgraph SG5 ["🔌 Decisões de API"]
        IF6{"🔌 API de inscrições aluno?"}
        IF8{"🏫 Kroton"}
        IF10{"� API de inscrições aluno?"}
        IF11{"🎓 Estácio"}
    end

    %% Subgraph for Kroton flow
    subgraph SG6 ["🏫 Fluxo Kroton"]
        AC12["⏰ Cron Job 'sync_course'"]
        AC13["💾 Popula BD de inscrições"]
        AC14["📤 Envio dos dados do Aluno para IES"]
        IF9{"⚠️ Erro no envio?"}
        AC17["🔄 Reenvio dos dados automáticamente"]
    end

    %% Subgraph for Estácio flow
    subgraph SG7 ["🎓 Fluxo Estácio"]
        AC18["💾 Popula BD de inscrições"]
        AC19["🔐 Envio dos dados do Aluno para Onetrust"]
        AC20["📤 Envio dos dados do Aluno para IES"]
        IF12{"⚠️ Erro no envio?"}
    end

    %% Subgraph for manual processes
    subgraph SG8 ["✋ Processos Manuais"]
        AC4["✍️ Assina o contrato"]
        AC5["📤 Envia dos documentos"]
        AC6["🎯 Processo Seletivo"]
        AC21["📞 IES nos avisa"]
        AC22["✋ Envio manual"]
        AC23["📄 Aluno recebe comprovante da bolsa"]
        AC24["🏢 Matrícula no balcão da IES"]
    end

    %% Subgraph for lead generation
    subgraph SG9 ["📋 Geração de Leads"]
        AC7["📋 Cadastro (E-mail, CPF, Nome, Nascimento, Celular e CEP)"]
        IF13{"🔄 Tipo de Integração?"}
        IF14{"🔌 API"}
        IF15{"🏫 Kroton"}
        IF17{"🤖 Crawler"}
    end

    %% Subgraph for lead processing
    subgraph SG10 ["📤 Processamento de Leads"]
        AC25["💾 Popula BD de inscrições, mas separa com type captação"]
        AC26["📤 Envio dos leads para IES"]
        AC27["💾 Popula BD de inscrições, mas separa com codAgentPdv = 14412833"]
        AC28["🔐 Envia dados do lead para Onetrust"]
        AC29["📤 Envio dos dados dos leads para IES"]
        AC30["💾 Popula banco 'subscribe_bot'"]
        AC31["📤 Envio do lead para a IES"]
    end

    %% Additional nodes for completeness
    AC15["� Popula BD de inscrições"]
    AC16["� Envio dos dados do Aluno para IES"]
    IF16{"🎓 Estácio"}

    %% Main flowchart connections with subgraph organization
    INICIO --> IF1
    
    %% First major decision branch
    IF1 -->|Sim| IF2
    IF1 -->|Não| AC7
    
    %% Config de Admissão branches
    IF2 -->|Digital| IF5  
    IF2 -->|Manual| AC1
    
    %% Digital admission path
    IF5 -->|Sim| AC8
    IF5 -->|Não| AC1
    
    %% Document handling
    AC8 --> IF7
    IF7 -->|Sim| AC10
    IF7 -->|Não| AC9
    AC9 --> AC8
    
    %% Success path from digital admission
    AC10 --> AC11
    AC11 --> FIM
    
    %% Manual contract path
    AC1 --> AC2
    AC2 --> AC3
    AC3 --> IF3
    
    %% admission_enroll decision
    IF3 -->|Sim| IF4
    IF3 -->|Não| AC4
    
    %% PEF payment branch
    IF4 -->|Sim| IF6
    IF4 -->|Não| AC4
    
    %% First API check
    IF6 -->|Sim| IF8
    IF6 -->|Não| AC23
    
    %% Alternative manual path
    AC4 --> AC5
    AC5 --> AC6
    AC6 --> IF10
    
    %% Second API check
    IF10 -->|Sim| IF11
    IF10 -->|Não| AC24
    
    %% Kroton specific flow
    IF8 -->|Kroton| AC12
    IF8 -->|Não| IF11
    AC12 --> AC13
    AC13 --> AC14
    AC14 --> IF9
    IF9 -->|Sim| AC17
    IF9 -->|Não| FIM
    AC17 --> AC14
    
    %% Estácio specific flow
    IF11 -->|Estácio| AC18
    IF11 -->|Não| AC21
    AC18 --> AC19
    AC19 --> AC20
    AC20 --> IF12
    IF12 -->|Sim| AC17
    IF12 -->|Não| FIM
    
    %% Manual fallback
    AC21 --> AC22
    AC22 --> FIM
    
    %% Voucher path
    AC23 --> AC24
    AC24 --> FIM
    
    %% Lead generation path (Cadastro)
    AC7 --> IF13
    
    %% Integration routing
    IF13 --> IF14
    IF14 -->|API| IF15
    IF14 -->|Crawler| IF17
    
    %% API routing decisions
    IF15 -->|Kroton| AC25
    IF15 -->|Estácio| AC27
    
    %% Kroton lead flow
    AC25 --> AC26
    AC26 --> LEAD
    
    %% Estácio lead flow
    AC27 --> AC28
    AC28 --> AC29
    AC29 --> LEAD
    
    %% Crawler lead flow
    IF17 --> AC30
    AC30 --> AC31
    AC31 --> LEAD
    
    %% All leads end here
    LEAD --> FIM

    %% Green comment connections (dotted lines to show context)
    COMMENT1 -.-> AC12
    COMMENT2 -.-> AC13
    COMMENT3 -.-> AC14
    COMMENT4 -.-> AC15
    COMMENT5 -.-> AC16
    COMMENT6 -.-> AC17
    COMMENT7 -.-> AC18
    COMMENT8 -.-> AC19
    COMMENT9 -.-> AC20
    COMMENT10 -.-> AC25
    COMMENT11 -.-> AC26
    COMMENT12 -.-> IF17
    COMMENT13 -.-> AC30

    %% 🎨 Node Styling - Pastel Colors for better readability
    
    %% 🌼 Pastel Yellow - Start/End nodes
    classDef yellowNodes fill:#FFF5B7,stroke:#B8860B,stroke-width:2px,color:#2C1810
    class INICIO,FIM,LEAD yellowNodes
    
    %% 🌸 Pastel Pink - Decision (IF) nodes  
    classDef pinkNodes fill:#F8BBD9,stroke:#C2185B,stroke-width:2px,color:#4A0E2E
    class IF1,IF2,IF3,IF4,IF5,IF6,IF7,IF8,IF9,IF10,IF11,IF12,IF13,IF14,IF15,IF16,IF17 pinkNodes
    
    %% 💚 Pastel Green - Comment nodes
    classDef greenNodes fill:#C8E6C9,stroke:#2E7D32,stroke-width:2px,color:#1B4332
    class COMMENT1,COMMENT2,COMMENT3,COMMENT4,COMMENT5,COMMENT6,COMMENT7,COMMENT8,COMMENT9,COMMENT10,COMMENT11,COMMENT12,COMMENT13 greenNodes
    
    %% 🔘 Pastel Grey - Action nodes
    classDef greyNodes fill:#F0F0F0,stroke:#616161,stroke-width:2px,color:#212121
    class AC1,AC2,AC3,AC4,AC5,AC6,AC7,AC8,AC9,AC10,AC11,AC12,AC13,AC14,AC15,AC16,AC17,AC18,AC19,AC20,AC21,AC22,AC23,AC24,AC25,AC26,AC27,AC28,AC29,AC30,AC31 greyNodes

```