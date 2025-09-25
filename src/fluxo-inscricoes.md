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
flowchart TD
    %% 💬 Green comment nodes based on the image
    COMMENT1@{ shape: comment, label: "🔄 Essa rotina roda a cada 3h pegando, do BD da IES, os dias de aula presencial dos cursos Semi" }
    COMMENT2@{ shape: comment, label: "📊 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?" }
    COMMENT3@{ shape: comment, label: "📤 Cron job que roda script que envia os alunos para a IES. Frequência: a cada 3h em minuto 30. Envia dados do aluno + course_id e os dias de presencial" }
    COMMENT4@{ shape: comment, label: "💾 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?" }
    COMMENT5@{ shape: comment, label: "🚀 Cron job que roda script que envia os alunos para a IES. Frequência: a cada 3h em minuto 30. Envia dados do aluno + course_id" }
    COMMENT6@{ shape: comment, label: "✅ Existe um cron job 'checker' que verifica o status do aluno na IES" }
    COMMENT7@{ shape: comment, label: "💿 Cron job que roda script no Databricks salvando as ordens com status = 'paid' no BD de Inscrição Frequencia: ?" }
    COMMENT8@{ shape: comment, label: "🔐 Cron job que roda a cada 2h entre as 6h e 18, responsável por enviar dados para plataforma de LGPD (Onetrust)" }
    COMMENT9@{ shape: comment, label: "📋 Cron job que roda a cada 1h entre as 10h e 14h. Enviamos os dados do aluno + cod_campus cod_turno, cod_curso, cod_forma_ingresso" }
    COMMENT10@{ shape: comment, label: "🎯 Define type captação com base no checkout_step. Se initiated ou registered = captação" }
    COMMENT11@{ shape: comment, label: "📤 Cron job com envio diário as 8h. Envia course_offer (dado feito com base em algumas queries)" }
    COMMENT12@{ shape: comment, label: "🤖 IES que usam Crawler: Belas Artes, Kroton Pós, FMU e Anima Presencial e EaD. Usamos o mesmo bot todas as IES" }
    COMMENT13@{ shape: comment, label: "🔍 Processo de verificação e envio automático" }

    INICIO(["🚀 Aluno se interessa pela bolsa (CTA - Quero esta bolsa)"])
    FIM(["✅ Fim"])
    LEAD(["💡 Lead 'Vendido'"])
    
    AC1["✍️ Assina o contrato"]
    AC2["📤 Envio dos documentos"]
    AC3["🎯 Processo Seletivo"]
    AC4["✍️ Assina o contrato"]
    AC5["📤 Envia dos documentos"]
    AC6["🎯 Processo Seletivo"]
    AC7["📋 Cadastro (E-mail, CPF, Nome, Nascimento, Celular e CEP)"]
    AC8["📄 Aluno envia documentos"]
    AC9["❌ Rejeitar documentos"]
    AC10["✏️ Matrícula com dados do aluno"]
    AC11["🎓 Aluno matriculado"]
    AC12["⏰ Cron Job 'sync_course'"]
    AC13["💾 Popula BD de inscrições"]
    AC14["📤 Envio dos dados do Aluno para IES"]
    AC15["💾 Popula BD de inscrições"]
    AC16["📤 Envio dos dados do Aluno para IES"]
    AC17["🔄 Reenvio dos dados automáticamente"]
    AC18["💾 Popula BD de inscrições"]
    AC19["🔐 Envio dos dados do Aluno para Onetrust"]
    AC20["📤 Envio dos dados do Aluno para IES"]
    AC21["📞 IES nos avisa"]
    AC22["✋ Envio manual"]
    AC23["📄 Aluno recebe comprovante da bolsa"]
    AC24["🏢 Matrícula no balcão da IES"]
    AC25["💾 Popula BD de inscrições, mas separa com type captação"]
    AC26["📤 Envio dos leads para IES"]
    AC27["💾 Popula BD de inscrições, mas separa com codAgentPdv = 14412833"]
    AC28["🔐 Envia dados do lead para Onetrust"]
    AC29["📤 Envio dos dados dos leads para IES"]
    AC30["💾 Popula banco 'subscribe_bot'"]
    AC31["📤 Envio do lead para a IES"]
    
    %% 🌸 Decisões principais (Pink IF nodes)
    IF1{"❓ admission_created"}
    IF2{"⚙️ Config de Admissão"}
    IF3{"📝 admission_enroll"}
    IF4{"💳 Pagamento PEF?"}
    IF5{"📱 Admissão Digital?"}
    IF6{"🔌 API de inscrições aluno?"}
    IF7{"📋 Documentação correta?"}
    IF8{"🏫 Kroton"}
    IF9{"⚠️ Erro no envio?"}
    IF10{"🔌 API de inscrições aluno?"}
    IF11{"🎓 Estácio"}
    IF12{"⚠️ Erro no envio?"}
    IF13{"🔄 Tipo de Integração?"}
    IF14{"🔌 API"}
    IF15{"🏫 Kroton"}
    IF16{"🎓 Estácio"}
    IF17{"🤖 Crawler"}

    %% Main flowchart connections
    INICIO --> IF1
    
    %% Main flow from admission_created decision
    IF1 -->|Sim| IF2
    IF1 -->|Não| AC7
    
    %% Config de Admissão flow
    IF2 -->|Digital| IF5
    IF2 -->|Manual| AC1
    
    %% Admissão Digital branch
    IF5 -->|Sim| AC8
    IF5 -->|Não| AC1
    
    %% Traditional enrollment flow
    AC1 --> AC2
    AC2 --> AC3
    AC3 --> IF3
    
    %% Document validation flow
    AC8 --> IF7
    IF7 -->|Sim| AC10
    IF7 -->|Não| AC9
    AC9 --> AC8
    
    %% Matrícula flow
    AC10 --> AC11
    AC11 --> FIM
    
    %% admission_enroll decision
    IF3 -->|Sim| IF4
    IF3 -->|Não| AC4
    
    %% PEF Payment flow
    IF4 -->|Sim| IF6
    IF4 -->|Não| AC4
    
    %% API inscriptions decision (first branch)
    IF6 -->|Sim| IF8
    IF6 -->|Não| AC23
    
    %% Manual enrollment flow
    AC4 --> AC5
    AC5 --> AC6
    AC6 --> IF10
    
    %% API inscriptions decision (second branch)
    IF10 -->|Sim| IF11
    IF10 -->|Não| AC24
    
    %% Kroton integration flow
    IF8 -->|Sim| AC12
    IF8 -->|Não| IF11
    AC12 --> AC13
    AC13 --> AC14
    AC14 --> IF9
    IF9 -->|Sim| AC17
    IF9 -->|Não| FIM
    AC17 --> AC14
    
    %% Estácio integration flow
    IF11 -->|Sim| AC18
    IF11 -->|Não| AC21
    AC18 --> AC19
    AC19 --> AC20
    AC20 --> IF12
    IF12 -->|Sim| AC17
    IF12 -->|Não| FIM
    
    %% Manual process
    AC21 --> AC22
    AC22 --> FIM
    
    %% Alternative flows
    AC23 --> AC24
    AC24 --> FIM
    
    %% Cadastro flow (captação)
    AC7 --> IF13
    
    %% Integration type decision
    IF13 --> IF14
    IF14 -->|API| IF15
    IF14 -->|Crawler| IF17
    
    %% API integration branches
    IF15 -->|Kroton| AC25
    IF15 -->|Estácio| AC27
    
    %% Kroton captação flow
    AC25 --> AC26
    AC26 --> LEAD
    
    %% Estácio captação flow  
    AC27 --> AC28
    AC28 --> AC29
    AC29 --> LEAD
    
    %% Crawler integration flow
    IF17 --> AC30
    AC30 --> AC31
    AC31 --> LEAD
    
    %% Lead final destination
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

```

---

## Referências Técnicas

- **[Kroton Lead Integration](kroton-lead-integration.md)**: Documentação completa da integração com APIs Kroton, incluindo OAuth2, Elasticsearch e processamento de matrículas
- **[Estácio Lead Integration](estacio-lead-integration.md)**: Documentação detalhada da integração Estácio com compliance LGPD via OneTrust
- **Databricks**: Importação diária de dados de alunos e ordens
- **APIs de Terceiros**: Integrações diretas com sistemas das IES parceiras