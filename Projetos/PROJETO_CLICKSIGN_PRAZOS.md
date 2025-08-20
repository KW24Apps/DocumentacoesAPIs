# PROJETO: Gestão de Prazos de Assinatura ClickSign

Este projeto engloba duas funcionalidades distintas para gerenciar os prazos de documentos enviados para assinatura via ClickSign, integrados ao Bitrix24.

---

## PROJETO 1: Verificação Automática e Prorrogação de Prazo

**Objetivo:** Criar um processo agendado que verifica diariamente os documentos prestes a vencer, prorroga o prazo uma única vez e notifica o Bitrix em cada etapa.

### Parte 1: Preparação da Estrutura

- [ ] **Banco de Dados:**
    - [ ] Adicionar a coluna `prazo_prorrogado` (TINYINT(1), DEFAULT 0) na tabela `assinaturas_clicksign`.
- [ ] **DAO (`AplicacaoAcessoDAO.php`):**
    - [ ] Criar o método `marcarPrazoComoProrrogado(string $documentKey): bool`.
    - [ ] Criar o método `obterAssinaturasEmAberto(): array`.
- [ ] **Helper (`ClickSignHelper.php`):**
    - [ ] Criar o método `listarDocumentos(int $page = 1): ?array` para buscar documentos com paginação.
    - [ ] Criar o método `estenderPrazo(string $documentKey, string $novaData): ?array` que fará a chamada `PATCH /api/v1/documents/:key`.

### Parte 2: Implementação do Controller de Agendamento

- [ ] **Controller (`SchedulerController.php`):**
    - [ ] Criar o novo arquivo `apis.kw24.com.br/Apps/controllers/SchedulerController.php`.
    - [ ] Criar o método `verificarPrazosClickSign()`.
- [ ] **Rota:**
    - [ ] Adicionar uma nova rota em `apis.kw24.com.br/Apps/routers/` (ex: `schedulerRoutes.php`) para mapear um endpoint (ex: `/scheduler/verificar-prazos`) para o novo controller e método.

### Parte 3: Lógica de Verificação e Ação

- [ ] **Dentro de `verificarPrazosClickSign()`:**
    - [ ] **Passo 1:** Chamar `ClickSignHelper::listarDocumentos()` em loop para obter todos os documentos com status "running".
    - [ ] **Passo 2:** Para cada documento, verificar se o `deadline_at` vence nas próximas 24 horas.
    - [ ] **Passo 3:** Se estiver para vencer, usar `AplicacaoAcessoDAO::obterAssinaturaClickSign(key)` para buscar os dados locais, incluindo `prazo_prorrogado`.
    - [ ] **Passo 4 (Lógica de Decisão):**
        - [ ] **SE `prazo_prorrogado` for `false`:**
            - [ ] Calcular `nova_data` (ex: hoje + 3 dias).
            - [ ] Chamar `ClickSignHelper::estenderPrazo(key, nova_data)`.
            - [ ] Chamar `BitrixDealHelper::editarDeal()` para atualizar o campo de data no Bitrix e o campo de retorno com a mensagem de prorrogação.
            - [ ] Chamar `AplicacaoAcessoDAO::marcarPrazoComoProrrogado(key)`.
        - [ ] **SE `prazo_prorrogado` for `true`:**
            - [ ] Chamar `BitrixDealHelper::editarDeal()` apenas para atualizar o campo de retorno com a mensagem de "aviso de vencimento final".

### Parte 4: Configuração Final

- [ ] **Cron Job:**
    - [ ] Configurar um Cron Job no servidor para chamar a rota criada na Parte 2 uma vez por dia.

---

## PROJETO 2: Atualização Manual de Prazo via Bitrix

**Objetivo:** Criar um endpoint que possa ser chamado pelo Bitrix para atualizar manualmente o prazo de um documento na ClickSign.

### Parte 1: Preparação da Estrutura

- [ ] **DAO (`AplicacaoAcessoDAO.php`):**
    - [ ] Criar o método `obterAssinaturaPeloDealId(int $dealId): ?array`.

### Parte 2: Implementação do Endpoint

- [ ] **Controller (`ClickSignController.php`):**
    - [ ] Adicionar um novo método público `atualizarPrazoManualmente()`.
    - [ ] O método deve receber `deal_id` e `nova_data` (via `$_POST` ou `$_GET`).
- [ ] **Rota:**
    - [ ] Adicionar uma nova rota em `clicksignRoutes.php` para mapear um endpoint (ex: `/clicksign/atualizar-prazo`) para o novo método.

### Parte 3: Lógica de Atualização

- [ ] **Dentro de `atualizarPrazoManualmente()`:**
    - [ ] **Passo 1:** Validar os parâmetros recebidos (`deal_id`, `nova_data`).
    - [ ] **Passo 2:** Chamar `AplicacaoAcessoDAO::obterAssinaturaPeloDealId(deal_id)` para obter o `document_key`.
    - [ ] **Passo 3:** Se o `document_key` for encontrado, chamar `ClickSignHelper::estenderPrazo(document_key, nova_data)`.
    - [ ] **Passo 4:** Se a atualização na ClickSign for bem-sucedida, chamar `BitrixDealHelper::editarDeal()` para atualizar o campo de retorno no Bitrix com uma mensagem de sucesso.
    - [ ] **Passo 5:** Retornar uma resposta JSON de sucesso ou erro.

### Parte 4: Configuração Final

- [ ] **Bitrix24:**
    - [ ] Configurar um Webhook ou Robô de Automação no Bitrix que, ao alterar o campo de data de um deal, chame a rota criada na Parte 2, enviando os dados necessários.
