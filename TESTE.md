# Checklist de Funcionalidade

## Verificação de Funcionalidade do Dashboard

Use este checklist para validar que o dashboard está funcionando corretamente.

### ✅ Inicialização

- [ ] Abrir `index.html` no navegador
- [ ] Página carrega sem erros (verificar console F12)
- [ ] Botão "Atualizar agora" está visível
- [ ] Data de última atualização aparece
- [ ] Tema padrão (claro/escuro) carrega corretamente

### ✅ Carregamento de Dados

- [ ] Dados carregam da Google Sheet
- [ ] Contagem de registros aparece na parte superior
- [ ] KPIs mostram valores numéricos:
  - [ ] Registros
  - [ ] Valor estimado (em BRL)
  - [ ] Prioridade alta
  - [ ] Com PAE

### ✅ Filtros

- [ ] Dropdown "Origem" popula com valores
- [ ] Dropdown "Demandante" popula com valores
- [ ] Dropdown "Item" popula com valores
- [ ] Dropdown "Grupo" popula com valores
- [ ] Dropdown "Prioridade" popula com valores
- [ ] Dropdown "Fonte do recurso" popula com valores
- [ ] Dropdown "Contrato novo" popula com valores
- [ ] Campo de busca funciona (tente buscar uma descrição)

### ✅ Botões de Ação

- [ ] **Atualizar agora**: Força reload dos dados
- [ ] **Limpar filtros**: Reseta todos os selects para "Todos"
- [ ] **Tema**: Alterna entre claro e escuro

### ✅ Gráficos (Aba Visão Geral)

- [ ] Gráfico "Valor por origem" (barras horizontais)
- [ ] Gráfico "Distribuição por prioridade" (pizza)
- [ ] Gráfico "Valor por item" (barras)
- [ ] Gráfico "Valor por grupo" (barras)
- [ ] Gráfico "Fonte do recurso" (barras horizontais)

### ✅ Tabela de Detalhamento (Abas Visão Geral)

- [ ] Tabela carrega com dados
- [ ] Colunas aparecem: Ordem, Origem, Demandante, Descrição, Item, Grupo, Prioridade, Data desejada, Valor estimado, PAE
- [ ] Dados estão ordenados por valor decrescente
- [ ] Badges de prioridade aparecem com cores corretas (Alta=vermelho, Média=amarelo, Baixa=verde)

### ✅ Filtros Funcionam

- [ ] Selecionar "Origem" filtra a tabela
- [ ] Selecionar "Prioridade=Alta" filtra apenas alta prioridade
- [ ] Buscar por palavra filtra resultados
- [ ] Combinação de filtros funciona
- [ ] KPIs se atualizam com os filtros

### ✅ Aba Ranking

- [ ] Botão "Ranking" fica ativo quando clicado
- [ ] Gráfico "Ranking por demandante" carrega
- [ ] Gráfico "Ranking por descrição" carrega
- [ ] Tabela "Maiores valores" mostra top 30 itens
- [ ] Itens estão ordenados por valor decrescente

### ✅ Aba Cronograma

- [ ] Botão "Cronograma" fica ativo quando clicado
- [ ] Gráfico "Valor por período desejado" carrega
- [ ] Gráfico "Itens por período" carrega (linha)
- [ ] Timeline mostra períodos com barras de progresso
- [ ] Valores aparecem corretamente formatados em BRL

### ✅ Responsividade

- [ ] Em tela larga (1920px+): Layout 2-colunas para gráficos
- [ ] Em tela média (1024px): Gráficos em 1 coluna
- [ ] Em tela pequena (768px): Tudo empilhado
- [ ] Sidebar colapsa em telas pequenas

### ✅ Tema Claro/Escuro

- [ ] **Tema claro**: Fundo claro, texto escuro
- [ ] **Tema escuro**: Fundo escuro, texto claro
- [ ] Cores se aplicam a:
  - [ ] Fundo
  - [ ] Cards
  - [ ] Gráficos
  - [ ] Textos
  - [ ] Bordas

### ✅ Auto-atualização

- [ ] Dados atualizam automaticamente a cada 60 segundos
- [ ] Mensagem "Última atualização" muda com o tempo
- [ ] Gráficos são recriados sem flickering excessivo

### ✅ Formatação de Dados

- [ ] Valores monetários mostram em BRL (R$ X.XXX,XX)
- [ ] Números grandes mostram com separadores (ex: 1.000.000)
- [ ] Datas aparecem no formato esperado

### ✅ Interatividade dos Gráficos

- [ ] Clicar em elementos dos gráficos (hover mostra informações)
- [ ] Gráficos respondem a mudanças de filtro
- [ ] Sem mensagens de erro no console

---

## Testes Específicos

### Test 1: Filtro combinado
1. Selecione "Origem" = TI
2. Selecione "Prioridade" = Alta
3. Verifique: KPIs, gráficos e tabela se atualizam
4. Limpe: Clique "Limpar filtros"
5. Verifique: Tudo volta ao normal

### Test 2: Busca full-text
1. Digite "rede" no campo de busca
2. Verifique: Apenas itens com "rede" aparecem
3. Digite "PAE-001"
4. Verifique: Apenas itens com esse PAE aparecem

### Test 3: Navegação entre abas
1. Clique em "Visão geral" → deve mostrar todos os gráficos
2. Clique em "Ranking" → deve mostrar ranking
3. Clique em "Cronograma" → deve mostrar timeline
4. Volte para "Visão geral" → gráficos devem estar em cache

### Test 4: Atualização manual
1. Clique "Atualizar agora"
2. Verifique: Círculo de carregamento (se houver)
3. Verifique: "Última atualização" muda

### Test 5: Tema alternado
1. Clique "Tema"
2. Verifique: Cor de fundo muda
3. Recarregue página (F5)
4. Verifique: Tema persiste (salvo em localStorage)

---

## Problemas Comuns & Soluções

| Problema | Solução |
|----------|---------|
| Gráficos não aparecem | F12 → Console → Procure erros de Plotly |
| Dados não carregam | Verifique SHEET_ID e SHEET_NAME no código |
| Filtros vazios | Confirme que há dados na Google Sheet |
| Erro de CORS | Compartilhe a Google Sheet como "Qualquer pessoa com link" |
| Tabela vazia com filtros | Clique "Limpar filtros" e tente novamente |

---

## Performance

Verificações de performance:

- [ ] Primeira carga: < 3 segundos
- [ ] Mudança de filtro: < 1 segundo
- [ ] Mudança de aba: < 100ms
- [ ] Scroll da tabela: sem lag
- [ ] Console: sem warnings (apenas info/debug esperados)

---

Versão: 1.0  
Data: 2025-05-06
