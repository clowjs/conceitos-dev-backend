[www.whimsical.com](http://www.whimsical.com)

[www.devdocs.io](http://www.devdocs.io)

### Criando/iniciando projeto node:

1. **mkdir/cd para diretorio do projeto**
2. **yarn init -y**
3. **yarn add express**

```jsx
// Criando constante 'express' para dar import no express
const express = require('express');

// Criando constante 'app' que sera nossa aplicacao, usando o express()
const app = express();

// Criando uma rota, no caso '/projects' e respondendo com o texto 'Hello World'
app.get('/projects', (request, response) => {
    return response.send('Hello World');
});

// Fazendo o node 'escutar' pela porta 3333
app.listen(3333);
```

Iniciar o node, rodando nosso index.js:

- **node src/index.js**

### Configurando Nodemon:

Ferramenta que faz com que o node seja reiniciado automaticamente sempre que realizarmos alteracoes no codigo fonte da aplicação.

Instalando nodemon no nosso projeto:

- **yarn add nodemon -D**

-D: Apenas para ambiente de desenvolvimento, não sera instalado/usando no servidor final

Executando projeto utilizando o nodemon

- **yarn nodemon src/index.js**

Para facilitar desenvolvimento, podemos adicionar "atalhos" de comandos no package.json:

```json
"main": "src/index.js", // mudamos o caminho padrao da aplicacao para src/index.js
"scripts": {
    "dev": "nodemon" // criamos o atalho "dev"
},
```

Dessa forma, só precisamos usar o comando "**yarn dev**" para rodar nossa aplicação utilizando o nodemon.

- **yarn dev**
