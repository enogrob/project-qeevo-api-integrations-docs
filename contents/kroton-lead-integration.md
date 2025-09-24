# Integração de Leads da Kroton

## Contents

- [Processo de Inscrição da Kroton](#processo-de-inscrição-da-kroton)
- [Arquitetura](#arquitetura)
- [Perspectivas Alternativas](#perspectivas-alternativas)
- [Lista de IES/integradores com integração ativa](#lista-de-iesintegradores-com-integração-ativa)
- [Esquema de payloads esperados por tipo de evento](#esquema-de-payloads-esperados-por-tipo-de-evento)
- [Padrão de autenticação por tipo de integração](#padrão-de-autenticação-por-tipo-de-integração)
- [Endpoints de envio](#endpoints-de-envio)
- [Regras de negócio por integração ativa](#regras-de-negócio-por-integração-ativa)
- [Definição de eventos mínimos por tipo de ação](#definição-de-eventos-mínimos-por-tipo-de-ação)
- [Formato de resposta esperado das APIs externas](#formato-de-resposta-esperado-das-apis-externas)
- [Status de processamento - follow_ups table](#status-de-processamento---follow_ups-table)
- [References](#references)

## Processo de Inscrição da Kroton

O sistema de integração de leads da Kroton é um serviço Node.js/TypeScript que automatiza o processo de inscrição de alunos no vestibular da Kroton através de sua API oficial. Este sistema faz parte do ecossistema Quero Educação e permite a consulta de ofertas disponíveis e o registro de inscrições de forma automatizada.

O processo começa com a importação diária de dados de alunos que não converteram na Kroton através de um notebook Databricks, seguido por uma série de ações automatizadas: sincronização de cursos via Elasticsearch da Kroton, validação de dados pessoais, verificação de inscrições existentes e, finalmente, o registro de novas inscrições. O sistema mantém controle completo do ciclo de vida das inscrições através de status bem definidos e follow-ups detalhados.

A integração utiliza tanto a API oficial da Kroton quanto seu middleware de catálogo via Elasticsearch, garantindo acesso às ofertas mais atualizadas e respeitando os limites de rate limiting estabelecidos pela instituição (100 requisições a cada 5 minutos).

## Arquitetura

```mermaid
flowchart TD
    subgraph "🎯 Quero Educação Ecosystem"
        DB[("`🗄️ **PostgreSQL**
        Database`")] 
        DATABRICKS["`📊 **Databricks**
        Daily Import`"]
    end
    
    subgraph "🏗️ Kroton Integration Service"
        API["`🚀 **API Server**
        Node.js/TypeScript`"]
        
        subgraph "⚙️ Job Processors"
            CHECKER["`🔍 **Checker**
            Validate Students`"]
            SYNC["`🔄 **SyncCourses**
            Course Catalog`"]
            REGISTER["`📝 **Register**
            Student Enrollment`"]
            FIXDATA["`🔧 **FixData**
            Gender Assignment`"]
        end
        
        subgraph "🏪 Data Models"
            SUBS["`📋 **Subscription**
            Student Data`"]
            COURSE["`🎓 **Course**
            Course Information`"]
            FOLLOWUP["`📈 **FollowUp**
            Status Tracking`"]
        end
    end
    
    subgraph "🎓 Kroton Services"
        KROTON_API["`🔗 **Kroton API**
        OAuth2 + REST`"]
        ELASTICSEARCH["`🔍 **Elasticsearch**
        Course Catalog`"]
    end
    
    subgraph "📱 External Services"
        SLACK["`💬 **Slack**
        Notifications`"]
        IBGE["`🏛️ **IBGE Census**
        Name Gender Data`"]
    end

    %% Main data flow
    DATABRICKS -->|"`📥 Import students
    Status: to_sync`"| DB
    DB --> API
    
    %% Job processing flow
    API --> FIXDATA
    FIXDATA -->|"`👥 Gender assignment
    IBGE data lookup`"| IBGE
    FIXDATA --> SYNC
    
    SYNC -->|"`🔍 Course search
    Elasticsearch queries`"| ELASTICSEARCH
    ELASTICSEARCH -->|"`📚 Course matches
    2 options required`"| SYNC
    SYNC -->|"`✅ Status: to_register
    ❌ Status: empty_course
    🚫 Status: course_failed`"| DB
    
    SYNC --> CHECKER
    CHECKER -->|"`📊 Check existing
    inscriptions`"| KROTON_API
    KROTON_API -->|"`✅ Existing data
    📊 Status updates`"| CHECKER
    
    CHECKER --> REGISTER
    REGISTER -->|"`📝 New enrollment
    OAuth2 + API calls`"| KROTON_API
    KROTON_API -->|"`✅ Status: registered
    ❌ Status: register_failed
    ⏳ Status: awaiting_register`"| REGISTER
    
    %% Data relationships
    API -.-> SUBS
    API -.-> COURSE
    API -.-> FOLLOWUP
    
    %% Notifications
    API -->|"`📢 Job status
    Slack integration`"| SLACK
    
    %% Styling
    classDef ecosystemNodes fill:#E8F4FD,stroke:#1976D2,stroke-width:2px,color:#000
    classDef serviceNodes fill:#FFF3E0,stroke:#F57C00,stroke-width:2px,color:#000
    classDef jobNodes fill:#E8F5E8,stroke:#388E3C,stroke-width:2px,color:#000
    classDef dataNodes fill:#F3E5F5,stroke:#7B1FA2,stroke-width:2px,color:#000
    classDef krotonNodes fill:#FFEBEE,stroke:#D32F2F,stroke-width:2px,color:#000
    classDef externalNodes fill:#F1F8E9,stroke:#689F38,stroke-width:2px,color:#000
    
    class DB,DATABRICKS ecosystemNodes
    class API serviceNodes
    class CHECKER,SYNC,REGISTER,FIXDATA jobNodes
    class SUBS,COURSE,FOLLOWUP dataNodes
    class KROTON_API,ELASTICSEARCH krotonNodes
    class SLACK,IBGE externalNodes
```

## Perspectivas Alternativas

<details>
<summary><strong>Sequence Diagram - Processo de Inscrição</strong> (Clique para expandir)</summary>

```mermaid
sequenceDiagram
    participant D as 📊 Databricks
    participant DB as 🗄️ Database
    participant API as 🚀 API Service
    participant FD as 🔧 FixData Job
    participant SC as 🔄 SyncCourses Job
    participant C as 🔍 Checker Job
    participant R as 📝 Register Job
    participant ES as 🔍 Elasticsearch
    participant KA as 🎓 Kroton API
    participant S as 💬 Slack

    Note over D,S: 🌅 Daily Import Process
    D->>+DB: Import students (status: to_sync)
    Note right of DB: 👥 New student subscriptions created
    
    Note over D,S: 🔧 Data Preparation Phase
    API->>+FD: Execute FixData action
    FD->>DB: Query subscriptions without gender
    Note right of FD: 👤 Gender assignment using IBGE census data
    FD->>-DB: Update gender information
    FD->>S: 📢 Job completion notification
    
    Note over D,S: 📚 Course Synchronization Phase
    API->>+SC: Execute SyncCourses action
    SC->>DB: Fetch subscriptions (status: to_sync)
    loop For each subscription
        SC->>+ES: Search courses by offer criteria
        ES-->>-SC: Return matching courses (need 2 options)
        alt Courses found
            SC->>DB: Update status to 'to_register'
            Note right of DB: ✅ Ready for enrollment
        else No courses found
            SC->>DB: Update status to 'empty_course'
            Note right of DB: ❌ No available courses
        else API Error
            SC->>DB: Update status to 'course_failed'
            Note right of DB: 🚫 Retry later
        end
    end
    SC->>-S: 📢 Sync completion notification
    
    Note over D,S: 🔍 Verification Phase
    API->>+C: Execute Checker action
    C->>DB: Fetch all active subscriptions
    loop For each subscription
        C->>+KA: Check existing enrollment by CPF
        KA-->>-C: Return enrollment status
        alt Already enrolled
            C->>DB: Create/update FollowUp record
            Note right of DB: 📈 Track existing enrollment
        end
    end
    C->>-S: 📢 Check completion notification
    
    Note over D,S: 📝 Registration Phase
    API->>+R: Execute Register action
    R->>DB: Fetch subscriptions (status: to_register)
    loop For each subscription
        R->>+KA: Submit enrollment request
        KA-->>R: Return processing result
        alt Registration successful
            R->>DB: Update status to 'registered'
            Note right of DB: ✅ Successfully registered
        else Processing in progress
            R->>DB: Update status to 'awaiting_register'
            Note right of DB: ⏳ Async processing
        else Registration failed
            R->>DB: Update status to 'register_failed'
            Note right of DB: ❌ Registration error
        end
    end
    R->>-S: 📢 Registration completion notification
    
    Note over D,S: 📈 Continuous Monitoring
    loop Daily monitoring
        API->>C: Re-check enrollment status
        C->>KA: Query latest status updates
        KA-->>C: Return current classifications
        C->>DB: Update FollowUp records
        Note right of DB: 🔄 Status: enrolled, dropped, failed
    end
```

</details>

<details>
<summary><strong>State Diagram - Ciclo de Vida da Inscrição</strong> (Clique para expandir)</summary>

```mermaid
stateDiagram-v2
    [*] --> to_sync : 📥 Databricks Import
    
    to_sync --> course_failed : 🚫 API Error
    to_sync --> empty_course : ❌ No Courses Found
    to_sync --> to_register : ✅ Courses Found
    
    course_failed --> to_register : 🔄 Retry Success
    course_failed --> course_failed : 🔄 Retry Failed
    
    empty_course --> [*] : ⛔ Final State
    
    to_register --> registered : ✅ Registration Success
    to_register --> register_failed : ❌ Registration Failed
    to_register --> awaiting_register : ⏳ Processing
    
    awaiting_register --> registered : ✅ Processing Complete
    awaiting_register --> failed : 🚫 Processing Error
    
    registered --> enrolled : 🎓 Student Enrolled
    registered --> dropped : 📉 Student Dropped
    registered --> failed : 🚫 IES Rejected
    
    enrolled --> [*] : ✅ Success End
    dropped --> [*] : ❌ Dropped End
    failed --> [*] : 🚫 Failed End
    register_failed --> [*] : ❌ Error End
    
    note right of to_sync
        👥 Student imported from Databricks
        with course and personal data
    end note
    
    note right of to_register
        📚 2 course options found
        👤 Gender validated
        📊 Ready for enrollment
    end note
    
    note right of registered
        📝 API call successful
        📈 FollowUp tracking starts
        🔄 Periodic status checks
    end note
    
    note right of enrolled
        🎯 Final success state
        💰 Revenue generated
        📊 Conversion complete
    end note
```

</details>

## Lista de IES/integradores com integração ativa

| Instituição | Tipo de Integração | Status | Observações |
|-------------|-------------------|---------|-------------|
| Kroton | API REST + Middleware | ✅ Ativo | Integração completa com OAuth2 |
| Elasticsearch Kroton | Catálogo de Cursos | ✅ Ativo | Rate limit: 100 req/5min |

**Modalidades Suportadas:**
- **COLABORAR**: Educação a Distância (EAD)
- **OLIMPO**: Presencial

**Ambientes Disponíveis:**
- **Staging**: `https://ingresso-api-stg-portal.krthomolog.com.br`
- **Production**: `https://ingresso-api-portal.kroton.com.br`

## Esquema de payloads esperados por tipo de evento

### Inscrição de Aluno (POST)

```typescript
interface EnrollmentPayload {
  dadosPessoais: {
    celular: string;        // "11975405666"
    cpf: string;           // "378.457.608-70" 
    dataNascimento: string; // "1989-08-31"
    email: string;         // "mauricio.matsoui@redealumni.com"
    endereco: {
      cep: string;         // "12243-740"
      logradouro: string;  // "Rua Pedro de Toledo"
      municipio: string;   // "São José dos Campos"
      numero: string;      // "48"
      uf: string;         // "SP"
    };
    necessidadesEspeciais: any[];
    nome: string;          // "Mauricio Matsoui"
    rg: string;           // "20000000"
    sexo: "M" | "F";      // "M" or "F"
  };
  inscricao: {
    canalVendas: {
      id: number;          // 85
    };
    idAfiliado: string;    // "DL00QUERO12991"
    idTipoProva: number;   // 1
    ofertas: {
      primeiraOpcao: {
        id: string;        // "1093731-446-52505-580-872433-11342-100"
      };
      segundaOpcao?: {
        id: string;        // Optional second option
      };
    };
  };
}
```

### Consulta de Cursos (Elasticsearch)

```typescript
interface CourseSearchPayload {
  query: {
    bool: {
      must: [
        { match: { dsCurso: { query: string; operator: "AND" } } },
        { match: { idUnidadeOrigem: number } },
        { match: { dsTipoCurso: string } },
        { match: { vlMensalidadeDe: number } },
        { match: { ativa: boolean } },
        { match: { dsModalidade: string } },
        { term: { periodoCaptacao: number } },
        { range: { dtTerminoInscricao: { gte: string } } }
      ];
    };
  };
}
```

### Modelo de Dados - Subscription

```typescript
interface Subscription {
  id: number;
  user_name: string;
  cpf: string;
  gender?: string;
  birthday: string;
  sent_at?: string;
  last_check?: string;
  email: string;
  area_code: string;
  phone_number: string;
  address: string;
  // Status possíveis: to_sync, course_failed, empty_course, 
  // to_register, registered, register_failed, dropped, 
  // enrolled, failed, awaiting_register
}
```

## Padrão de autenticação por tipo de integração

### Kroton API - OAuth2

```typescript
interface AuthConfig {
  client_id: string;      // API_KROTON_CLIENT_ID
  client_secret: string;  // API_KROTON_SECRET
  grant_type: "client_credentials";
  scope: string;          // API_KROTON_SUBSCRIPTION
}
```

**Características:**
- Token expira rapidamente
- Rate limit: 100 requisições a cada 5 minutos
- Renovação automática implementada no `krotonService`

### Elasticsearch Middleware

**Staging**: Requer autenticação via Bearer Token
```http
Authorization: Bearer {token}
```

**Production**: Sem autenticação necessária (até 07/01/2022)

## Endpoints de envio

### Kroton API Endpoints

| Endpoint | Método | Descrição |
|----------|---------|-----------|
| `/oauth2/token` | POST | Autenticação OAuth2 |
| `/ms/inscricao/v4/captacao/inscricao/cpf/{cpf}` | GET | Consulta CPF Inscrito |
| `/ms/matricula/captacao/v1/ms/matricula/candidato/cpf/{cpf}` | GET | Consulta CPF Matriculado |
| `/ms/inscricao/v4/captacao/inscricao/{inscricao}/sistema/{sistema}` | GET | Consulta Código de Inscrição |
| `/ms/inscricao/v4/captacao/inscricao` | POST | Inscrição do Aluno |

### Elasticsearch Middleware

| Ambiente | URL Base |
|----------|----------|
| Staging | `https://captacao-aks-stg.krthomolog.com.br/elasticlayer/middleware/oferta/_search` |
| Production | `https://captacao-akS.kroton.com.br/elasticlayer/middleware/oferta/_search` |

## Regras de negócio por integração ativa

### Validações de Inscrição

1. **CPF Único**: Constraint de unicidade na tabela `subscriptions` por CPF
2. **Duas Opções de Curso**: Obrigatório para realizar inscrição
3. **Gender Obrigatório**: Inscrições sem gender são ignoradas no register
4. **Data de Nascimento**: Validação pendente para datas antes de 1900
5. **Campos de Endereço**: Todos obrigatórios conforme API da Kroton

### Rate Limiting

- **Kroton API**: 100 requisições / 5 minutos
- **Implementação**: Chunks com intervalo controlado na classe `Base`
- **Retry Logic**: Implementado para falhas de rede e rate limit

### Status Transitions

```
to_sync → syncCourses → [to_register|empty_course|course_failed]
to_register → register → [registered|register_failed|awaiting_register]
registered → checker → [enrolled|dropped|failed]
```

## Definição de eventos mínimos por tipo de ação

### Actions Disponíveis

| Action | Descrição | Frequência |
|--------|-----------|------------|
| `checker` | Valida alunos matriculados/inscritos | Diária |
| `syncCourses` | Busca ofertas no Elasticsearch | Por demanda |
| `register` | Inscreve leads no vestibular | Por demanda |
| `fixData` | Adiciona gender via IBGE census | Por demanda |
| `allJobs` | Executa todas as actions | Automática |

### Eventos de Sistema

1. **Import Event**: Databricks → Database (daily)
2. **Sync Event**: Course catalog synchronization
3. **Register Event**: Student enrollment submission
4. **Check Event**: Status verification and updates
5. **Notification Event**: Slack alerts for job status

## Formato de resposta esperado das APIs externas

### Kroton API - Inscrição Response

```typescript
interface EnrollmentResponse {
  success: boolean;
  inscricaoId?: string;
  status: "NEW" | "PROCESSING" | "ERROR" | "SUCCESS";
  message?: string;
  errors?: string[];
}
```

### Kroton API - Consulta CPF Response

```typescript
interface CPFQueryResponse {
  inscricoes: Array<{
    id: string;
    classificacao: {
      descricao: "Convocado" | "Classificado" | "Aluno" | 
                "Inscrito" | "Itinerante" | "Inscrito VG Online" | 
                "Ausente";
    };
    curso: string;
    unidade: string;
    status: string;
  }>;
}
```

### Elasticsearch - Course Search Response

```typescript
interface CourseSearchResponse {
  hits: {
    total: { value: number };
    hits: Array<{
      _source: {
        id: string;
        dsCurso: string;
        dsTipoCurso: string;
        dsModalidade: string;
        vlMensalidadeDe: number;
        dtTerminoInscricao: string;
        ativa: boolean;
        periodoCaptacao: number;
        sistema: "COLABORAR" | "OLIMPO";
      };
    }>;
  };
}
```

## Status de processamento - follow_ups table

### Classificações da Kroton (source.inscricao.classificacao.descricao)

| Status Kroton | Status Interno | Descrição |
|---------------|----------------|-----------|
| **Aluno** | `enrolled` | ✅ Matriculado (aprovado + pagamento efetuado) |
| **Convocado** | `enrolled` | 🎯 Aprovado e convocado para matrícula |
| **Classificado** | `registered` | 📝 Aprovado em vestibular |
| **Inscrito** | `registered` | 📋 Inscrição realizada |
| **Itinerante** | `registered` | 🚶 Ingressante por vestibular |
| **Inscrito VG Online** | `registered` | 💻 Inscrito para prova online |
| **Ausente** | `dropped` | ❌ Faltou na prova agendada |
| **Desclassificado** | `dropped` | ❌ Não aprovado no processo |

### Status de Processamento Assíncrono

| Status API | Status Interno | Descrição |
|------------|----------------|-----------|
| **NEW** | `awaiting_register` | ⏳ Inscrição em fila de processamento |
| **PROCESSING** | `awaiting_register` | 🔄 Processamento em andamento |
| **ERROR** | `failed` | 🚫 Erro no processamento |
| **SUCCESS** | `registered` | ✅ Processamento concluído |

### Estrutura da Tabela FollowUp

```typescript
interface FollowUp {
  id: number;
  kroton_subscription_id?: number;
  kroton_subscription_dealer_id?: string;
  source: any; // JSON com dados completos da Kroton
  subscription: Subscription;
  created_at: Date;
  updated_at: Date;
}
```

## References

- **GitHub Repository**: [https://github.com/quero-edu/kroton-lead-integration](https://github.com/quero-edu/kroton-lead-integration)
- **Kroton API Documentation**: 
  - Staging: https://ingresso-api-stg-portal.krthomolog.com.br/products/Ingresso
  - Production: https://ingresso-api-portal.kroton.com.br/products/Ingresso
- **Databricks Notebook**: [Daily Import Process](https://dbc-cd62f9f0-a95c.cloud.databricks.com/?o=7804433505040691#notebook/2344378349282606/command/2344378349282607)
- **Quero Boot**: [Development Environment](https://github.com/quero-edu/quero-boot)
- **TypeScript**: v14.17.6 Runtime Environment
- **TypeORM**: Database ORM and Migration System
- **Docker**: Containerization and Local Development