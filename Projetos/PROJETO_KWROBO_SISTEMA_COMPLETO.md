# 🤖 PROJETO KWRobo - Especificação Técnica Completa

## 🎯 **Objetivo**
AI Agent autônomo que aprende workflows completos de atendimento por observação visual e executa de forma independente, com capacidade de escalação inteligente.

**Conceito:** Robô que trabalha como funcionário - aprende observando e executa autonomamente.

---

## 🏗️ **Arquitetura Técnica Híbrida**

### **Sistema Core**
```
KWRobo/
├── 🖥️ APLICAÇÃO LOCAL (.exe)
│   ├── Vision System (captura tela + OCR)
│   ├── Browser Automation (Playwright)
│   ├── Interface Desktop (PyQt6)
│   └── License Manager (JWT + hardware binding)
│
├── ☁️ SERVIÇOS CLOUD
│   ├── AI Engine (GPT-4o + Claude + Gemini)
│   ├── Knowledge Base (PostgreSQL + Vector DB)
│   ├── Learning System (análise padrões + ML)
│   └── License Server (controle acesso)
│
└── 🔗 COMUNICAÇÃO
    ├── API REST (HTTPS + JWT)
    ├── WebSocket (real-time)
    └── Encrypted Transfer
```

### **Stack Tecnológico Definido**
- **Linguagem:** Python 3.11+
- **Database:** PostgreSQL + Redis
- **AI:** Multi-LLM (GPT-4o, Claude 3.5, Gemini Pro)
- **Automation:** Playwright (superior ao Selenium)
- **Vision:** OpenCV + EasyOCR + PaddleOCR
- **Desktop:** PyQt6
- **API:** FastAPI + Uvicorn
- **Queue:** Celery + Redis
- **Monitoring:** Prometheus + Grafana

### **Bibliotecas Específicas**
```bash
# Core
pip install fastapi uvicorn[standard]
pip install sqlalchemy alembic psycopg2
pip install redis celery

# AI & Vision
pip install openai anthropic google-generativeai
pip install opencv-python ultralytics
pip install easyocr paddleocr

# Automation & Interface
pip install playwright
pip install PyQt6

# Monitoring & Utils
pip install prometheus-client
pip install structlog
pip install sentry-sdk
```

---

## ✅ **6 Decisões Estratégicas Fundamentais**

### **1. 🧠 Granularidade de Aprendizado: HÍBRIDA**
- **Você ensina:** Workflow completo do início ao fim
- **Sistema identifica:** Blocos reutilizáveis automaticamente
- **Implementação:** Gravação sequencial + análise posterior para extração de blocos
- **Vantagem:** Máxima reutilização + facilidade de treinamento

### **2. 🕐 Processamento: ANÁLISE POSTERIOR**
- **Durante gravação:** Foco 100% no treinamento, zero interrupções
- **Após STOP:** Sistema processa tudo em 30-60 segundos
- **Implementação:** Buffer de gravação + processamento assíncrono
- **Vantagem:** Treinamento natural + análise robusta

### **3. 👀 Detecção de Mensagens: WORKFLOW ENSINÁVEL**
- **Conceito:** Detecção também é um workflow que você ensina
- **Implementação:** OCR + análise visual de elementos da interface
- **Funciona em:** Bitrix24, WhatsApp, CRMs diversos, qualquer sistema web
- **Vantagem:** Adaptável + personalizável por empresa

### **4. 📁 Organização: DOIS TIPOS DE WORKFLOWS**

#### **📚 WORKFLOWS DE PLATAFORMA (Como usar a ferramenta)**
```
plataforma/
├── login/ (autenticação no sistema)
├── deteccao/ (detectar mensagens, notificações)
├── navegacao/ (abrir conversas, alternar entre chats)
├── arquivos/ (anexar, baixar, visualizar documentos)
└── busca/ (buscar clientes, empresas, filtros)
```

#### **🎭 WORKFLOWS DE ATENDIMENTO (Como trabalhar)**
```
atendimento/
├── primeiro_contato/ (saudação, apresentação)
├── documentos/ (entregar guias, contratos)
├── financeiro/ (consultar saldo, cobranças)
├── suporte/ (resolver problemas, escalações)
└── followup/ (acompanhamento, satisfação)
```

**Vantagem Crítica:** Se plataforma muda → só re-treina workflows de PLATAFORMA, todos os de ATENDIMENTO continuam funcionando!

### **5. 🆘 Fallback: CONFIGURÁVEL POR WORKFLOW**
- **Workflows novos:** 0 tentativas (conservador total)
- **Workflows experientes:** até 5 tentativas (confiante)
- **Workflows sensíveis:** sempre conservador (ex: cancelamento)
- **Evolução automática:** tentativas aumentam com taxa de sucesso
- **Implementação:** Sistema de configuração individual por workflow

### **6. 💾 Armazenamento: DELETAR VÍDEOS APÓS PROCESSAMENTO**
- **Lógica:** Workflow aprendido = vídeo inútil
- **Economia:** 98% menos espaço (150MB vídeo → 2MB workflow)
- **Performance:** Dados mínimos = velocidade máxima
- **Implementação:** Processamento + extração + limpeza automática

---

## 🎓 **Metodologia de Treinamento**

### **Fase 1: Ensinar PLATAFORMA (Como usar o sistema)**
```
Aula 1: [LOGIN] Como entrar no sistema
Aula 2: [DETECÇÃO] Como identificar mensagens novas
Aula 3: [NAVEGAÇÃO] Como alternar entre conversas
Aula 4: [ARQUIVOS] Como anexar e gerenciar documentos
Aula 5: [BUSCA] Como encontrar clientes e informações
```

### **Fase 2: Ensinar ATENDIMENTO (Como trabalhar com clientes)**
```
Processo 1: [PRIMEIRO CONTATO] Como receber cliente novo
Processo 2: [DOCUMENTOS] Como entregar guias e contratos
Processo 3: [FINANCEIRO] Como resolver questões de cobrança
Processo 4: [SUPORTE] Como resolver problemas técnicos
Processo 5: [FOLLOWUP] Como fazer acompanhamento
```

### **Interface de Treinamento**
```
┌─────────────────────────────────────────────────┐
│  🔴 GRAVANDO  │  ⏹️ PARAR  │  📋 "Entregar Guia"    │
│                                                 │
│  📱 Capturando: Tela + Áudio + Cliques          │
│  💾 Tamanho: 45MB                               │
│  ⏰ Tempo: 00:03:42                             │
└─────────────────────────────────────────────────┘
```

---

## 🤖 **Funcionamento do Sistema**

### **Fluxo de Execução Autônoma**
```
1. ROBÔ MONITORA: Sistema detecta novas mensagens (workflow ensinado)
2. ROBÔ ANALISA: Tipo de solicitação usando IA + palavras-chave
3. ROBÔ ESCOLHE: Workflow de ATENDIMENTO apropriado
4. ROBÔ EXECUTA: Combinando workflows de PLATAFORMA necessários
5. ROBÔ ESCALONA: Se não consegue resolver (fallback configurável)
```

### **Exemplo Prático de Operação**
```
CLIENTE: "Preciso da minha guia de pagamento"

ROBÔ PENSA:
├── Detecta: Solicitação de documento (análise IA)
├── Escolhe: Workflow "entregar_guia_pagamento"
├── Usa workflows de plataforma: login → navegacao → busca → arquivos
├── Executa: Processo completo automatizado
└── Resultado: Cliente recebe guia em 30 segundos

Se falhar:
└── Transfere: "Encontrei algumas opções, mas para garantir o documento 
              correto, vou conectar você com um especialista"
```

### **Sistema de Configuração Inteligente**
```
WORKFLOW: "Entregar Guia" (Experiente - 247 sucessos, 96.8% taxa)
├── Tentativas permitidas: 5 (confiante)
├── Timeout por tentativa: 30 segundos
├── Estratégias: Múltiplas formas de busca
└── Fallback: Após 5 tentativas sem sucesso

WORKFLOW: "Cancelar Conta" (Sensível - sempre transfere)
├── Tentativas permitidas: 0 (sempre conservador)
├── Motivo: Área crítica financeira
├── Ação: Transferência imediata para especialista
└── Estratégia: Segurança em primeiro lugar
```

---

## 🌟 **Biblioteca Universal de Workflows**

### **Conceito Revolucionário**
Workflows universais pré-construídos que reduzem tempo de setup de meses para minutos.

### **Categorias da Biblioteca**

#### **🖥️ WORKFLOWS SISTEMA OPERACIONAL (100% Universal)**
```
universais/sistema/
├── windows/
│   ├── abrir_word.json              # Ctrl+R → "winword"
│   ├── abrir_excel.json             # Ctrl+R → "excel"
│   ├── abrir_navegador.json         # Chrome/Edge/Firefox
│   ├── alternar_janelas.json        # Alt+Tab
│   └── copiar_colar_arquivo.json    # Ctrl+C/Ctrl+V
├── navegador/
│   ├── nova_aba.json                # Ctrl+T
│   ├── fechar_aba.json              # Ctrl+W
│   ├── navegar_url.json             # Ctrl+L
│   └── voltar_pagina.json           # Alt+←
└── office/
    ├── salvar_documento.json        # Ctrl+S
    ├── imprimir.json                # Ctrl+P
    └── buscar_texto.json            # Ctrl+F
```

#### **🏢 WORKFLOWS PLATAFORMA ESPECÍFICA (90% Universal)**
```
universais/plataformas/
├── bitrix24/
│   ├── login_logout.json            # Processo autenticação
│   ├── acessar_contatos.json        # Menu → CRM → Contatos
│   ├── buscar_cliente_cpf.json      # Filtros → CPF/CNPJ
│   ├── acessar_deals.json           # Menu → CRM → Negócios
│   └── criar_tarefa.json            # + Nova Tarefa
├── omie/
│   ├── login_omie.json              # Login sistema financeiro
│   ├── consultar_cliente.json       # Busca por CNPJ
│   ├── gerar_boleto.json            # Financeiro → Boletos
│   └── consultar_saldo.json         # Relatórios → Saldo
└── whatsapp_web/
    ├── login_whatsapp.json          # QR Code scan
    ├── buscar_conversa.json         # Ctrl+F → nome
    └── enviar_arquivo.json          # 📎 → anexar
```

### **Estratégia de Implementação**
```
CLIENTE INSTALA:
├── Download KWRobo
├── Escolhe plataformas: "Bitrix24" + "Omie"
├── Workflows universais carregados automaticamente
├── Sistema funcionando em 5 minutos
└── Cliente só ensina workflows de ATENDIMENTO específicos

VANTAGEM COMPETITIVA:
├── Sem biblioteca: 2-3 meses para setup
├── Com biblioteca: 5 minutos para 80% funcionando
└── Diferencial: 2000x mais rápido que concorrentes
```

---

## 🔧 **Estrutura de Arquivos Completa**

```
kwrobo.kw24.com.br/
├── 📱 desktop_app/                    # Aplicação .exe local
│   ├── main.py                       # Entry point principal
│   ├── 🖥️ gui/                        # Interface PyQt6
│   │   ├── main_window.py            # Dashboard principal
│   │   ├── training_dialog.py        # Interface REC/STOP
│   │   ├── workflow_manager.py       # Gerenciar workflows
│   │   ├── settings_panel.py         # Configurações sistema
│   │   └── dashboard_widgets.py      # Widgets status/métricas
│   ├── 👁️ vision/                     # Sistema de visão local
│   │   ├── screen_capture.py         # Screenshots OpenCV
│   │   ├── ocr_processor.py          # EasyOCR + PaddleOCR
│   │   ├── element_detector.py       # YOLO detection elementos
│   │   ├── video_recorder.py         # Gravação treinamentos
│   │   └── image_utils.py            # Utilities processamento imagem
│   ├── 🤖 automation/                 # Automação Playwright
│   │   ├── browser_controller.py     # Controle navegador
│   │   ├── action_executor.py        # Executor ações (click/type)
│   │   ├── workflow_runner.py        # Executor workflows
│   │   ├── element_locator.py        # Localização elementos DOM
│   │   └── fallback_manager.py       # Sistema escalação
│   ├── 🔐 license/                    # Sistema licenciamento
│   │   ├── license_manager.py        # Gerenciador principal
│   │   ├── hardware_binding.py       # Binding hardware específico
│   │   ├── jwt_validator.py          # Validação tokens JWT
│   │   └── offline_validator.py      # Validação offline
│   ├── ☁️ api_client/                 # Cliente API cloud
│   │   ├── cloud_client.py           # HTTP client (requests/httpx)
│   │   ├── websocket_client.py       # WebSocket real-time
│   │   ├── auth_manager.py           # Gerenciamento autenticação
│   │   ├── workflow_sync.py          # Sincronização workflows
│   │   └── model_schemas.py          # Pydantic schemas
│   ├── 💾 local_storage/              # Armazenamento local
│   │   ├── workflow_cache.py         # Cache workflows SQLite
│   │   ├── settings_store.py         # Configurações locais
│   │   └── temp_manager.py           # Gerenciamento temporários
│   └── 🛠️ utils/                      # Utilitários aplicação
│       ├── crypto_utils.py           # Criptografia local
│       ├── file_utils.py             # Manipulação arquivos
│       ├── validation_utils.py       # Validações entrada
│       └── logging_config.py         # Configuração logs
│
├── ☁️ cloud_api/                      # Serviços cloud
│   ├── 🌐 app/                        # FastAPI application
│   │   ├── main.py                   # API principal
│   │   ├── auth.py                   # Autenticação JWT
│   │   ├── middleware.py             # Middlewares (CORS, auth, etc)
│   │   ├── dependencies.py           # Dependencies FastAPI
│   │   └── routers/                  # Endpoints REST
│   │       ├── workflows.py          # CRUD workflows
│   │       ├── training.py           # Upload/análise vídeos
│   │       ├── licensing.py          # Controle licenças
│   │       ├── analytics.py          # Métricas/relatórios
│   │       ├── users.py              # Gestão usuários
│   │       └── health.py             # Health checks
│   ├── 🧠 ai/                         # Engine IA
│   │   ├── orchestrator.py           # Orquestrador multi-LLM
│   │   ├── providers/                # Provedores IA
│   │   │   ├── openai_client.py      # GPT-4o integration
│   │   │   ├── claude_client.py      # Claude 3.5 integration
│   │   │   ├── gemini_client.py      # Gemini Pro integration
│   │   │   └── base_provider.py      # Provider abstrato
│   │   ├── decision_engine.py        # Engine decisão contextual
│   │   ├── context_analyzer.py       # Análise contexto mensagens
│   │   ├── prompt_templates.py       # Templates prompts
│   │   └── response_parser.py        # Parser respostas IA
│   ├── 📚 learning/                   # Sistema aprendizado
│   │   ├── video_processor.py        # Processamento vídeos
│   │   ├── pattern_analyzer.py       # Análise padrões visuais
│   │   ├── workflow_builder.py       # Construção workflows
│   │   ├── block_detector.py         # Detecção blocos reutilizáveis
│   │   ├── knowledge_extractor.py    # Extração conhecimento
│   │   ├── similarity_engine.py      # Engine similaridade
│   │   └── optimization_engine.py    # Otimização workflows
│   ├── 💾 data/                       # Camada dados
│   │   ├── database.py               # Conexão PostgreSQL
│   │   ├── models/                   # Modelos SQLAlchemy
│   │   │   ├── workflow.py           # Modelo workflow
│   │   │   ├── user.py               # Modelo usuário
│   │   │   ├── license.py            # Modelo licença
│   │   │   ├── execution.py          # Modelo execução
│   │   │   └── analytics.py          # Modelo analytics
│   │   ├── repositories/             # Repositories pattern
│   │   │   ├── workflow_repo.py      # Repository workflows
│   │   │   ├── user_repo.py          # Repository usuários
│   │   │   └── analytics_repo.py     # Repository analytics
│   │   ├── migrations/               # Migrações Alembic
│   │   └── seeders/                  # Dados iniciais
│   ├── 🔐 license/                    # Sistema licenciamento
│   │   ├── license_server.py         # Servidor licenças
│   │   ├── user_manager.py           # Gestão usuários
│   │   ├── billing_integration.py    # Integração cobrança
│   │   ├── hardware_validator.py     # Validação hardware
│   │   └── subscription_manager.py   # Gestão assinaturas
│   ├── 📊 monitoring/                 # Observabilidade
│   │   ├── metrics.py                # Métricas Prometheus
│   │   ├── logging.py                # Logs estruturados
│   │   ├── health_checks.py          # Health endpoints
│   │   ├── error_tracking.py         # Sentry integration
│   │   └── performance_monitor.py    # Monitor performance
│   ├── 🔄 tasks/                      # Tasks Celery
│   │   ├── video_processing.py       # Processamento vídeos
│   │   ├── workflow_analysis.py      # Análise workflows
│   │   ├── batch_operations.py       # Operações batch
│   │   └── scheduled_tasks.py        # Tasks agendadas
│   └── 🛠️ utils/                      # Utilities cloud
│       ├── redis_client.py           # Cliente Redis
│       ├── s3_client.py              # Cliente storage S3
│       ├── email_service.py          # Serviço email
│       └── encryption.py             # Criptografia
│
├── 🧠 workflows/                      # Base conhecimento
│   ├── 🌐 universais/                 # Workflows universais
│   │   ├── sistema/                  # Sistema operacional
│   │   │   ├── windows/              # Workflows Windows
│   │   │   ├── navegador/            # Workflows navegador
│   │   │   └── office/               # Workflows Office
│   │   └── plataformas/              # Plataformas específicas
│   │       ├── bitrix24/             # Workflows Bitrix24
│   │       ├── omie/                 # Workflows Omie
│   │       └── whatsapp_web/         # Workflows WhatsApp
│   ├── 📚 clientes/                   # Workflows por cliente
│   │   └── [empresa_id]/             # Workflows específicos empresa
│   │       ├── plataforma/           # Como usar ferramenta
│   │       │   ├── login/            # Autenticação
│   │       │   ├── deteccao/         # Detectar mensagens
│   │       │   ├── navegacao/        # Navegação interface
│   │       │   ├── arquivos/         # Gestão arquivos
│   │       │   └── busca/            # Busca informações
│   │       └── atendimento/          # Como atender clientes
│   │           ├── primeiro_contato/ # Novos clientes
│   │           ├── documentos/       # Entrega documentos
│   │           ├── financeiro/       # Questões financeiras
│   │           ├── suporte/          # Suporte técnico
│   │           └── followup/         # Acompanhamento
│   └── 📝 templates/                  # Templates workflows
│       ├── plataforma_template.json  # Template workflows plataforma
│       └── atendimento_template.json # Template workflows atendimento
│
├── 🔗 shared/                         # Código compartilhado
│   ├── models/                       # Modelos comuns
│   │   ├── workflow_model.py         # Estrutura workflow
│   │   ├── action_model.py           # Modelo ação
│   │   ├── user_model.py             # Modelo usuário
│   │   └── base_model.py             # Modelo base
│   ├── schemas/                      # Schemas Pydantic
│   │   ├── workflow_schemas.py       # Schemas workflow
│   │   ├── user_schemas.py           # Schemas usuário
│   │   └── api_schemas.py            # Schemas API
│   ├── utils/                        # Utilitários comuns
│   │   ├── crypto_utils.py           # Criptografia compartilhada
│   │   ├── file_utils.py             # Manipulação arquivos
│   │   ├── validation_utils.py       # Validações
│   │   ├── date_utils.py             # Utilitários data/hora
│   │   └── string_utils.py           # Utilitários string
│   ├── constants/                    # Constantes sistema
│   │   ├── status_codes.py           # Códigos status
│   │   ├── error_messages.py         # Mensagens erro
│   │   └── config_keys.py            # Chaves configuração
│   └── protocols/                    # Protocolos/interfaces
│       ├── api_protocols.py          # Contratos API
│       ├── workflow_protocols.py     # Contratos workflow
│       └── storage_protocols.py      # Contratos storage
│
├── 🚀 deployment/                     # Deploy e packaging
│   ├── docker/                       # Containers cloud
│   │   ├── Dockerfile.api            # Container API
│   │   ├── Dockerfile.worker         # Container Celery worker
│   │   ├── Dockerfile.scheduler      # Container Celery beat
│   │   ├── docker-compose.yml        # Orquestração desenvolvimento
│   │   └── docker-compose.prod.yml   # Orquestração produção
│   ├── installer/                    # Installer aplicação desktop
│   │   ├── build_exe.py              # Script PyInstaller
│   │   ├── requirements.txt          # Dependências desktop
│   │   ├── setup.iss                 # Script Inno Setup
│   │   ├── icon.ico                  # Ícone aplicação
│   │   └── license.txt               # Licença software
│   ├── kubernetes/                   # Manifests Kubernetes
│   │   ├── namespace.yaml            # Namespace
│   │   ├── api-deployment.yaml       # Deployment API
│   │   ├── worker-deployment.yaml    # Deployment worker
│   │   ├── redis-deployment.yaml     # Deployment Redis
│   │   ├── postgres-deployment.yaml  # Deployment PostgreSQL
│   │   └── ingress.yaml              # Regras Ingress
│   └── scripts/                      # Scripts automação
│       ├── setup_dev.py              # Setup desenvolvimento
│       ├── deploy_prod.py            # Deploy produção
│       ├── migrate_db.py             # Migrações database
│       ├── backup_data.py            # Backup dados
│       ├── restore_data.py           # Restore dados
│       └── health_check.py           # Health check sistema
│
├── 🧪 tests/                          # Testes automatizados
│   ├── desktop/                      # Testes aplicação desktop
│   │   ├── test_vision.py            # Testes sistema visão
│   │   ├── test_automation.py        # Testes automação
│   │   ├── test_gui.py               # Testes interface
│   │   ├── test_license.py           # Testes licenciamento
│   │   └── conftest.py               # Configuração pytest
│   ├── cloud/                        # Testes API cloud
│   │   ├── test_api.py               # Testes endpoints
│   │   ├── test_ai.py                # Testes IA
│   │   ├── test_learning.py          # Testes aprendizado
│   │   ├── test_licensing.py         # Testes licenças
│   │   ├── test_tasks.py             # Testes Celery
│   │   └── conftest.py               # Configuração pytest
│   ├── integration/                  # Testes integração
│   │   ├── test_end_to_end.py        # Testes E2E completos
│   │   ├── test_workflow_execution.py # Testes execução workflows
│   │   ├── test_api_integration.py   # Testes integração API
│   │   └── test_performance.py       # Testes performance
│   ├── fixtures/                     # Fixtures testes
│   │   ├── sample_workflows.json     # Workflows exemplo
│   │   ├── test_videos/              # Vídeos teste
│   │   └── mock_data.py              # Dados mock
│   └── utils/                        # Utilitários testes
│       ├── test_helpers.py           # Helpers testes
│       ├── mock_services.py          # Mock services
│       └── data_generators.py        # Geradores dados teste
│
├── 📚 docs/                           # Documentação
│   ├── api/                          # Documentação API
│   │   ├── openapi.json              # Especificação OpenAPI
│   │   ├── endpoints.md              # Documentação endpoints
│   │   └── authentication.md        # Documentação autenticação
│   ├── user_manual/                  # Manual usuário
│   │   ├── installation.md           # Guia instalação
│   │   ├── getting_started.md        # Primeiros passos
│   │   ├── training_workflows.md     # Guia treinamento
│   │   ├── configuration.md          # Configuração sistema
│   │   └── troubleshooting.md        # Resolução problemas
│   ├── developer/                    # Documentação desenvolvimento
│   │   ├── setup.md                  # Setup desenvolvimento
│   │   ├── architecture.md           # Arquitetura detalhada
│   │   ├── contributing.md           # Guia contribuição
│   │   ├── coding_standards.md       # Padrões código
│   │   └── testing.md                # Estratégia testes
│   ├── deployment/                   # Documentação deploy
│   │   ├── local_setup.md            # Setup local
│   │   ├── docker_deploy.md          # Deploy Docker
│   │   ├── kubernetes_deploy.md      # Deploy Kubernetes
│   │   └── monitoring.md             # Setup monitoramento
│   └── workflows/                    # Documentação workflows
│       ├── workflow_format.md        # Formato workflows
│       ├── best_practices.md         # Melhores práticas
│       └── examples/                 # Exemplos workflows
│
├── 📋 requirements/                   # Arquivos requirements
│   ├── base.txt                      # Dependências base
│   ├── desktop.txt                   # Dependências desktop
│   ├── cloud.txt                     # Dependências cloud
│   ├── dev.txt                       # Dependências desenvolvimento
│   └── test.txt                      # Dependências testes
│
├── 🔧 config/                         # Configurações projeto
│   ├── settings.py                   # Settings principais
│   ├── development.env               # Environment desenvolvimento
│   ├── production.env                # Environment produção
│   ├── logging.yaml                  # Configuração logging
│   └── monitoring.yaml               # Configuração monitoring
│
├── .gitignore                        # Git ignore
├── .dockerignore                     # Docker ignore
├── .github/                          # GitHub workflows
│   └── workflows/
│       ├── ci.yml                    # Continuous Integration
│       ├── cd.yml                    # Continuous Deployment
│       └── security.yml              # Security checks
├── pytest.ini                       # Configuração pytest
├── pyproject.toml                    # Configuração projeto Python
├── README.md                         # README principal
├── LICENSE                           # Licença projeto
└── CHANGELOG.md                      # Changelog projeto
```

---

## 💡 **Vantagens Competitivas**

### **vs. Chatbots Tradicionais**
- ✅ **Aprende visualmente** - não precisa programar regras
- ✅ **Workflows completos** - não apenas respostas pontuais
- ✅ **Funciona em qualquer sistema** - não limitado a texto

### **vs. RPA Tradicional**
- ✅ **Inteligência contextual** - não apenas automação cega
- ✅ **Aprendizado por demonstração** - não precisa programar
- ✅ **Fallback inteligente** - sabe quando parar e transferir

### **vs. Funcionário Humano**
- ✅ **Disponibilidade 24/7** - não tem limitações horárias
- ✅ **Consistência 100%** - não tem variações de humor/cansaço
- ✅ **Escala infinita** - atende múltiplos clientes simultaneamente

---

## 🚀 **Cronograma de Implementação**

### **Fase 1: MVP Fundação**
```
Objetivos:
├── Estrutura Python funcional
├── Captura de tela básica
├── Interface PyQt6 com botões REC/STOP
├── Controle básico navegador
└── Integração LLM inicial

Tarefas:
├── Setup ambiente Python 3.11+
├── Estrutura pastas conforme arquitetura
├── OpenCV para screenshots
├── PyQt6 interface básica
├── Playwright controle navegador
├── Gemini para análise (gratuito inicial)
└── Teste end-to-end simples
```

### **Fase 2: Sistema de Aprendizado**
```
Objetivos:
├── Processamento vídeos de treinamento
├── Extração de workflows
├── Sistema de blocos reutilizáveis
├── Armazenamento PostgreSQL
└── Reprodução ações simples

Tarefas:
├── EasyOCR para reconhecimento texto
├── Análise sequência de ações
├── Algoritmo detecção blocos
├── Models SQLAlchemy
├── Sistema workflow builder
└── Executor ações básicas
```

### **Fase 3: Autonomia Completa**
```
Objetivos:
├── Sistema decisão contextual
├── Multi-LLM orchestration
├── Execução autônoma completa
├── Fallback configurável
└── Monitoramento conversas

Tarefas:
├── Engine decisão IA
├── Integração GPT-4o + Claude
├── Sistema fallback individual
├── Loop monitoramento contínuo
├── Escalação inteligente
└── Relatórios execução
```

### **Fase 4: Produção e Otimização**
```
Objetivos:
├── Performance tuning
├── Sistema licenciamento
├── Biblioteca workflows universais
├── Analytics avançados
└── Deploy produção

Tarefas:
├── Otimização performance
├── Sistema licenças JWT
├── Workflows universais Windows/Office
├── Dashboards analytics
├── Deploy Docker/Kubernetes
└── Monitoramento Prometheus
```

---

## 🔄 **Fluxos Críticos do Sistema**

### **Fluxo de Treinamento**
```
1. USUÁRIO: Clica REC, define nome workflow
2. SISTEMA: Inicia gravação (tela + áudio + ações)
3. USUÁRIO: Executa workflow completo naturalmente
4. USUÁRIO: Clica STOP quando termina
5. SISTEMA: Processa vídeo (30-60 segundos)
6. IA: Analisa sequência, identifica blocos, extrai workflow
7. SISTEMA: Salva workflow estruturado, deleta vídeo
8. USUÁRIO: Testa workflow recém-criado
```

### **Fluxo de Execução**
```
1. ROBÔ: Monitora sistema via workflow "detectar_mensagens"
2. ROBÔ: Identifica nova mensagem
3. IA: Analisa conteúdo + contexto da mensagem
4. SISTEMA: Escolhe workflow de atendimento apropriado
5. ROBÔ: Executa workflow combinando blocos de plataforma
6. SISTEMA: Monitora execução, aplica fallbacks se necessário
7. ROBÔ: Finaliza atendimento ou transfere conforme configuração
```

### **Fluxo de Fallback**
```
1. ROBÔ: Tentativa de execução falha
2. SISTEMA: Verifica configuração do workflow
3. SE tentativas < limite configurado:
   ├── Tenta abordagem alternativa
   └── Incrementa contador tentativas
4. SE tentativas >= limite:
   ├── Executa mensagem de transferência
   ├── Conecta com humano especialista
   └── Registra caso para melhoria
```

---

## 🎯 **Especificações Técnicas Críticas**

### **Formato de Workflow (JSON)**
```json
{
  "id": "entregar_guia_pagamento",
  "tipo": "atendimento",
  "categoria": "documentos",
  "versao": "1.2.3",
  "criado_em": "2024-01-15T10:30:00Z",
  "taxa_sucesso": 0.968,
  "total_execucoes": 247,
  "configuracao": {
    "tentativas_max": 5,
    "timeout_tentativa": 30,
    "modo_conservador": false
  },
  "triggers": {
    "palavras_chave": ["guia", "pagamento", "boleto", "cobrança"],
    "contexto": ["documento", "financeiro"],
    "confianca_minima": 0.7
  },
  "blocos": [
    {
      "id": "bloco_1",
      "tipo": "plataforma",
      "acao": "navegar_conversa",
      "workflow_ref": "plataforma/navegacao/abrir_conversa.json"
    },
    {
      "id": "bloco_2", 
      "tipo": "plataforma",
      "acao": "buscar_cliente",
      "workflow_ref": "plataforma/busca/buscar_cliente_cpf.json",
      "parametros": {
        "fonte_cpf": "conversa_ativa"
      }
    },
    {
      "id": "bloco_3",
      "tipo": "atendimento",
      "acao": "gerar_guia",
      "steps": [
        {"tipo": "click", "elemento": "#btn-financeiro", "timeout": 5},
        {"tipo": "wait", "duracao": 2},
        {"tipo": "click", "elemento": ".guia-pagamento", "timeout": 3},
        {"tipo": "download", "elemento": "#download-btn", "timeout": 10}
      ]
    },
    {
      "id": "bloco_4",
      "tipo": "plataforma", 
      "acao": "anexar_arquivo",
      "workflow_ref": "plataforma/arquivos/anexar_arquivo.json",
      "parametros": {
        "arquivo": "ultimo_download"
      }
    }
  ],
  "fallback": {
    "mensagem": "Encontrei algumas opções de guia, mas para garantir o documento correto, vou conectar você com um especialista financeiro.",
    "transferir_para": "financeiro",
    "contexto_transferencia": "Cliente solicitou guia de pagamento - {nome_cliente} - {empresa}"
  }
}
```

### **Sistema de Blocos Reutilizáveis**
```python
class WorkflowBlock:
    def __init__(self, block_id: str, block_type: str):
        self.id = block_id
        self.type = block_type  # 'plataforma' ou 'atendimento'
        self.reuse_count = 0
        self.success_rate = 0.0
    
    def execute(self, context: dict) -> bool:
        """Executa o bloco com contexto fornecido"""
        pass
    
    def can_reuse_in(self, workflow_type: str) -> bool:
        """Verifica se bloco pode ser reutilizado em tipo de workflow"""
        return self.type == 'plataforma' or workflow_type == 'atendimento'

class WorkflowEngine:
    def __init__(self):
        self.blocks = {}
        self.workflows = {}
    
    def build_workflow_from_video(self, video_path: str) -> dict:
        """Constrói workflow analisando vídeo de treinamento"""
        # 1. Extrai frames e ações do vídeo
        # 2. Identifica blocos candidatos a reutilização
        # 3. Constrói estrutura workflow
        # 4. Retorna workflow estruturado
        pass
    
    def execute_workflow(self, workflow_id: str, context: dict) -> bool:
        """Executa workflow completo"""
        workflow = self.workflows[workflow_id]
        for block in workflow.blocks:
            if not self.execute_block(block, context):
                return self.handle_fallback(workflow, context)
        return True
```

### **Sistema Multi-LLM**
```python
class MultiLLMOrchestrator:
    def __init__(self):
        self.providers = {
            'gpt4o': OpenAIClient(),
            'claude': ClaudeClient(), 
            'gemini': GeminiClient()
        }
    
    def analyze_message(self, message: str) -> dict:
        """Analisa mensagem para escolher workflow"""
        # GPT-4o: Melhor para function calling e estruturação
        result = self.providers['gpt4o'].analyze(
            message, 
            function_schema=self.get_workflow_selection_schema()
        )
        return result
    
    def generate_response(self, context: dict) -> str:
        """Gera resposta contextual"""
        # Claude: Melhor para geração de texto natural
        return self.providers['claude'].generate_response(context)
    
    def analyze_visual_elements(self, screenshot: bytes) -> dict:
        """Analisa elementos visuais da tela"""
        # Gemini: Melhor para análise multimodal
        return self.providers['gemini'].analyze_image(screenshot)
```

---

## 🎯 **Próximos Passos**

### **Implementação Imediata**
1. **Setup ambiente desenvolvimento** - Python 3.11, VS Code, Git
2. **Criar estrutura básica** - Pastas conforme arquitetura definida
3. **Implementar captura tela** - OpenCV + screenshots básicos
4. **Interface PyQt6** - Botões REC/STOP funcionais
5. **Controle navegador** - Playwright básico
6. **Integração IA inicial** - Gemini para análise

### **Desenvolvimento Paralelo**
7. **Workflows universais** - Gravar ações Windows básicas
8. **Sistema de licenças** - JWT + hardware binding
9. **Database setup** - PostgreSQL + models iniciais
10. **Testes automatizados** - Framework pytest + fixtures

**🚀 Foco total no desenvolvimento do sistema - sem distrações de negócio ou marketing!**
