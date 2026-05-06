# Desenvolvimento & Customizações

## Estrutura do Projeto

```
dashboardPCA/
├── index.html          # Arquivo principal do dashboard
├── README.md           # Documentação completa
├── config.json         # Configurações
├── .gitignore          # Arquivos ignorados pelo git
└── DEVELOP.md          # Este arquivo
```

## Modificações Comuns

### 1. Alterar cores do tema

No `index.html`, procure pela seção `:root`:

```css
:root,[data-theme="light"]{
  --color-bg:#f7f6f2;
  --color-surface:#f9f8f5;
  --color-primary:#01696f;
  /* ... outras cores */
}
```

### 2. Mudar a origem dos dados

No `index.html`, procure por:

```javascript
const SHEET_ID = '1-XrRG5oLqrcMPLNHePm3KS4671vCp1r0';
const SHEET_NAME = 'geral';
```

E substitua pelos seus valores.

### 3. Adicionar novos gráficos

Adicione um novo `<div>` no HTML:

```html
<article class="card pad">
  <div class="metric-label">Novo Gráfico</div>
  <div id="chartNovo" class="chart"></div>
</article>
```

E na função `renderCharts()`, adicione:

```javascript
const novo = aggregate(data, 'sua_coluna').slice(0, 10);
Plotly.newPlot('chartNovo', [{
  type: 'bar',
  x: novo.map(d => d.label),
  y: novo.map(d => d.value),
  marker: { color: cor }
}], baseLayout(), { displayModeBar: false, responsive: true });
```

### 4. Criar novos filtros

1. Adicione no HTML (sidebar):
```html
<div class="field">
  <label>Novo Filtro</label>
  <select id="novoFiltro" class="select"></select>
</div>
```

2. Atualize o array `selects`:
```javascript
const selects = ['origem', 'demandante', ..., 'novoFiltro'];
const fieldMap = { ..., novoFiltro: 'coluna_do_csv' };
```

3. Adicione a lógica de filtro em `getFiltered()`:
```javascript
if(document.getElementById('novoFiltro').value !== 'Todos' && 
   r.coluna_do_csv !== document.getElementById('novoFiltro').value)
  return false;
```

### 5. Usar dados de um CSV local

Substitua o `loadLiveData()` para ler de um arquivo local:

```javascript
async function loadLiveData() {
  const res = await fetch('dados.csv');
  const text = await res.text();
  // resto do código permanece igual
}
```

## Performance

### Para muitos registros (> 1000):

1. Implemente paginação na tabela
2. Limite os gráficos a top N items
3. Use Web Workers para processamento heavy
4. Implemente virtual scrolling

### Cache de dados

```javascript
const cacheKey = 'pca_dashboard_cache';
const cacheExpiry = 5 * 60 * 1000; // 5 minutos

function isCacheValid() {
  const cached = localStorage.getItem(cacheKey);
  if (!cached) return false;
  return Date.now() - JSON.parse(cached).time < cacheExpiry;
}
```

## Debugging

Abra as Developer Tools (F12) e use:

```javascript
// Ver dados carregados
console.log(rawData);

// Ver dados filtrados
console.log(getFiltered());

// Testar agregação
console.log(aggregate(rawData, 'origem'));
```

## Deploy

### Opção 1: GitHub Pages
```bash
git add .
git commit -m "Dashboard PCA"
git push origin main
```

Ative GitHub Pages nas configurações do repositório.

### Opção 2: Servidor simples
```bash
# Python 3
python -m http.server 8000

# Node.js
npx http-server
```

Acesse: `http://localhost:8000`

### Opção 3: Vercel/Netlify
Faça upload do repositório e configure automatic deployments.

## Troubleshooting de Desenvolvimento

### Gráficos não atualizam
- Verifique se `Plotly.newPlot()` é chamado com o ID correto
- Confirme que os dados estão sendo filtrados corretamente

### Filtros não populam
- Verifique se `fillFilters()` é chamada após `loadLiveData()`
- Confirme que os campos existem no CSV com os nomes corretos

### Performance lenta
- Reduza frequência de atualização: altere `setInterval(loadLiveData, 60000)` para um valor maior
- Incremente o intervalo de agregação

## Stack Tecnológico

- **HTML5**: Markup
- **CSS3**: Styling (variáveis CSS para temas)
- **JavaScript Vanilla**: Zero dependências
- **Plotly.js**: Gráficos
- **Google Sheets API**: Integração de dados

## Suporte & Contribuições

Para dúvidas ou melhorias, abra uma issue no repositório.
