# PROJETO: Refatoração Sistema Deals Bitrix

## Passo a Passo




## PARTE 1: Agendamento de Jobs


**Passo 1:** Receber a requisição de qualquer controller, seja para criar ou editar deals (negócios).

**Passo 2:** O controller deve chamar a função `criarJobParaFila($entityId, $categoryId, array $deals, $tipoJob)` do arquivo `helpers/BitrixDealHelper.php`.

**Passo 3:** A função `criarJobParaFila` chama diretamente o arquivo `dao/BatchJobDAO.php` e utiliza a função `criarJob` para salvar o job no banco de dados.

**Passo 4:** Retornar sucesso se o job foi salvo no banco.


### Observações
- O parâmetro `$tipoJob` define se o job é de criação (`criar_deals`) ou edição (`editar_deals`).
- Não há validação de quantidade ou lógica de negócio neste momento: tudo que chega é agendado.


## PARTE 2: Processamento da Fila de Jobs

**Passo 1:** O cron executa o arquivo `DealBatchManager.php` a cada minuto.


**Passo 2:** O arquivo `DealBatchManager.php` chama o controller `DealBatchController.php`.

**Passo 3:** O controller executa a função `processarProximoJob()` (a ser criada em `DealBatchController.php`). Esta função utiliza `buscarJobPendente()` do arquivo `dao/BatchJobDAO.php` para buscar o próximo job pendente na fila.

**Passo 4:** O controller deve marcar o job como "processando" usando a função `marcarComoProcessando($jobId)` do arquivo `dao/BatchJobDAO.php`.

**Passo 5:** O controller processa o job conforme o tipo, utilizando as funções do arquivo `helpers/BitrixDealHelper.php` (`criarDeal` ou `editarDeal`).

**Passo 6:** Após o processamento, o controller deve marcar o job como "concluído" (`marcarComoConcluido($jobId, $resultado)`) ou "erro" (`marcarComoErro($jobId, $mensagemErro)`) no banco, usando o arquivo `dao/BatchJobDAO.php`. Além disso, o controller deve enviar uma mensagem de notificação ao funcionário solicitante informando o resultado do job (função de notificação a ser criada).


