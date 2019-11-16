# Configurando um projeto React do 0.
Olá, me chamo Carlos Daniel, minha intenção com esse posta é explicar de uma vez por todas como configurar um projeto em React totalmente do zero, é necessário que você tenha familiaridade com o  [Terminal](https://pt.wikipedia.org/wiki/Terminal_(inform%C3%A1tica)) e com algumas ferramentas como [yarn](https://yarnpkg.com/pt-BR/) ou [npm](https://www.npmjs.com/).
## 1º Passo

Crie uma pasta para que possa armazenar todo o código do projeto
No ***Linux*** ou no ***MAC*** execute o comando:

```bash
  mkdir first-react-project && cd first-react-project
```

## 2º Passo

Dentro da pasta inicialize o Packager:

```bash
  yarn init -y
```

> Estou utilizando o `yarn`, mas você pode utilizar 
> também o `npm`, fica à seu critério :)

## 3º Passo

Instalando as dependências necessárias para criar um projeto **React** do zero:

```bash
 yarn add @babel/core @babel/preset-env @babel/preset-react webpack webpack-cli webpack-dev-server @babel/plugin-proposal-class-properties -D
```

Instalando os ***Loaders***

```bash
yarn add babel-loader style-loader css-loader file-loader -D
```
> `babel-loader` é o responsável por transpilar o código para que todos 
> os navegadores entendam.

> `css-loader` é responsável por carregar todos os arquivos de css.

> `file-loader` é responsável por carregar arquivos (.png, .jpg ...etc)

> `style-loader` é responsável por carregar os arquivos de estilo(css) dentro
> de arquivos *javascript*

> Todos esses loaders serão utilizados na configuração do *webpack* que
> acabamos de instalar.

> Utilize a flag `-D` no final para que essas dependências fiquem registradas 
> como dependências de desenvolvimento.

## 4º Passo

Agora instale o **React** e o **React-DOM** para poder construir sua aplicação
Utilizando o **React**.

```bash
yarn add react react-dom prop-types
```

## 5º Passo

### Configurando o ***Babel***:

> O ***Babel*** nos permite utilizar a nova *sintaxe* do *Javascript*
> em todos os navegadores sem sofrer problemas de incompatibilidade com
> essa *sintaxe* nova que utilizamos quando estamos criando aplicações
> com o **React**.

Na raiz do seu *projeto* crie um arquivo chamado *babel.config.js* ou rode 
o comando a seguir no terminal(caso você use ***linux***):

```bash
 touch babel.config.js
```

Agora vamos editar esse arquivo que acabamos de criar, deixe ele da seguinte maneira:

```js
module.exports = {
  presets: [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  plugins : ['@babel/plugin-proposal-class-properties']
}
```
> O `@babel/preset-env` é responsável por, digamos assim, traduzir a nova *sintaxe*
> do *Javascript* pro navegador, features como `import`, `export`, as 
> *arrow-functions*: `()=>{}` e etc, já o `@babel/preset-react`, é responsável
> por fazer com que o navegador entenda as coisas do **React** propriamente dito
> como, por exemplo código *JSX*(a mistura de *Javascript* com *HTML*).

> O `@babel/plugin-proposal-class-properties` permite que possamos
> declarar variáveis de `state` fora do método `constructor`, sem
> plugin teriamos que escrever algo mais o menos assim:.

```jsx
import React, {Component} from 'react';

export default class Teste extends Component{
  constructor(){
    this.state = {
      // algum state
    }
  }
  render(){
    return <h1>Teste</h1>
  }
}
```
> Mas com esse plugin podemos simplesmente escrever assim:

```jsx
import React, {Component} from 'react';

export default class Teste extends Component{
  state = {
    // algum state
  }
  render(){
    return <h1>Teste</h1>
  }
}
```
## 6º passo

### Configurando o ***Webpack***

O webpack fica com a responsabilidade de *live-reload*, então basicamente
ele fica escutando tudo o que fazemos no nosso código, e também fica com a 
responsabilidade de transpilar o código e mover eles pra pasta correta
para que nossa aplicação funcione belezinha.

Antes de tudo é importante configuramos a estrutura do projeto.

Partindo do pressuposto que todo nosso código deva ficar dentro de uma pasta 
chamada `src`(*source*), na raiz  do projeto criamos uma pasta chamada `src`, 
dentro dessa pasta `src` fica todo nosso código inclusive o nosso 
*entry-point*(ponto de entrada) o arquivo *index.js*.

No linux rode esse comando na raiz do projeto e fica tudo certo :)

```bash
 mkdir src && touch src/index.js
```

### Criando o diretório do código transpilado

Bem, agora vamos configurar a *pasta* onde vai ficar todo o código que o webpack
vai pegar da nossa aplicação transpilar.

Na raiz do projeto crie uma *pasta* chamada `public`, e fim, temos a pasta
onde vai ficar todo o nosso código transpilado.

```bash
mkdir public
```

### Voltando a configuração do ***Webpack***
Ainda na raiz do seu *projeto* crie um arquivo chamado *webpack.config.js*:

Caso esteja no linux é só rodar:

```bash
 touch webpack.config.js
```

Edite esse arquivo que acabamos de criar, e é importante deixar exatamente assim:

```js
  const path = require('path');
  module.exports = {
    entry: path.resolve(__dirname, 'src', 'index.js'),
    output: {
      path: path.resolve(__dirname, 'public'),
      filename: 'bundle.js'
    },
    devServer: {
      contentBase: path.resolve(__dirname, 'public')
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader'
          }
        },
        {
          test: /\.css$/,
          use: [{ loader: 'style-loader'}, { loader: 'css-loader'}]
        },
        {
          test: /.*\.(gif|png|jpe?g)$/i,
          use: {loader: 'file-loader'}
        }
      ]
    }
  }
```

Você deve estar se perguntando, ok, o que significa toda essa configuração ?

Vamos por partes:

### entry
> Aqui informamos pro *webpack* onde estamos escrevendo o código.

### output
  #### path
> *output/path* é onde informamos o caminho para a pasta que configuramos,
> lá vai ficar o código transpilado(o código para que todos os navegadores entendam).

### output
  #### filename
> *output/filename* é onde informamos como vai se chamar o arquivo onde vai
> ficar todo o código transpilado

### module
  #### rules
> Nas *module/rules* é onde nós dizemos pro webpack quem faz o quê, ex:
> o *babel-loader* transpila o código da *sintaxe* nova do *javascript*.

## Rules

Pois bem, temos várias `Rules`, mas vou explicar só uma que o resto fica fácil
de entender.

Dentro do objeto de configuração do *webpack*,  nos deparamos com uma `key`
chamada `module`, essa `key` é um objeto que contem várias configurações,
uma delas é a `key` que chamamos de `rules`, essa `key` contem todas as 
instruções para o *webpack* fazer tudo direitinho, dentro de `rules`, temos um
`array` de objetos, dentro de cada objeto temos mais algumas `keys`, sendo elas
`test` que recebe uma expressão regular que consegue pegar todos os arquivos e utilizar 
seus devidos *loaders*, `exclude`, onde dizemos pro *webpack*
o que não transpilar, e por fim `use`, onde informamos para o *webpack* o
`loader` que nós vamos utilizar.

## Dev Server

A `key` `devServer`, informa o path da pasta `public` onde ele encontra o 
nosso `index.html`, possibilitando asssim o Live Reload.

## 7º passo

Configurando os `scripts` da nossa aplicação

Dentro do seu `package.json`, logo após a `key` `license`, crie uma `key`
com o nome `scripts`, essa key deve ser um objeto.

Agora vamos criar o script de `build` da nossa aplicação, o que vai fazer
toda a configuração que a gente fez funcionar.

E agora criamos também o script de `dev` que serve para *subir* o servidor,
e fazer a mágica acontecer.

ex dentro do `package.json`:
```json
{
  "name": "first-react-project",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "webpack --mode production",
    "dev": "webpack-dev-server --mode development"
  },
  "devDependencies": {
    "@babel/core": "^7.7.2",
    "@babel/plugin-proposal-class-properties": "^7.7.0",
    "@babel/preset-env": "^7.7.1",
    "@babel/preset-react": "^7.7.0",
    "babel-loader": "^8.0.6",
    "css-loader": "^3.2.0",
    "file-loader": "^4.2.0",
    "style-loader": "^1.0.0",
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.10",
    "webpack-dev-server": "^3.9.0"
  },
  "dependencies": {
    "prop-types": "^15.7.2",
    "react": "^16.11.0",
    "react-dom": "^16.11.0"
  }
}
```
## 7.1

Execute no terminal:

```bash
yarn build
```
## 8º Passo

Vamos voltar um pouquinho antes de continuar, lembra daquela pasta `public` que
nós criamos? Dentro dela vamos criar um arquivo `index.html` onde vamos instanciar
o arquivo `bundle.js` que configuramos lá no `webpack`.

Na raiz do projeto rode:
```bash
touch public/index.html
```

Edite o arquivo `index.html` e deixe-o assim:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>RecactJS</title>
</head>
<body>
  <div id="App"></div>
  <script src="./bundle.js"></script>
</body>
</html>
```

> O que fizemos aqui foi uma simples estrutura `html` e carregamos o arquivo
> `bundle.js` para dentro desse nosso `index.html`, e também criamos uma `div`
> com um atributo`id="App"`.


## Pronto temos nosso projeto Configurado

### Agora vamos para o nosso Hello World em ***REACT***

Abra um terminal e execute o seguinte comando:

```bash
yarn dev
```

Agora abra seu editor de código favorito e #boraCodar

## Hello React!
Agora crie um arquivo chamado `App.js` dentro da pasta `src`, não precisa fazer
nada com ele agora.

Agora dentro da pasta `src` tem um arquivo chamado `index.js`, se você seguiu todos
os passos até aqui ele deve estar em branco, vamos editar ele agora, deixe-o
exatamente assim: 

```jsx
import React from 'react';

import {render} from 'react-dom';

import App from './App';

render(<App />, document.getElementById('App'));
```

Certo, o que fizemos aqui ?

1. Importamos o `React` de dentro da lib `react`.

2. Importamos o método `render` de dentro da lib `react-dom`.

3. Importamos nosso `App.js`

4. Usamos a função `render` para renderizar o nosso arquivo `App.js` dentro da div que nós criamos naquele arquivo `index.html`.

Agora vamos para o arquivo `App.js`

Deixe o arquivo `App.js` assim:

```jsx

import React from 'react';

function App(){
  return <h1>Hello React</h1>
}
export default App

```

Aqui nós simplemente retornamos o texto `Hello React` dentro de um `h1`.

Lembra que eu disse pra você executar o comando:

```bash
yarn dev
```

Pois bem, observe a saída desse comando no terminal, principalmente as primeiras
linhas, elas devem retornar algo parecido com isso:

```bash
yarn run v1.17.3
$ webpack-dev-server --mode development
ℹ ｢wds｣: Project is running at http://localhost:8080/
ℹ ｢wds｣: webpack output is served from /
```

Abra este endereço no seu navegador [HelloReact](http://localhost:8080/), ou
http://localhost:8080/.

e Fim :)
