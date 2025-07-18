# Guia para Instalar e Configurar o Eslint e Prettier

Este guia fornece instruções passo a passo para instalar e configurar o Eslint e o Prettier em um projeto Node.js.

## Índice

- [Iniciar um package.json](#iniciar-um-packagejson)
- [Instalar Eslint e Prettier](#instalar-eslint-e-prettier)
- [Instalar o Prettier e os plughins do Eslint](#instalar-o-prettier-e-os-plughins-do-eslint)
- [Adicionar o plugin do prettier ao Eslint](#adicionar-o-plugin-do-prettier-ao-eslint)
- [Criar o arquivo de configuração do Prettier](#criar-o-arquivo-de-configuração-do-prettier)
  - [Sugestões de Configuração do Prettier](#sugestões-de-configuração-do-prettier)
  - [Regras de Formatação Básicas](#regras-de-formatação-básicas)
  - [Regras de Vírgulas e Quebras](#regras-de-vírgulas-e-quebras)
  - [Regras de Quebra de Linha](#regras-de-quebra-de-linha)
  - [Configurações Específicas](#configurações-específicas)
- [Como funciona o EsLint](#como-funciona-o-eslint)
  - [Como Funcionam as Regras do ESLint](#como-funcionam-as-regras-do-eslint)
    - [Estrutura de Configuração](#1-estrutura-de-configuração)
    - [Níveis de Regras](#2-níveis-de-regras)
    - [Exemplo de Configuração](#3-exemplo-de-configuração)
    - [Exemplo de Regras Customizadas](#4-exemplo-de-regras-customizadas)
    - [Ordem de Precedência](#5-ordem-de-precedência)
- [Regras ESLint Recomendadas por Categoria](#regras-eslint-recomendadas-por-categoria)
  - [Erros Críticos](#1-erros-críticos)
  - [Melhores Práticas](#2-melhores-práticas)
  - [Formatação (Compatível com Prettier)](#3-formatação-compatível-com-prettier)
  - [Qualidade de Código](#4-qualidade-de-código)
  - [Nomenclatura](#5-nomenclatura)
  - [Outras Boas Práticas](#6-outras-boas-práticas)
- [Usar o arquivo .eslintignore](#usar-o-arquivo-eslintignore)

## Iniciar um package.json

Primeiro, você precisa iniciar um novo projeto Node.js ou usar um existente. Para iniciar um novo `package.json`, execute o seguinte comando no terminal:

```bash
npm init -y
```

## Instalar Eslint e Prettier

Em seguida, instale o Eslint e o Prettier como dependências de desenvolvimento

```bash
npm install eslint --save-dev
npm init @eslint/config@latest
```

## Instalar o Prettier e os plughins do Eslint

```bash
npm install --save-dev --save-exact prettier
npm i -D eslint-config-prettier
```

## Adicionar o plugin do prettier ao Eslint

Abra o arquivo eslint.config.mjs e adicione o plugin do Prettier:

```javascript
// codigo existente
import eslintConfigPrettier from 'eslint-config-prettier';
export default defineConfig([
  // ... outras configurações do eslint
  { files: ['**/*.{js,mjs,cjs}'], languageOptions: { globals: globals.node } },
  eslintConfigPrettier, // adicione ao final do array
]);
```

## Criar o arquivo de configuração do Prettier

Crie um arquivo chamado `.prettierrc.json` na raiz do seu projeto. Abaixo veremos uma série de regras disponíveis para configurar o Prettier:

### Sugestões de Configuração do Prettier

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "quoteProps": "as-needed",
  "jsxSingleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

### Regras de Formatação Básicas

- **printWidth** (padrão: `80`): Define o comprimento máximo da linha antes de quebrar.
- **tabWidth** (padrão: `2`): Define quantos espaços por nível de indentação.
- **useTabs** (padrão: `false`): Usa tabs em vez de espaços para indentação.
- **semi** (padrão: `true`): Adiciona ponto e vírgula no final das declarações.
- **singleQuote** (padrão: `false`): Usa aspas simples em vez de aspas duplas.

### Regras de Vírgulas e Quebras

- **trailingComma** (padrão: `"es5"`): Controla vírgulas no final.
  - `"none"`: Sem vírgulas no final.
  - `"es5"`: Vírgulas onde válidas no ES5 (objetos, arrays).
  - `"all"`: Vírgulas onde possível (incluindo parâmetros de função).

### Regras de Quebra de Linha

- **bracketSpacing** (padrão: `true`): Espaços dentro de chaves de objetos `{ foo: bar }`.
- **bracketSameLine** (padrão: `false`): Coloca `>` de JSX na mesma linha.
- **arrowParens** (padrão: `"always"`): Parênteses em arrow functions.
  - `"always"`: `(x) => x`
  - `"avoid"`: `x => x`

### Configurações Específicas

- **endOfLine** (padrão: `"lf"`): Tipo de quebra de linha (`lf`, `crlf`, `cr`, `auto`).
- **quoteProps** (padrão: `"as-needed"`): Quando colocar aspas em propriedades de objeto.
- **jsxSingleQuote** (padrão: `false`): Aspas simples em JSX.

## Como funciona o EsLint

## Como Funcionam as Regras do ESLint

### 1. Estrutura de Configuração

O ESLint utiliza objetos de configuração que definem:

- **files**: Quais arquivos receberão as regras.
- **rules**: Regras específicas e seus níveis.
- **extends**: Configurações pré-definidas.
- **plugins**: Funcionalidades adicionais.

### 2. Níveis de Regras

- `"off"` ou `0`: Regra desabilitada.
- `"warn"` ou `1`: Apenas aviso (não impede execução).
- `"error"` ou `2`: Erro (impede execução).

### 3. Exemplo de Configuração

```javascript
export default defineConfig([
  {
    files: ['**/*.{js,mjs,cjs}'], // Aplica a arquivos JS
    plugins: { js }, // Plugin JavaScript
    extends: ['js/recommended'], // Usa regras recomendadas
  },
  {
    files: ['**/*.js'],
    languageOptions: { sourceType: 'commonjs' }, // CommonJS para .js
  },
  {
    files: ['**/*.{js,mjs,cjs}'],
    languageOptions: { globals: globals.node }, // Globais do Node.js
  },
  eslintConfigPrettier, // Desabilita regras que conflitam com Prettier
]);
```

### 4. Exemplo de Regras Customizadas

```javascript
export default defineConfig([
  {
    files: ['**/*.{js,mjs,cjs}'],
    plugins: { js },
    extends: ['js/recommended'],
    rules: {
      'no-console': 'warn', // Avisa sobre console.log
      'no-unused-vars': 'error', // Erro para variáveis não usadas
      semi: ['error', 'always'], // Exige ponto e vírgula
      quotes: ['error', 'single'], // Exige aspas simples
    },
  },
  // ...outras configurações...
]);
```

### 5. Ordem de Precedência

- Configurações mais específicas sobrescrevem as gerais.
- Arrays são processados em ordem.
- `eslintConfigPrettier` deve vir por último para evitar conflitos com Prettier.

O ESLint analisa seu código e aplica essas regras, reportando problemas conforme os níveis configurados.

## Regras ESLint Recomendadas por Categoria

### 1. Erros Críticos

```js
'no-unused-vars': 'error',      // Variáveis não usadas
'no-undef': 'error',            // Variáveis não definidas
'no-unreachable': 'error',      // Código inalcançável
'no-console': 'warn',           // Uso de console.log (apenas aviso)
```

### 2. Melhores Práticas

```js
'eqeqeq': ['error', 'always'],      // Use === ao invés de ==
'no-var': 'error',                  // Prefira let/const
'prefer-const': 'error',            // Use const quando possível
'no-eval': 'error',                 // Proíbe uso de eval()
'no-implied-eval': 'error',         // Proíbe eval implícito
```

### 3. Formatação (Compatível com Prettier)

```js
'indent': 'off',                    // Prettier gerencia indentação
'quotes': 'off',                    // Prettier gerencia aspas
'semi': 'off',                      // Prettier gerencia ponto e vírgula
```

### 4. Qualidade de Código

```js
'complexity': ['warn', 10],             // Máximo de 10 caminhos por função
'max-depth': ['warn', 4],               // Máximo de 4 níveis de bloco
'max-lines-per-function': ['warn', 50], // Máximo de 50 linhas por função
```

### 5. Nomenclatura

```js
'camelcase': ['error', { properties: 'never' }], // CamelCase em variáveis
'new-cap': 'error',                             // Construtores com maiúscula
```

### 6. Outras Boas Práticas

```js
'no-magic-numbers': ['warn', { ignore: [0, 1, -1] }], // Evite números mágicos
'no-duplicate-imports': 'error',                      // Importações duplicadas
'no-return-assign': 'error',                          // Atribuição em return
'no-self-compare': 'error',                           // Comparação consigo mesmo
'prefer-template': 'error',                           // Prefira template literals
```

## Usar o arquivo .eslintignore

Você pode criar um arquivo `.eslintignore` na raiz do seu projeto para especificar quais arquivos ou diretórios o ESLint deve ignorar. Por exemplo:

```
node_modules/
dist/
build/
coverage/
*.min.js
*.bundle.js
```
