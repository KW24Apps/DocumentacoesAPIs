# ClickSign WhatsApp - Implementação

## 🎯 OBJETIVO
Implementar suporte para assinatura via WhatsApp no ClickSignController existente, mantendo compatibilidade com email.

---

## 📋 ÍNDICE DAS ALTERAÇÕES

### **PASSO 1: Consulta de Contatos (Linha 176-179)**
Buscar TODOS os campos sempre: `['NAME', 'LAST_NAME', 'EMAIL', 'PHONE']`  
**Razão:** Ter email e telefone disponíveis independente do tipo escolhido

### **PASSO 2: Validação de Dados (Linha ~190)**
IF baseado em `$params['tipoassinatura']` para validar email OU telefone  
**Razão:** WhatsApp precisa validar telefone, Email precisa validar email

### **PASSO 3: Criação de Signatário (Linha ~280)**  
IF baseado em `$params['tipoassinatura']` para payload diferente na ClickSign  
**Razão:** WhatsApp usa `phone_number + auths=['whatsapp']`, Email usa `email + auths=['email']`

### **PASSO 4: Envio de Notificação (Linha ~300)**
IF baseado em `$params['tipoassinatura']` para método diferente  
**Razão:** WhatsApp usa `enviarNotificacaoWhatsApp()`, Email usa `enviarNotificacao()`

---

## ✅ **CONFIRMAÇÃO**
- **3 pontos com IF condicional** (Passos 2, 3, 4)
- **1 ponto sem alteração** (Passo 1 - só adiciona campo)
- **Campo `tipoassinatura`** vem do config_extra para `$params`
- **Validação de arquivo e payload documento** não mudam

---

## 🔧 **RESUMO TÉCNICO**
- **Arquivos alterados:** 1 (ClickSignController.php)
- **Linhas modificadas:** 4 
- **Novos métodos:** 1 (ClickSignHelper::enviarNotificacaoWhatsApp)
- **Complexidade:** Muito baixa

---

## ❓ **CONFIRMAÇÃO DA ANÁLISE**

Você está **100% correto**! São exatamente **3 mudanças** em **3 lugares**:

1. **Passo 2:** Validação (email vs telefone)
2. **Passo 3:** Criação signatário (payload diferente) 
3. **Passo 4:** Notificação (método diferente)

O **Passo 1** (consulta) só adiciona um campo, não muda lógica.

**Análise perfeita!** ✅

**Status:** Estratégia definida - Pronto para implementação

---

# 📱 **PARTE 2: ACEITE VIA WHATSAPP (TEXTO)**

## 🎯 **OBJETIVO PARTE 2**
Implementar método separado para aceite via WhatsApp usando texto simples (não documentos).

---

## 📋 **NOVO MÉTODO: GerarAceiteWhatsApp()**

### **ESTRUTURA SIMPLIFICADA:**
1. **Validação básica:** entityId, dealId, token
2. **Consulta contato:** Buscar NAME + PHONE apenas
3. **Validação dados:** Verificar nome + telefone + conteúdo
4. **Criar aceite:** API `POST /api/v2/acceptance_term/whatsapps`
5. **Atualizar Bitrix:** Status de envio

### **CAMPOS NECESSÁRIOS (config_extra):**
- `destinatario`: ID do contato Bitrix
- `titulo`: Campo com título do aceite
- `conteudo`: Campo com texto do aceite  
- `retorno`: Campo para status/resposta

### **API ACEITE WHATSAPP:**
```json
{
  "name": "Aceite de Proposta",
  "content": "Você aceita a proposta XYZ?",
  "sender_name": "user_name",
  "signer_phone": "11999999999",
  "signer_name": "João Silva"
}
```

### **WEBHOOKS ACEITE (7 eventos):**
- `acceptance_term_sent`: Aceite enviado
- `acceptance_term_completed`: ✅ Aceito
- `acceptance_term_refused`: ❌ Recusado
- `acceptance_term_canceled`: Cancelado
- `acceptance_term_expired`: Expirado

### **COMPLEXIDADE:**
- **Linhas código:** ~80-100 (vs 400 da assinatura)
- **Validações:** Mínimas (nome + telefone + texto)
- **Webhooks:** 7 eventos simples
- **Limitação:** 1500 chars (título + conteúdo)

---

## 🔧 **RESUMO TÉCNICO PARTE 2**
- **Método novo:** `GerarAceiteWhatsApp()`
- **Método webhook:** `retornoAceiteWhatsApp()`
- **API:** V2 acceptance_term (diferente de documentos)
- **Campos:** 4 campos vs 8+ da assinatura
- **Tamanho:** ~25% do código da assinatura

**Status:** Projeto aceite via WhatsApp - Pronto para implementação separada

---

# 🔄 **PARTE 3: CONTROLE DE SIGNATÁRIOS**

## 🎯 **OBJETIVO PARTE 3**
Implementar controle inteligente de signatários com movimentação automática entre campos do Bitrix.

---

## 📋 **FUNCIONALIDADE:**

### **CAMPOS BITRIX (config_extra):**
- `UF_CRM_41_1754316221`: **Signatários a assinar** (quem falta)
- `UF_CRM_41_1754316398`: **Signatários que já assinaram** (quem concluiu)

### **BANCO DE DADOS - NOVAS COLUNAS:**
```sql
ALTER TABLE assinaturas_clicksign 
ADD COLUMN signatarios_json TEXT,
ADD COLUMN signatarios_assinados_json TEXT;
```

### **ESTRUTURA JSON:**
```json
// signatarios_json (todos iniciais)
[
  {"id": "signer_key_123", "nome": "João Silva", "email": "joao@email.com", "papel": "contratante"},
  {"id": "signer_key_456", "nome": "Maria Santos", "email": "maria@email.com", "papel": "contratada"}
]

// signatarios_assinados_json (quem já assinou)
[
  {"id": "signer_key_123", "nome": "João Silva", "email": "joao@email.com", "papel": "contratante", "data_assinatura": "2025-01-15 14:30:00"}
]
```

### **FLUXO DE MOVIMENTAÇÃO:**
1. **Criar assinatura:** Salva todos signatários em `signatarios_json`
2. **Receber webhook `sign`:** Move signatário para `signatarios_assinados_json`  
3. **Atualizar Bitrix:** Recalcula campos "a assinar" vs "já assinaram"
4. **Controle automático:** Sempre sincronizado entre banco e Bitrix

---

## 🔧 **IMPLEMENTAÇÃO:**

### **MODIFICAÇÕES:**
1. **`registrarAssinaturaClicksign()`** → Adicionar signatários JSON
2. **`assinaturaRealizada()`** → Movimentar + atualizar Bitrix  
3. **Config extra** → Mapear novos campos
4. **Banco** → Duas colunas JSON para controle

### **VANTAGENS:**
- **Controle preciso** de quem assinou
- **Campos Bitrix** sempre atualizados
- **Log completo** de assinaturas  
- **Performance** com JSON otimizado

---

# 📨 **PARTE 4: MELHORIA DE RETORNOS**

## 🎯 **OBJETIVO PARTE 4**  
Padronizar retornos com códigos e implementar comentários diretos no Bitrix (ao invés de campos).

---

## 📋 **CÓDIGOS DE RETORNO:**
```php
// Códigos padronizados
const RETORNO_CODES = [
    'CS001' => 'Documento enviado para assinatura',
    'CS002' => 'Assinatura realizada por {nome} - {email}',
    'CS003' => 'Documento assinado com sucesso',
    'CS004' => 'Assinatura cancelada por prazo',
    'CS005' => 'Arquivo baixado e anexado',
    'CS099' => 'Erro: {detalhes}'
];
```

## 📋 **COMENTÁRIOS BITRIX:**

### **API DESCOBERTA:** `crm.timeline.comment.*`
- **`crm.timeline.comment.add`** → Adiciona comentário na timeline
- **`crm.timeline.comment.update`** → Atualiza comentário  
- **`crm.timeline.comment.list`** → Lista comentários da entidade
- **`crm.timeline.comment.delete`** → Remove comentário

### **ESTRUTURA:**
```php
// Exemplo de implementação
BitrixHelper::adicionarComentario($entityTypeId, $entityId, [
    'COMMENT' => 'CS002: Assinatura realizada por João Silva - joao@email.com',
    'AUTHOR_ID' => 1
]);
```

### **VANTAGENS:**
- **Timeline visual** no card do Bitrix
- **Histórico cronológico** das assinaturas  
- **Campos livres** para outros usos
- **Melhor UX** para acompanhar progresso

**Status:** API encontrada - Pronto para implementação

---

**Status Geral:** 4 projetos mapeados - Pronto para implementação