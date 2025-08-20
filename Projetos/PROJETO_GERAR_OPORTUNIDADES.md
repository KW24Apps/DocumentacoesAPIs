# PROJETO: Gerar Oportunidades (Coleta e Envio em Massa)

## Partes do projeto

- Parte 1 — Coleta de dados
- Parte 2 — Diagnóstico de Operação
- Parte 3 — Organização e montagem do array
- Parte 4 — Criação dos negócios


## Parte 1 — Coleta de dados (checklist)

- [ ] Passo 1: Usar a função `BitrixDealHelper::consultarDeal($entityId, $dealId, $camposStr)` (arquivo: `Apps/helpers/BitrixDealHelper.php`) e armazenar o resultado em `$item`
 - [ ] Passo 2: Usar `BitrixHelper::consultarCamposCrm(2)` (arquivo: `Apps/helpers/BitrixHelper.php`) e armazenar o resultado em `$crmFields`.
 - [ ] Passo 3: Construir `$oportunidades`: mapa por nome amigável com subcampos `oferecida{id,texto}`, `convertida{id,texto}`, `oportunidade{id,texto}` para reaproveito.
 - [ ] Passo 4: Normalizar e armazenar em `$empresas`, `$ofer`, `$conv` — salvar nome e id quando disponíveis
 - [ ] Passo 5: Definir campos que não serão copiados para os novos deals — variável `$naoCopiar`


## Parte 2 — Diagnóstico de Operação (checklist)

- [ ] Passo 1 : Definir `$processType` e detectar situação:
	- 1 = solicitar diagnóstico (etapa `C53:UC_1PAPS7`)
	- 2 = concluído sem diagnóstico (etapa `C53:WON` e `vinculados` vazio)
	- 3 = concluído com diagnóstico já realizado (etapa `C53:WON` e `vinculados` não vazio)
	- Normalizar `$vinculados` (array, trim, filtrar vazios) antes do teste
- [ ] Passo 2 : Se comparativo, executar `compararVinculados` para obter faltantes
	- Implementação: usar `BitrixHelper::listarItems('deal', ['filter' => ['ufcrm_1707331568' => $dealId]], ['select' => 'companyId,ufCrm_1646069163997'])` para buscar todos os deals vinculados ao `closer` (ID recebido via GET: `deal`/`id`) e salvar em `$vinculadosList`.

    
## Parte 3 — Organização e montagem do array (função `montarArraysParaCriacao()`)

**Parâmetros recebidos**: `$processType`, `$empresas`, `$ofer`, `$conv`, `$oportunidades`, `$camposBase`, `$dealId`

- [ ] Passo 1: Determinar pipeline e etapa baseado no tipo de processo e situação atual:
	
	**Para processType = 1 (Solicitando Diagnóstico):**
	- Se `ufCrm_1650979003` (tipo de processo) for: `administrativo`, `administrativo anexo 5`, `contencioso ativo` ou **vazio**:
		- Pipeline: `Relatório Preliminar` (ID 17 - `GeraroptndEnums::CATEGORIA_RELATORIO_PRELIMINAR`)
		- Etapa: `Triagem Checklist` (`GeraroptndEnums::FASE_TRIAGEM_CHECKLIST` = 'C17:NEW')
	- Qualquer outro valor:
		- Pipeline: `Contencioso` (ID 55 - `GeraroptndEnums::CATEGORIA_CONTENCIOSO`)
		- Etapa: `Triagem` (`GeraroptndEnums::FASE_TRIAGEM` = 'C55:NEW')
	
	**Para processType = 2/3 (Concluído):**
	- Se `ufCrm_1650979003` for `administrativo`:
		- Pipeline: `Operacional` (ID 15 - `GeraroptndEnums::CATEGORIA_OPERACIONAL`)
		- Etapa: `Triagem Checklist Operacional` (`GeraroptndEnums::FASE_TRIAGEM_CHECKLIST_OPERACIONAL` = 'C15:NEW')
	- Qualquer outro valor (não pode ser vazio nesta etapa):
		- Pipeline: `Contencioso` (ID 55 - `GeraroptndEnums::CATEGORIA_CONTENCIOSO`)
		- Etapa: `Triagem` (`GeraroptndEnums::FASE_TRIAGEM` = 'C55:NEW')
	
	**Valores administrativos válidos**: usar `GeraroptndEnums::TIPOS_ADMINISTRATIVO` para validação

- [ ] Passo 2: Switch baseado em `$processType` para determinar origem das oportunidades:
	- Caso 1: usar `$ofer` (oportunidades oferecidas)  
	- Caso 2: usar `$conv` (oportunidades convertidas)
	- Caso 3: usar `$conv` mas filtrar apenas as que não existem (comparativo com `$existentes`)

- [ ] Passo 3: Para cada caso, montar produto cartesiano `$empresas` × `$oportunidadesOrigem`:
	- Para cada `empresa` obter `companyId` (primeiro elemento se array)
	- Para cada `nome_amigável` resolver `opportunityId = $oportunidades[$nome]['oportunidade']['id']` (sempre usar subcampo `oportunidade`)
	- Montar pares `{companyId, opportunityId}` 

- [ ] Passo 4: Para cada par, construir deal completo usando `$camposBase`:
	- Aplicar normalização de campos (múltiplo vs único) baseada em metadados
	- Definir `companyId` e `ufCrm_1646069163997` (oportunidade) com os IDs resolvidos
	- Adicionar `ufcrm_1707331568` (closer) = `$dealId`
	- Definir pipeline e etapa conforme determinado no Passo 1
	- **NÃO incluir** campos `oferecidas`/`convertidas` na criação

- [ ] Passo 5: Montar array final no formato `BitrixDealHelper::criarDeal($entityId, $categoryId, $dealsArray)`
	- Retornar estrutura pronta: `['entityId' => 2, 'categoryId' => $categoria, 'fields' => $dealsArray]`

**Observação**: Esta função executa apenas um dos casos (1/2/3) por chamada e retorna array estruturado para criação em lote.

## Parte 4 — Criação dos negócios (inline no `executar()`)

- [ ] Passo 1: Chamar `$resultado = $this->montarArraysParaCriacao($processType, $empresas, $ofer, $conv, $oportunidades, $camposBase, $dealId)`
- [ ] Passo 2: Executar `BitrixDealHelper::criarDeal($resultado['entityId'], $resultado['categoryId'], $resultado['fields'])` 
- [ ] Passo 3: Processar retorno (logs, métricas, vincular IDs criados ao deal original se necessário)



