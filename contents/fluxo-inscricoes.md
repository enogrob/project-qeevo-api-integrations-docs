# 🎓 Fluxo de Inscrições QueroEdu

## 📋 Visão Geral

Este documento detalha o fluxo completo de inscrições do sistema QueroEdu, apresentando o processo desde o interesse inicial do aluno até a finalização da matrícula ou geração de leads. O diagrama abaixo ilustra as diferentes rotas e decisões que determinam como cada estudante é processado pelo sistema.

### 🎯 Conceitos Fundamentais

#### 📚 Definições de Status
- **admission_created**: Aluno comprou conosco e já enviamos a matrícula para a IES, mas isso não o isenta de fazer as outras etapas
- **admission_enroll**: Assinou o contrato, mandou documentos, fez processo seletivo - aí sim mandamos matrícula para a faculdade

#### 🚀 Ponto de Entrada
O fluxo inicia quando o aluno demonstra interesse clicando em "Quero esta bolsa", seguido imediatamente pelo processo de cadastro com dados essenciais (E-mail, CPF, Nome, Nascimento, Celular e CEP).

### 🔄 Principais Rotas de Processo

#### 💳 Rota de Pagamento PEF
Após o cadastro, o sistema verifica se houve pagamento PEF:
- **Com Pagamento**: Direciona para admissão digital com validação automatizada de documentos
- **Sem Pagamento**: Encaminha para o fluxo de geração de leads

#### 🔐 Admissão Digital vs Manual
Para alunos com pagamento confirmado:
- **Digital**: Processo automatizado com upload e validação de documentos
- **Manual**: Processo tradicional com assinatura de contrato, envio de documentos e processo seletivo

#### 🏫 Integração com IES
O sistema suporta múltiplas formas de integração:
- **API Kroton**: Integração direta com endpoints específicos para cursos semipresenciais e presenciais
- **API Estácio**: Integração com compliance LGPD através da plataforma OneTrust
- **Crawler**: Para IES sem API disponível (Belas Artes, Kroton Pós, FMU, Anima)

### 🔧 Integrações Disponíveis

#### 🏫 Integração Kroton 
Para detalhes completos sobre a integração Kroton, consulte: [Kroton Lead Integration](https://github.com/quero-edu/kroton-lead-integration/blob/master/__docs__/kroton-lead-integration.md)
- **Tecnologia**: API REST + OAuth2 + Elasticsearch
- **Características**: Rate limiting (100 req/5min), sincronização de cursos automática
- **Jobs**: `sync_course`, populamento de BD, envio automático de dados

#### 🎓 Integração Estácio
Para detalhes completos sobre a integração Estácio, consulte: [Estácio Lead Integration](https://github.com/quero-edu/estacio-lead-integration/tree/master/__docs__/estacio-lead-integration.md)
- **Compliance**: Integração obrigatória com OneTrust (LGPD)
- **Tecnologia**: API Direta + OneTrust
- **Características**: Processamento em chunks, retry automático
- **Jobs**: Sync LGPD (a cada 2h), registro de inscrições (10h-14h UTC)

#### 🤖 Integração via Crawler
- **IES Atendidas**: Belas Artes, Kroton Pós, FMU, Anima (Presencial e EaD)
- **Tecnologia**: Bot automatizado único para todas as IES
- **Processo**: Populamento do banco `subscribe_bot` + envio automatizado

### 📊 Fluxos Detalhados do Sistema

#### 1. **🚀 Fluxo de Entrada e Cadastro**
- Captura do interesse do aluno através do CTA
- Coleta imediata de dados pessoais essenciais
- Verificação de pagamento PEF como ponto de decisão crítico

#### 2. **💳 Fluxo PEF (Pagamento Confirmado)**
- **Admissão Digital**: Processo automatizado com validação de documentos em tempo real
- **Fallback Manual**: Alternativa para casos onde a admissão digital não é aplicável
- **Integração API**: Roteamento direto para sistemas das IES parceiras

#### 3. **🔄 Fluxo de Integração com IES**
- **Kroton Semipresencial/Presencial**: Diferentes pipelines conforme modalidade do curso
- **Estácio**: Processo com etapa adicional de compliance LGPD via OneTrust  
- **Processo Manual**: Para IES que requerem intervenção humana
- **Sistema de Retry**: Reenvio automático em caso de falhas de comunicação

#### 4. **📝 Fluxo de Geração de Leads**
- Ativado para alunos sem pagamento PEF confirmado
- **Roteamento por Tipo**: API direta ou Crawler conforme disponibilidade da IES
- **Segmentação**: Leads marcados com códigos específicos para tracking
- **Distribuição**: Envio formatado conforme especificações de cada parceiro

#### 5. **⚠️ Tratamento de Erros e Fallbacks**
- Monitoramento contínuo através de cron jobs verificadores
- Reprocessamento automático para falhas temporárias
- Escalação para processo manual quando necessário
- Notificações para equipes de suporte em casos críticos

![Fluxo de Inscrições QueroEdu](fluxo-inscricoes.jpg)

### ⏰ Automação e Cronograma de Execução

O sistema opera através de diversos jobs automatizados com cronogramas específicos:

#### 📅 Jobs de Sincronização
- **Sync Course (Kroton)**: Execução a cada 3h, atualizando dados de cursos semipresenciais
- **LGPD Sync (Estácio)**: A cada 2h entre 6h-18h, enviando dados para plataforma OneTrust
- **Course Offers**: Envio diário às 8h de ofertas baseadas em queries específicas

#### 🔄 Jobs de Processamento
- **Dados de Alunos**: A cada 3h no minuto 30, enviando informações completas para IES
- **Status Checker**: Verificação contínua do status dos alunos nas IES parceiras
- **Lead Processing**: A cada 30min para leads com status 'initiated' e 'registered'

#### 💾 Jobs de Persistência
- **BD Population**: Salvamento contínuo de ordens com status 'paid'
- **Lead Separation**: Classificação automática por tipo de captação
- **Subscriber Bot**: Populamento do banco para processamento via crawler

### 🎯 Indicadores de Qualidade

#### ✅ Pontos de Controle
- Validação de documentação antes da matrícula
- Verificação de APIs antes do envio de dados
- Confirmação de recebimento pelas IES
- Monitoramento de taxa de erro por integração

#### 📈 Métricas de Performance  
- Taxa de conversão por rota (Digital vs Manual)
- Tempo médio de processamento por IES
- Volume de reprocessamento por tipo de erro
- Eficiência de leads gerados vs convertidos

## Referências Técnicas

- **[Kroton Lead Integration](https://github.com/quero-edu/kroton-lead-integration/blob/master/__docs__/kroton-lead-integration.md)**: Documentação completa da integração com APIs Kroton, incluindo OAuth2, Elasticsearch e processamento de matrículas
- **[Estácio Lead Integration](https://github.com/quero-edu/estacio-lead-integration/tree/master/__docs__/estacio-lead-integration.md)**: Documentação detalhada da integração Estácio com compliance LGPD via OneTrust
- **Databricks**: Importação diária de dados de alunos e ordens
- **APIs de Terceiros**: Integrações diretas com sistemas das IES parceiras