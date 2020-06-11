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

## Aula 12 - Configurando Store do Redux

A autenticação do usuário vamos fazer pelo Redux e armazenar o token e os dados do usuário logado, e acessar esses dados em vários locais.

Vamos instalar todas as libs necessárias:
```
yarn add redux redux-saga react-redux reactotron-redux reactotron-redux-saga immer
```

## Aula 13 -Autenticação

Implementação do login, porém ainda não estou salvando os dados do usuário e nem o token permanentemente em uma local, quando eu faço um refresh na página perco o store e o dados.

```
yarn add axios
```

## Aula 14 - Armazenando perfil

Vamos armazenar os dados do usuário em um outro reducer chamado user.

Dois reducers podem ouvir as mesmas actions, tanto o user quanto auth irão ouvir a action: @auth/SIGN_IN_SUCCESS e vão fazer alguma coisa com esse dado, ambas vão armazenar no seu estado, os respectivos valores que lhe fazem sentido. Embora o user venha na autenticação eu amarzeno ele na user pois quando altrerarmos o perfil não estaremos lidando om autenticação.

## Aula 15 - Persistindo autenticação

Vamos armazenas os dados de autenticação no storage do navegador para que não se perca quando fizer refresh no navegador.

Vamos utilizar uma lib para auxiliar com isso:
```
yarn add redux-persist
```

O [redux-persist](https://github.com/rt2zz/redux-persist) pode ser integrado ao localStorage do navegador, AsyncStorage do App Mobile com React Native e também com alguns bancos locais.

Então vamos integrar com a nossa aplicação:

Criamos um arquivo `persistReducers.js` dentro de `src/store`:

```
import storage from 'redux-persist/lib/storage';
import { persistReducer } from 'redux-persist';

export default reducers => {
  const persistedReducer = persistReducer(
    {
      key: 'gobarber',
      storage,
      whitelist: ['auth', 'user'],
    },
    reducers
  );

  return persistedReducer;
};
```

Importamos o storage do redux-persist e também o persistReducer do redux-persist.

e criamos uma função que recebe os reducers como parâmetro, criamos uma const persistedReducers que será o retorno da função. com o método persistReducer passamos um objeto de configuração contendo a chave que será o nome da chave do localStorage, passamos a implementação do storage que importamos do redux-persist e também passamos uma whitelist que são os nomes dos reducers que iremos amarzenar (persistir) no localStorage. E por fim retornamos a função. Tudo isso está na documentação da biblioteca.

Agora vamos usar essa função no arquivo `index.js` do `src/store`:

```
import { persistStore } from 'redux-persist';
import createSagaMiddleware from 'redux-saga';
import createStore from './createStore';
import persistReducers from './persistReducers';
import rootReducer from './modules/rootReducer';
import rootSaga from './modules/rootSaga';

const middlewares = [];

const sagaMonitor =
  process.env.NODE_ENV === 'development'
    ? console.tron.createSagaMonitor()
    : null;

const sagaMiddleware = createSagaMiddleware({ sagaMonitor });

middlewares.push(sagaMiddleware);

const store = createStore(persistReducers(rootReducer), middlewares);
const persistor = persistStore(store);

sagaMiddleware.run(rootSaga);

export { store, persistor };
```

Eu importei `persistStore` da lib `redux-persist` e o `persistReducers` do arquivo `persistReducers` que criamos.

Invocamos a função perstiReducers passando o rootReducer com todos os reducers, na criação do store:

```
const store = createStore(persistReducers(rootReducer), middlewares);
```

Criamos uma constante persistor que recebe um objeto que é retornado do persistStore(store), que é responsável pela reidratação de pegar os dados do localStorage e disponibilizar no reducers.

```
const persistor = persistStore(store);
```

E por fim exportamos duas variáveis, a store e persistor:

```
export { store, persistor };
```

E com isso a aplicação vai quebrar devido a esse export não ser mais default, temos que importar individualmente o store e o persistor.

Então no no Route.js e no App.js corrigimos isso:

`Route.js`:
```
...
import { store } from  '~/store';
...
```

`App.js`:
```
import './config/ReactotronConfig';
import React from 'react';
import { PersistGate } from 'redux-persist/integration/react';
import { Router } from 'react-router-dom';
import { Provider } from 'react-redux';
import Routes from './routes';
import history from './services/history';
import GlobalStyle from './styles/global';
import { store, persistor } from './store';

function App() {
  return (
    <Provider store={store}>
      <PersistGate persistor={persistor}>
        <Router history={history}>
          <Routes />
          <GlobalStyle />
        </Router>
      </PersistGate>
    </Provider>
  );
}

export default App;
```

Aqui mudamos um pouco, importamos o store e persistor e também o component PersistGate do redux-persist.

Envolvemos a aplicação sobre o PersistGate passando a propriedade persistor com o nosso persistor.

```
 <PersistGate persistor={persistor}>
   <Router history={history}>
      <Routes />
      <GlobalStyle />
   </Router>
</PersistGate>
```

Agora a aplicação volta a funcionar, quando o usário logar na aplicação todo o fluxo de persistir no localStorage os reducers da whiteList vai funcionar, e quando sair da aplicação  ele continuará logado, pois ele perde os dados e busca do localStorage.

LocalStorage:
```
{,…}
auth: "{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTAsImlhdCI6MTU3MDYyOTY0NywiZXhwIjoxNTcxMjM0NDQ3fQ.8_7pqc7Ds2yudoPNr0r3NO_DnH2t9fgMOwxvIRuqwRs","signed":true,"loading":false}"
user: "{"profile":{"id":10,"name":"Avatolino Cliente","email":"avatar@gmail.com","provider":true,"avatar":null}}"
_persist: "{"version":-1,"rehydrated":true}"
```
![](https://github.com/marlonleite/images/blob/master/bootcamp-rocketseat/gobarber-web-localstorage-redux-persist.png?raw=true)

Reactotron:

![](https://github.com/marlonleite/images/blob/master/bootcamp-rocketseat/gobarber-web-reactotron-redux-persist.png?raw=true)

Pronto, agora o fluxo de login ficou bem automatizado.

## Aula 16 - Loading da Autenticação

* Refatorei o reducers para não ter que ficar escrevendo toda vez:
```
return  produce(state, draft  => { ... }
```

* Criei mais dois cases no reducerde auth para REQUEST, SUCESSO e FAILURE.
* No SignIn peguei usando o useSelector, o estado do loading e fiz uma condição ternária para mostrar 'Carregando...' se o loading estiver `true` e 'Acessar' quando estiver `false`.
* No sagas eu coloquei um try/catch para capturar algum erro da API e disparar uma action de falha no signIn.

## Aula 17 - Exibindo toasts

* Instalei a lib: `yarn add react-toastify` para enviar mensagens para o usuarío
* Adicionei o componente ToastContainer no `App.js` com auto fechamento de 3 segundos
* importei o estilos do react-toastify: `import  'react-toastify/dist/ReactToastify.css';` no arquivo `global.js`
* No saga de auth removi os `console.log` e adicionei: `toast.error('usuario nao é prestador de servico');`

## Aula 18 - Cadastro na aplicação

Agora criar um cadastro novo do GoBarberWeb

* Criei uma action nova `signUpRequest` que recebe nome, email e senha do usuário
* No saga criei uma nova função signUp que ouve as chamadas da action de SIGN_UP_REQUEST:
```
export function* signUp({ payload }) {
  try {
    const { name, email, password } = payload;

    yield call(api.post, 'users', {
      name,
      email,
      password,
      provider: true,
    });

    history.push('/');
  } catch (err) {
    toast.error('Falha no cadastro verifique seus dados!');
    yield put(signFailure());
  }
}

export default all([
  takeLatest('@auth/SIGN_IN_REQUEST', signIn),
  takeLatest('@auth/SIGN_UP_REQUEST', signUp),
]);
```

* E no index.js do componente SignUp disparei a action com os dados do usuário.
