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

## Aula 06 - Layouts por página

Instale o styled-components:

```
yarn add styled-components
```

No caminho `src/routes/Route.js` adicione os layouts AuthLayout e DefaultLayout:
```
...
import AuthLayout from '../pages/_layouts/auth';
import DefaultLayout from '../pages/_layouts/default';
...

...
const Layout = signed ? DefaultLayout : AuthLayout;
...

```

## Aula 07 - Estilos globais

Vamos usar o `createGlobalStyle` do styled-component para gerar o estilo global da aplicação, criar `global.js` no caminho:

```
src/styles/global.js

```

E aplicar no App.js

```
...
import GlobalStyle from './styles/global';
...

function App() {
  return (
    <Router history={history}>
      <Routes />
      <GlobalStyle />
    </Router>
  );
}

export default App;
```

## Aula 08 - Utilizando Root import

Ao invés de ficarmos fazendo import passando `../../../MinhaPasta/MeuArquivo.js` podemos fazer algo melhor utilizando uma lib para resolver os caminhos pra gente, mas antes disso precisamos customizar o Create React App para configurar o babel.

Para isso vamos instalar:
```
yarn add customize-cra react-app-rewired -D
```

E também vamos instalar:
```
yarn add babel-plugin-root-import -D
```

E criar um arquivo `config-overrides.js` na raiz do projeto.

Estamos importando a função addBabelPlugin e override, vamos exportar essa configuração, sobrescrevendo com o override passando o plugin do babel que quremos em um array, o primeiro parâmetro é o nome do plugin e depois um objeto com a configuração, nesse caso passamos o rootPathSuffix `src` que é onde estão os nosso códigos javascript do projeto onde iremos fazer import e export.

Com isso agora podemos fazer os imports assim:

```
import AuthLayout from '~/pages/_layouts/auth';
import DefaultLayout from '~/pages/_layouts/default';
```

Esse sinal de `~` representa qualquer nível `../../../` até o `src` como sendo o primeiro nível.

E para testar e funcionar vamos trocar os scripts do package.json:

```
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject",
    "lint": "eslint --fix src --ext .js"
  },
```

e agora só executar `yarn start` e ver se tudo funcionou! =)

Mas o eslint está chiando alguns erros aqui...

Vamos instalar essa lib com o nome gigante para que o eslint ententa o `~` que estamos usando para fazer o import.

```
yarn add eslint-import-resolver-babel-plugin-root-import
```

e no arquivo `.eslintrc.js` colocamos um configuração:

```
settings: {
    'import/resolver': {
      'babel-plugin-root-import': {
        rootPathSuffix: 'src',
      },
    },
  },
```

Agora o eslint não reclama mais, só rodar `yarn lint` no terminal.

Mas agora se pressionar o teclado ctrl ou cmd e clicar no caminho do arquivo, ele não está acessando mais o arquivo.

```
import AuthLayout from  '~/pages/_layouts/auth';
```

Para resolver isso criaremos um arquivo `jsconfig.json` com o seguinte conteúdo:

```
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": { "~/*": ["*"] }
  }
}
```

Agora ele vai entender as importações e conseguiremos navegar nos as arquivos.

## Aula 09 - Estilização da página de autenticação

Vamos estilizar a página de Login e cadastro da aplicação.

Instalar o `polished`:

```
yarn add polished
```

## Aula 10 - Utilizando Unform

Para fazer a manipulação de estados precisaríamos converter a função em classe ou usar os Hooks e criar uma estado para cada input do formulário. A Rocketseat criou uma lib performática para lidar com formulários, [https://github.com/Rocketseat/unform](https://github.com/Rocketseat/unform), que iremos usar nesse projeto, vamos instalar então:

```
yarn add @rocketseat/unform
```

E agora só utilizar:

```
...
import { Form, Input } from '@rocketseat/unform';
...

...
<Form onSubmit={handleSubmit}>
        <Input name="email" type="email" placeholder="Seu e-mail" />
        <Input
          name="password"
          type="password"
          placeholder="Sua senha secreta"
        />

        <button type="submit">Acessar</button>
        <Link to="/register">Criar conta gratuíta</Link>
      </Form>
...
```

Muito interessante que basta passar o atributo `name` nos inputs e colocar um `onSubmit` com uma função e todos os dados preenchidos no formulário serão passados para a função na variável `data` podemos ter acesso a todos valores preenchidos.

Veja como ficou limpo, não precisei de um:

```
const [name, setName] = useState('');
const [email, setEmail] = useState('');
const [password, setPassword] = useState('');
```

e nem precisa fazer `setPassword(e.target.value)`, etc.

O que já era bom ficou ainda melhor!

Outras alternativas no mercado para lidar com formulário:

* [https://jaredpalmer.com/formik/docs/overview](https://jaredpalmer.com/formik/docs/overview)
* [https://redux-form.com/](https://redux-form.com/)
* [https://react-hook-form.com](https://react-hook-form.com/)

React Hook Form me parece uma boa, além do Unform da Rocketseat.

## Aula 11 - Validações

Vamos instalar e utilizar a biblioteca [yup](https://github.com/jquense/yup) que ajuda muito a validar tanto o frontend quanto o backend.

```
yarn add yup
```

O yup usa o padrão de schema validation, ela foi criada inspirada pelo [joi](https://github.com/hapijs/joi). Que é uma lib muito legal também para validação.

Vamos validar o SignUp:

```
import  * as Yup from  'yup';

const schema = Yup.object().shape({
  name: Yup.string().required('O nome é obrigatório'),
  email: Yup.string()
    .email('Insira um email válido')
    .required('O Email é obrigatório'),
  password: Yup.string()
    .min(6, 'A senha precisa de ter 6 caracteres no mínimo')
    .required('A Senha é obrigatória'),
});

<Form  schema={schema}  onSubmit={handleSubmit}>
...
</Form>
```

Pronto, que legal, estamos com validação em cada campo usando schema validation.

Quando dá algum erro é lançado uma mensagem de erro na tela do usuário conforme escrevemos no código, e podemos estilizar esse span, e é o que fizemos aqui:

`src/pages/_layout/auth/styles.js`:
```
  span {
      color: #fb6f91;
      align-self: flex-start;
      margin: 0 0 10px;
      font-weight: bold;
    }
```

Agora a mensagem aparece na tela bem mais bonita!

## Para mais detalhes veja o código.

Código: [https://github.com/marlonleite/gobarber-web](https://github.com/marlonleite/gobarber-web)
