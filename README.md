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
// mudamos o caminho padrao da aplicacao para src/index.js
"main": "src/index.js",
// criamos o atalho "dev"
"scripts": {
    "dev": "nodemon"
},
```

Dessa forma, só precisamos usar o comando "**yarn dev**" para rodar nossa aplicação utilizando o nodemon.

- **yarn dev**

### Métodos HTTP:

- **GET**: Buscar informações do back-end
- **POST**: Criar uma informação no back-end
- **PUT**/**PATCH**: Alterar uma informação no back-end
- **DELETE**: Deletar uma informação no back-end

### Tipos de parâmetros:

- **Query Params:** Filtros e paginação

```jsx
const { title, owner } = request.query;
console.log(title);
console.log(owner);
```

- **Route Params:** Identificar recursos (Atualizar/Deletar)

```jsx
const { id } = request.params;
console.log(id);
```

- **Request Params:** Conteúdo na hora de criar ou editar um recurso (JSON)

Necessário adicionar a proxima linha antes da criação das rotas, para que a função .json():

```jsx
app.use(express.json());
```

```jsx
const body = request.body;
console.log(body);
```

### Aplicação Funcional

Importanto uuidv4 (universal unique id):

- **yarn add uuidv4**

Importando uuidv4 logo apos o express:

```jsx
const { uuid } = require('uuidv4');
const projects = [];
```

**POST:**

```jsx
const project = { id: uuid(), title, owner };
projects.push(project);
```

**PUT:**

```jsx
const projectIndex = projects.findIndex(project => project.id === id);
if (projectIndex < 0) {
	return response.status(400).json({ "error": "Project not found."})
}
```

**GET:**

```jsx
const { title } = request.query;
const results = title 
		? projects.filter(project => project.title.includes(title))
		: projects;
return response.json(results);
```

## Middleware

Interceptador de requisições que pode interromper totalmente ou alterar dados da requisição.

```jsx
const { uuid, isUuid } = require('uuidv4');
```

```jsx
function logRequests(request, response, next) {
    const { method, url } = request;

    const logLabel = `[${method.toUpperCase()}] ${url}`;
    
    console.time(logLabel);
    // return next(); // Proximo middleware
    next();
    
    console.timeEnd(logLabel);
}

function validateProjectId(request, response, next) {
    const { id } = request.params;

    if (!isUuid(id)) {
        return response.status(400).json({ error: "Invalid project id."});
    }

    return next();
}
```

```jsx
app.use(logRequests);
app.use('/projects/:id', validateProjectId);
```
