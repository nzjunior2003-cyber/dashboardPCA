# 🚀 Quick Start - Dashboard PCA

## Iniciar em 30 segundos

### Opção 1: Arquivo Local (Mais Fácil)
```bash
1. Abra o arquivo: index.html
2. Pronto! O dashboard carrega
```

### Opção 2: Com Servidor Local
```bash
# Python 3
python -m http.server 8000

# Depois abra:
http://localhost:8000
```

### Opção 3: Com Node (http-server)
```bash
npx http-server
Acesse: http://localhost:8080
```

---

## Configuração Rápida

### Apontar para sua Google Sheet

No arquivo `index.html`, procure por:
```javascript
const SHEET_ID = '1-XrRG5oLqrcMPLNHePm3KS4671vCp1r0';
const SHEET_NAME = 'geral';
```

**Substitua by seus valores:**

1. Abra sua Google Sheet
2. URL: `https://docs.google.com/spreadsheets/d/XXXX/edit`
3. O ID está entre `/d/` e `/edit`
4. O nome da abinha está na aba inferior

**Importante:** 
- Compartilhe a sheet como "Qualquer pessoa com link" (ou público)
- Certifique-se que as colunas da sheet correspondem aos nomes esperados

---

## Funcionalidades

| Funcionalidade | Como usar |
|---|---|
| **Filtros** | Use o painel esquerdo para filtrar dados |
| **Busca** | Digite no campo "Busca" para pesquisar por descrição/PAE |
| **Gráficos** | 3 abas: Visão geral, Ranking, Cronograma |
| **Tema** | Clique em "Tema" para alternar claro/escuro |
| **Atualizar** | Clique em "Atualizar agora" ou aguarde 60 segundos |

---

## Estrutura do Projeto

```
dashboardPCA/
├── index.html              ← Arquivo principal
├── README.md               ← Documentação completa
├── DEVELOP.md              ← Guia de desenvolvimento
├── TESTE.md                ← Checklist de testes
├── config.json             ← Configurações
├── package.json            ← Metadados do projeto
├── dados-exemplo.csv       ← Exemplo de dados
└── .gitignore              ← Arquivos ignorados
```

---

## Coluna de Dados Esperada

Sua Google Sheet deve ter essas colunas:

- ORDEM
- ORIGEM
- DEMANDANTE
- DESCRIÇÃO
- ITEM
- SUBITEM
- GRUPO
- QUANTIDADE
- VALOR UNITÁRIO ESTIMADO ()
- VALOR TOTAL ESTIMADO ()
- PRIORIDADE (Alta/Média/Baixa)
- DATA DESEJADA (ex: Q1 2025)
- CONTRATO NOVO (Sim/Não)
- FONTE DO RECURSO
- VALOR DO RECURSO
- Nº DO PAE

*Veja `dados-exemplo.csv` para um exemplo*

---

## Próximos Passos

### 1️⃣ Testar com Dados de Exemplo
```bash
# Se quiser usar dados locais, edite index.html:
# Altere loadLiveData() para ler dados-exemplo.csv
```

### 2️⃣ Customizar Cores
Edite a seção CSS em `index.html`:
```css
:root {
  --color-primary: #01696f;  /* Mude esta cor */
}
```

### 3️⃣ Adicionar Mais Gráficos
Veja `DEVELOP.md` para instruções

### 4️⃣ Deploy
```bash
# GitHub Pages (automático)
git push

# ou Vercel/Netlify (faça upload do repositório)
```

---

## Troubleshooting

### ❌ "Dados não aparecem"
1. Verifique SHEET_ID (copie da URL)
2. Compartilhe a Google Sheet como pública
3. Verifique os nomes das colunas (case-sensitive)

### ❌ "Gráficos brancos"
1. Abra Console (F12)
2. Procure por erros
3. Verifique se Plotly.js carregou

### ❌ "Filtros vazios"
1. Recarregue a página (Ctrl+F5)
2. Clique "Atualizar agora"
3. Verifique se há dados na Google Sheet

---

## Suporte

- 📖 Veja `README.md` para documentação completa
- 🔧 Veja `DEVELOP.md` para customizações avançadas
- ✅ Veja `TESTE.md` para validar funcionalidades

---

**Desenvolvido para gestão eficiente do planejamento consolidado** ✨
