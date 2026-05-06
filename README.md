# Dashboard PCA Geral

Dashboard online em tempo real para análise de Planejamento Consolidado (PCA).

## Características

- **Sincronização em tempo real**: Dados atualizados automaticamente a cada 60 segundos
- **Filtros avançados**: Filtros por origem, demandante, item, grupo, prioridade, fonte de recurso e contrato
- **Busca full-text**: Pesquisa por descrição, origem, PAE, grupo e item
- **3 visualizações principais**:
  - **Visão Geral**: KPIs, gráficos analíticos e tabela detalhada
  - **Ranking**: Análise dos principais demandantes e maiores valores
  - **Cronograma**: Visualização temporal com linha do tempo operacional
- **Tema claro/escuro**: Alternância automática conforme preferência do sistema
- **Responsivo**: Adaptável para dispositivos móveis e desktop
- **Gráficos interativos**: Powered by Plotly.js

## Como usar

### Abrir o dashboard

Abra o arquivo `index.html` em um navegador web. Não é necessário servidor web - funciona localmente.

### Configurar a fonte de dados

O dashboard está configurado para ler dados de uma Google Sheet. Para alterar a fonte:

1. Abra o arquivo `index.html` em um editor de texto
2. Localize a seção JavaScript no final do arquivo
3. Altere as constantes:
   ```javascript
   const SHEET_ID = '1-XrRG5oLqrcMPLNHePm3KS4671vCp1r0';
   const SHEET_NAME = 'geral';
   ```
4. Substitua por seus valores

### Estrutura esperada da Google Sheet

As colunas esperadas são:

| Coluna | Descrição |
|--------|-----------|
| ORDEM | Número de identificação do item |
| ORIGEM | Origem da demanda |
| DEMANDANTE | Solicitante |
| DESCRIÇÃO | Descrição do item |
| ITEM | Categoria do item |
| SUBITEM | Subcategoria |
| GRUPO | Grupo de classificação |
| QUANTIDADE | Quantidade solicitada |
| VALOR UNITÁRIO ESTIMADO | Valor unitário |
| VALOR TOTAL ESTIMADO | Valor total do item |
| PRIORIDADE | Alta/Média/Baixa |
| DATA DESEJADA | Período desejado (ex: "Q1 2025") |
| CONTRATO NOVO | Sim/Não |
| FONTE DO RECURSO | Identificação da fonte |
| VALOR DO RECURSO | Valor disponível |
| Nº DO PAE | Número do PAE (se disponível) |

## Recursos

### Filtros
- **Busca**: Pesquisa por descrição, origem, PAE, grupo e item
- **Origem**: Filtrar por origem da demanda
- **Demandante**: Filtrar por solicitante
- **Item**: Filtrar por categoria
- **Grupo**: Filtrar por grupo de classificação
- **Prioridade**: Filtrar por nível de prioridade
- **Fonte do recurso**: Filtrar por fonte
- **Contrato novo**: Filtrar por tipo de contrato

### Botões de ação
- **Atualizar agora**: Força a atualização dos dados
- **Limpar filtros**: Reseta todos os filtros
- **Tema**: Alterna entre tema claro e escuro

### Gráficos (Visão Geral)
- Valor por origem
- Distribuição por prioridade
- Valor por item
- Valor por grupo
- Fonte do recurso

### Ranking
- Ranking por demandante
- Ranking por descrição (maiores valores)
- Tabela dos 30 principais itens

### Cronograma
- Valor por período desejado
- Itens por período
- Linha do tempo operacional com distribuição visual

## Configuração técnica

### Dependências
- **Plotly.js 2.35.2**: Para gráficos interativos
- **Google Fonts (Inter)**: Para tipografia
- Sem dependências backend - funciona 100% no navegador

### Navegadores suportados
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

## Notas importantes

1. **CORS**: A Google Sheet deve estar compartilhada ou pública para que o dashboard possa acessar
2. **Cache**: Os dados são atualizados com um timestamp para evitar cache
3. **Performance**: Com mais de 1000 registros, considere implementar paginação
4. **Privacidade**: Certifique-se de que a Google Sheet não contém dados sensíveis

## Troubleshooting

### Gráficos não aparecem
- Verifique se Plotly.js carregou corretamente
- Abra o console (F12) e procure por erros

### Dados não aparecem
- Verifique a URL da Google Sheet
- Certifique-se de que a sheet é pública ou compartilhada
- Verifique os nomes das colunas (devem corresponder exatamente)

### Filtros não funcionam
- Limpe o cache do navegador
- Recarregue a página (Ctrl+F5)

## Licença

Desenvolvido com ❤️ para gestão eficiente do planejamento consolidado.
