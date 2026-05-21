# Cypress: Passo a Passo (Registro da Semana 11)

## 1. Pré-requisitos

- Node.js instalado (versão 18+)
- Um projeto com `package.json`

---

## 2. Instalação

```bash
npm install cypress --save-dev
```

---

## 3. Abrir o Cypress pela primeira vez

```bash
npx cypress open
```

Isso abre a interface gráfica e cria a estrutura de pastas automaticamente:

```
cypress/
  e2e/          → seus testes
  fixtures/     → dados mockados (JSON)
  support/      → comandos customizados e configurações
cypress.config.js
```

---

## 4. Escrever o primeiro teste

Crie o arquivo `cypress/e2e/app.cy.js`:

```javascript
describe('App', () => {
  beforeEach(() => {
    cy.visit('/')
  })

  it('deve renderizar algo na página', () => {
    cy.get('body').should('not.be.empty')
  })

  it('deve exibir o título correto no h1', () => {
    cy.get('h1#test')
      .should('be.visible')
      .and('have.text', 'Teste com Cypress')
  })
})
```

> **Observação:** o primeiro teste verifica que a página renderizou algum conteúdo. O segundo busca especificamente o `<h1>` com `id="test"` e confere o texto.

---

## 5. Configurar a URL base

Em `cypress.config.js`:

```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:5173', // porta padrão do Vite
  },
})
```

Com isso, você usa `cy.visit('/')` em vez da URL completa.

---

## 6. Rodar os testes

**Com interface gráfica (desenvolvimento):**

```bash
npx cypress open
```

**No terminal, sem interface (CI/CD):**

```bash
npx cypress run
```

---

## 7. Comandos essenciais

| Comando | O que faz |
|---|---|
| `cy.visit('/rota')` | Navega para uma URL |
| `cy.get('seletor')` | Seleciona um elemento |
| `cy.contains('texto')` | Busca por texto visível |
| `.click()` | Clica em um elemento |
| `.type('texto')` | Digita em um input |
| `.should('exist')` | Verifica se existe |
| `.should('be.visible')` | Verifica se está visível |
| `cy.intercept()` | Intercepta requisições HTTP |

---

## 8. Dicas importantes

- **Prefira `data-cy`** nos elementos HTML para seletores mais estáveis:
  ```html
  <button data-cy="btn-salvar">Salvar</button>
  ```
  ```javascript
  cy.get('[data-cy="btn-salvar"]')
  ```

- **Evite `cy.wait(ms)`** com tempo fixo — use `.should()` para esperar condições.

- **Fixtures** servem para dados de teste reutilizáveis em `cypress/fixtures/`.

---

## 9. Adicionar scripts no `package.json`

```json
"scripts": {
  "cy:open": "cypress open",
  "cy:run": "cypress run"
}
```

Rode com:

```bash
npm run cy:open
```
