# GoBarber Web

Vamos contruir o GoBarber WEB que vai consumir a API Rest Gobarber Backend em NodeJS, vamos controlar rotas privadas, fazer a autenticação JWT e receber um token de autenticação. A autenticação do usuário vai ficar guardada no Redux para sempre que precisarmos do usuário logado, ter acesso ao Nome e avatar.


## Aula 01 - Estrutura do projeto

Para começar vamos criar um projeto com `create-react-app`:

```
npx create-react-app gobarber-web
```

Instalo e configuro o `eslint`:

```
yarn add eslint -D
```

Executo o eslint init com os seguintes parâmetros de configuração:

```
gobarber-web on  master [!] took 8s
❯ yarn eslint --init
yarn run v1.12.0
$ /Users/tgmarinho/Developer/bootcamp_rocketseat_studies/gobarber-web/node_modules/.bin/eslint --init
? How would you like to use ESLint? To check syntax, find problems, and enforce code style
? What type of modules does your project use? JavaScript modules (import/export)
? Which framework does your project use? React
? Does your project use TypeScript? No
? Where does your code run? (Press <space> to select, <a> to toggle all, <i> to invert selection)Browser
? How would you like to define a style for your project? Use a popular style guide
? Which style guide do you want to follow? Airbnb (https://github.com/airbnb/javascript)
? What format do you want your config file to be in? JavaScript
? Would you like to install them now with npm? Yes
```

Deixo baixando tudo!

Removo o `package-look.json` e rodo `yarn` para atualizar o `yarn-lock.json` uma vez que estou usando apenase o Yarn.

Instalo as dependências do prettier e babel:

```
❯ yarn add babel-eslint prettier eslint-plugin-prettier eslint-config-prettier -D
```

E configuro o `.eslintrc.js`:

```
module.exports = {
  env: {
    es6: true,
    jest: true,
    browser: true
  },
  extends: ["airbnb", "prettier", "prettier/react"],
  globals: {
    Atomics: "readonly",
    SharedArrayBuffer: "readonly",
    __DEV__: true
  },
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 2018,
    sourceType: "module"
  },
  plugins: ["react", "jsx-a11y", "import", "react-hooks", "prettier"],
  rules: {
    "prettier/prettier": "error",
    "react/jsx-filename-extension": ["error", { extensions: [".js", ".jsx"] }],
    "import/prefer-default-export": "off",
    "no-unused-vars": ["error", { argsIgnorePattern: "^_" }],
    "react/jsx-one-expression-per-line": "off",
    "global-require": "off",
    "react-native/no-raw-text": "off",
    "no-param-reassign": "off",
    "no-underscore-dangle": "off",
    camelcase: "off",
    "no-console": ["error", { allow: ["tron"] }],
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  },
  settings: {
    "import/resolver": {
      "babel-plugin-root-import": {
        rootPathSuffix: "src"
      },
    },
  },
};
```

Removi alguns arquivos desncessários, veja no github, e deixei o App.js com uma div apenas.

Rode o comando `yarn start` para conferir está funcionando, uma tela branca deve aparecer.

## Aula 03 - Configurando rotas

Quando falamos em rotas com React, já pensamos em React Router Dom, e sim vamos comecar instalando ele:

```
yarn add react-router-dom
```

E na pasta `src` vamos criar as pastas `pages`, `services` e `routes`  e um arquivo `index.js` dentro da pasta route e da pasta `services` um arquivo `history.js`. Na pasta `pages` vamos criar os arquivos `Dashboard`, `Profile`, `SignIn`, `SignUp`, cada pasta com seu respectivo arquivo `index.js`  e usando o snippets da Rocketseat vamos criar um conteudo: `rfc` enter e pronto, só salvar o componente que foi criado.

```
yarn add history
```

## Aula 04 - Configurando o Reactotron

Instale o Reactotron-react.js:
```
yarn add reactotron-react-js
```

## Aula 05 - Rotas privadas

Do jeito que está a aplicação, podemos acessar todas as rotas mesmo não estando logado, agora o que iremos fazer é controlar as rotas, verificando se o usuário está logado ou não.

Vamos criar um arquivo chamado `Route.js` que vai ser um wrapper do Route do react-router-dom:

Vamos criar uma função RouteWrapper que recebe as props do componente que irá utilizar esse componente RouteWrapper, ele recebe a prop Component, isPrivate e o restante que tiver será passada no ...rest.

Se o usuário não estiver auteticado e a rota for privada ele é redirecionado para o /.

Se ele estiver logado e a rota não é privada, ele vai para o dashboard, para as demais condições ele vai para Route com o seu respectivo componente.

Instale o `prop-types`:
```
yarn add prop-types
```

Crie um arquivo chamando `src/routes/Route.js`.

E no arquivo `src/routes/index.js` substitua o Route do react-router-dom:

```
...
import { Switch } from 'react-router-dom';
...

import Route from './Route';

```

## Para mais detalhes veja o código.

Código: [https://github.com/marlonleite/gobarber-web](https://github.com/marlonleite/gobarber-web)