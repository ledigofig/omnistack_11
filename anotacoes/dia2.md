# Dia 2 - Desenvolvimento do backend da aplicação Be The Hero. 

Para instalar o comando `code` no terminal do sistema operacional no vscode tecla: `ctrl+shift+p` para abrir a caixa de comando do vscode e digite: `install code` é só clicar para instalar o recurso. 

**Rotas**: é os endereços que as aplicações tem para acessar determinados recusos. Exemplo: [`localhost:3333/users](http://localhost:3333/users)` 

**Recursos** são geralmente associados a uma tabela no banco de dados. No finalzinho da rota temos o nome dado ao recurso, exemplo na rota anterior é o `/users` 

**Métodos HTTP**

**GET**: utilizamos quando queremos buscar uma informação do backend, qualquer tipo de retorno de uma informação. 

**POST**: Utilizamos quando for para criar uma nova informação no backend, por exemplo, criar usuários, fazer o login, etc.

**PUT**: Utilizado para alterar uma informação no backend.

**DELETE**: Utilizado para deletar uma informação no backend.

O software **[Insomnia](https://insomnia.rest/download/)** auxiliar no desenvolvimento para testar as rotas e recursos da aplicação backend. 

**Tipos de parâmetros**: 

**Query Params**: São parâmentros nomeados, enviados na rota após "?" e geralmente são utilizados para buscas, filtros, paginação...

No exemplo abaixo vemos como pegar essas informação que é passar pela rota: 

O **request** ele guarda todos os dados que são enviados pelo usuário na requisição.

O **response** é a resposta que será enviada para a requisição enviada.

    // Supondo a seguinte rota: http://localhost:3333/users?evento=Omnistack11&nome=Emerson
    app.get('/users', (request, response) => {
    	const params = request.query; // Armazena todas as requisição (query params).
    	
    	return response.json({
    		evento: params.evento,
    		aluno: params.nome
    	}); // A resposta enviada será um objeto JSON com os dados enviados pela requisição.
    })

**Route Params:** Parâmetros utilizados para identificar recursos. Serve para identificar um único recurso, por exemplo `users/:id`  utilizado para identificar um usuário e na rota envia o número de id do usuário que deseja buscar.

Observação: Nesse tipo de requisição o backend só entenderá um valor depois de `/users`. Pois se adicionar algo a mais como `/users/1/cargo` o backend irá entender como uma outra rota.

    // buscar um usuários especifico
    // Supondo http://localhost:3333/users/1
    app.get('/users/:id', (request, response) => {
      const params = request.params; // Armazena os parâmetros. (route params).
      
      console.log(params); // retornar o id
    });

Request Body: É o corpo da requisição, utilizado para criar ou alterar recursos. 

Exemplo de uso de request body na criação de uma usuário. Nesse caso enviamos um JSON contendo as informações a serem cadastradas no banco de dados, mas como exemplo irá apenas mostrar um console.log() com os dados que foram enviados. 

JSON enviado:

    {
    	"nome": "Emerson Freitas",
    	"idade": 28
    }

    const express = require('express');
    
    const app = express();
    
    // devemos informar que tipo de dados a aplicação receberá 
    app.use(express.json());
    
    // criar um usuário
    app.post('/users/', (request, response) => {
      const body = request.body; // Armazena o corpo da requisição.
      
      console.log(body);  // dados recebidos, um objeto.
    
      return response.json({
        message: 'Usuário criado'
      });
    })
    
    app.listen(3333);

Para nos ajudar no desenvolvimento instale o pacote **nodemon**, para não ficarmos precisando reiniciar o node a cada alteração no código. 

`npm install nodemon -D`

O comando acima instalamos o pacote nodemon. O `-D` significa que estamos instalando a biblioteca apenas como dependencia de desenvolvimento. 

Depois configuramos um script dentro do arquivo package.json adicione o seguinte trecho dentro do arquivo.

    "start": "nodemon index.js"

Depois é só executar o projeto com o comando: `npm start`.

**Organização dos diretórios do projeto**

Criamos a pasta **src** onde ficará os arquivos de códigos criados por nós. 

Mova o arquivo `index.js` para dentro da pasta **src.** 

Configure novamente o diretório no arquivo `package.json` o script de execução do nosso backend.

    "start" : "nodemon src/index.js"

Para separar nossas rotas do arquivo `index.js` para não ficar um arquivo muito extenso, crie um arquivo dentro da pasta **src** com o nome `routes.js`.

Recortamos as rotas do arquivo i`ndex.js` e passamos para o arquivo `routes.js` e também precisamos fazer algumas configurações para que funcione perfeitamente. 

O arquivo `route.js` ficará assim: 

    // importamos o express.
    const express = require('express');
    
    // utilizamos somente a classe de Rotas do express.
    const routes = express.Router();
    
    // renomeamos de app para routes.
    routes.post('/ongs', async (request, response) => {
      return response.json();
    });
    
    // deixamos nossas rotas disponíves para serem importadas.
    // então exportamos routes.
    module.exports = routes;

O arquivo index.js ficará assim: 

    const express = require('express');
    const routes = require('./routes'); // importamos nossas rotas
    
    const app = express();
    
    app.use(express.json());
    app.use(routes); // deve ser utilizado depois as configurações acima 
    
    app.listen(3333);

**Banco de dados**

**SQL**:

- MySQL
- SQLite
- PostgreSQL
- Oracle
- Microsoft SQL Server

SQL é a linguagem utilizada para nos comunicar com o banco de dados (universal), cada banco de dados podem ter algumas pequenas diferenças. 

**NoSQL**: 

- MongoDB
- Redis
- Amazon DynamoDB

NoSQL são banco de dados não relacional, cada banco de dados tem suas peculiaridades, ou seja, pode haver diferença de empresa para empresa na estrutura do banco de dados.

---

Driver: `SELECT * FROM users`;

Query Builder: `table('users').select('*').where() // Abordagem com Javascript.` 

Instalação do **[KNEX](http://knexjs.org/)** Query Builder Javascript para banco de dados SQL.

`npm install knex`

Já que utilizaremos SQLite então instale:

`npm install sqlite3`

Agora para fazer a conexão com o banco de dados, começamos executando o comando:

`npx knex init`

Após executar o comando acima irá criar um arquivo `knexfile.js` para configuração da conexão com o banco de dados.

Nesse arquivo temos as configurações:

- **development**: para a configuração de um banco local para trabalharmos com o desenvolvimento da aplicação.
- **staging**: um banco de dados para simular um o banco de produção da aplicação.
- **production**: configuração do banco de dados de produção, quando a aplicação é disponibilizada para ser usada.

Crie uma pasta chamada **database** dentro da pasta **src**.

Dentro da pasta **database** crie uma pasta chamada **migrations** é onde ficará salvas nossas migrations.

Nesse caso configuraremos o development

    development: {
      client: 'sqlite3',
      connection: {
        filename: './src/database/db.sqlite' // diretório do banco de dados.
      },
      migrations: {
    	  directory: './src/database/migrations' // diretório das migrations.
      },
    	useNullAsDefault: true, // Por padrão o SQLite não aceita inserção de dados vázio.
    },

**Entidades e Funcionalidades da aplicação Be The Hero**

**Entidades da aplicação**

- ONG
- Caso (incident)

**Funcionalidades**

- Login de ONG
- Logout de ONG
- Cadastro de ONG
- Cadastrar novos casos
- Listar casos especificos de uma ONG
- Deletar cados
- Listar todos os casos
- Entrar em constato com a ONG

**Criando migrations no KNEX** 

Migrations são a listagem de tabelas do nosso banco de dados. 

Para criar uma nova migration: `knex migrate:make migration_name`

Nossa migration executamos o seguinte comando: 

`npx knex migrate:make create_ongs`

**As migrations estão no diretório que foi configurado no arquivo `knexfile.js`**

Despois de criada a migration, então escrevemos como será nossa tabela: 

    // método para criar a tabela.
    exports.up = function(knex) {
      return knex.schema.createTable('ongs', function(table){
        table.string('id').primary();
        table.string('name').notNullable();
        table.string('email').notNullable();
        table.string('whatsapp').notNullable();
        table.string('city').notNullable();
        table.string('uf', 2).notNullable();
      });
    };
    // método para excluir a tabela em casos de erros.
    exports.down = function(knex) {
      return knex.schema.dropTable('ongs');
    };

Agora é só executar o comando: 

`npx knex migrate:latest` (Esse comando também criará o banco de dados como foi configurado no arquivo `knexfile.js`)

Mais uma migrate para a aplicação: 

`npx knex migrate:make create_incidents`

E o arquivo da migration: 

    exports.up = function(knex) {
      return knex.schema.createTable('incidents', function(table){
        table.increments();
    
        table.string('title').notNullable();
        table.string('description').notNullable();
        table.decimal('value').notNullable();
        
    		// chave estrangeira
        table.string('ong_id').notNullable();
    		
    		// referenciando a chave estrangeira com a tabela de ongs
        table.foreign('ong_id').references('id').inTable('ongs');
      });
    };
    
    exports.down = function(knex) {
      return knex.schema.dropTable('incidents');
    };

Depois de configurada a migration então executamos:

`npx knex migrate:latest`

Obs.: Em caso de erros com a migrations executamos o comando: `npx knex migrate:rollback` para desfazer a última alteração feita em nossas migrations. Para visualizar mais comando do knex cli: `npx knex` que mostrará no terminal as opções disponíveis.

Dentro do diretório de **database** criamos o arquivo `connection.js` 

    //importamos o knex.
    const knex = require('knex');
    
    // pegando as configurações do arquivo gerado pelo knex.
    const configuration = require('../../knexfile');
    
    // pegamos as configurações de conexão de ambiente de desenvolvimento.
    const connection = knex(configuration.development);
    
    // exportamos a conexão.
    module.exports = connection;

Agora vamos separar o lógica em um arquivo apropriado para isso.

Dentro da pasta **src** crie uma pasta chamada **controllers**.

Na pasta controllers crie o arquivo `OngController.js`.

O arquivo `routes.js` deve ficar assim: 

    const express = require('express');
    
    // Aqui já importamos o controller que será utilizado nas rotas.
    const OngController = require('./controllers/OngController');
    
    const routes = express.Router();
    
    // exportamos nossas rotas
    module.exports = routes;

**Cadastrando ONGs no Banco de Dados**

Para o cadastro de uma ONG acrescentamos os seguintes trechos em nosso arquivo `OngController.js`. 

    // importamos o pacote de criptografia que já vem com o Node.js.
    const crypto = require('crypto');
    
    // importamos o arquivo de configuração de conexão com o banco de dados.
    const connection = require('../database/connection');
    
    // devemos deixar esses métodos exportaveis para serem utilizado no arquivo de rotas.
    module.exports = {
    	// criamos uma função de cadastro de ONG.
      async create(request, response) {
    		// usamos a destruturação para pegar somente o que precisamos do corpo da requisição.
        const {name, email, whatsapp, city, uf} = request.body; 
    	  
    		// geramos um id aleatório de 4 bytes e depois convertido para string hexadecimal.
        const id = crypto.randomBytes(4).toString('HEX');
    		
    		// método de inserção das informações no banco de dados na tabela ongs. Query Builder.
    		// utilizamos o await antes para que o Javascript espera até que o método insert seja conclído.
        await connection('ongs').insert({
          id,
          name,
          email,
          whatsapp,
          city,
          uf,
        });
    		
    		// depois da inserção na tabela ongs concluída retorna o id da ong.
        return response.json({ id });
      },
    }

Depois adicionamos essa função na rota de cadastro: 

    // Adicionamos como segundo parâmetro da rota o função de cadastro de uma ONG.
    routes.post('/ongs', OngController.create);

Veja que utilizamos a rota como post para cadastro de uma ong.

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/ongs` utilizando o método POST no Insomnia e enviando o corpo da requisição um corpo do tipo JSON.

**Listando ONGs**

Adicione o método em `OngController.js`: 

Lembrando que as funções devem estar dentro do `module.exports = { //funções aqui }` e separadas por vírgula.

    // função de listagem de ong
    async index(request, response) => {
    	// fazemos a consulta no banco de dados utilizando o método select para buscar todas
    	// as ongs cadastradas.
      const ongs = await connection('ongs').select('*');
    	
    	// retornar um array de objetos, sendo um objeto para cada ong.
      return response.json(ongs);
    });

Depois adicionamos na rota: 

    // como segundo parâmetro passamos o função de listagem de ongs.
    routes.get('/ongs', OngController.index);

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/ongs` utilizando o método GET retornando um array de objetos.

Criamos agora o controller `IncidentController.js`.

**Cadastro de Caso**

A nele adicionamos a função de **cadastrar um caso**: 

    // Assim como o OngController.js precisamos importar a conexão com o banco de dados.
    const connection = require('../database/connection');
    
    module.exports = {
      async create(request, response) {
        const {title, description, value} = request.body;
    
    		// para pegar dados do header da requisição, nesse caso o id da ong logada.
        const ong_id = request.headers.authorization;
    		
    		// fazendo a destruturação para pegar o id do retorno dessa query builder.
    		// essa query builder retorna um array com o caso que foi cadastrado.
        const [id] = await connection('incidents').insert({
          title,
          description,
          value,
          ong_id,
        });
    		
    		// após a inserção do caso é retornado o id do caso.
        return response.json({ id });
      },
    };

E logo depois definimos a rota para o cadastro de caso. Adicione o trecho no arquivo `routes.js`.

    routes.post('/incidents', IncidentController.create);

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/incidents` utilizando o método POST no Insomnia e enviando o corpo da requisição um corpo do tipo JSON e o cabeçalho da requisição preenchido com o id da ong que está logada.

**Listagem dos casos**

No arquivo `IncidentsController.js` adicionamos mais uma função, função de listar os casos.

    async index(request, response) {
      const incidents = await connection('incidents').select('*');
    
    	return response.json(incidents);
     },

Lembrando que as funções devem estar dentro do `module.exports = { //funções aqui }` e separadas por vírgula.

No arquivo `routes.js` adicionamos a seguinte rota:

    routes.get('/incidents', IncidentController.index);

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/incidents` utilizando o método GET retornando um array de objetos.

**Deletando um caso**

Adicionamos a função delete no arquivo `IncidentController.js`: 

    async delete(request, response) {
    		// pegando o id enviado via route params.
        const { id } = request.params;
    		// pegando o id da ong que está logada, dado vem do cabeçalho da requisição.
        const ong_id = request.headers.authorization;
    		
    		// buscando o caso de acordo com os dados que chegaram por requisição.
        const incident = await connection('incidents')
          .where('id', id)
          .select('ong_id')
          .first();
    		
    		// verificando se o caso é realmente da ong logada para realizar tal operação.
        if(incident.ong_id !== ong_id) {
    			// caso o id da ong seja diferente do id da ong cadastrada
    			// então retorna um status code 401 - não autorizado.
          return response.status(401).json({ error: 'Operation not permitted.' });
        }
    		
    		// se passar a verificação então o caso é deletado.
        await connection('incidents').where('id', id).delete();
    		
    		// retorna um status code 204 - não tem conteúdo, porém ocorreu com sucesso.
        return response.status(204).send();
    
      },

Adicionando a rota em `routes.js`.

    routes.delete('/incidents/:id', IncidentController.delete);

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/incidents/id`  e nesse caso deve passar o id do caso (1, 2, 3...) utilizando o método DELETE retornando uma resposta de que deu certo ou não foi autorizado.

**Login ONG**

Agora criaremos uma função para a ONG acessar a aplicação utilizando o ID gerado para ela. 

Para isso criaremos um novo controller `SessionController.js`. 

    // importamos o arquivo de conexão com o banco de dados.
    const connection = require('../database/connection');
    
    module.exports = {
    	// função para logar 
      async create(request, response) {
    	  // usando destruturação pega o id que será enviado pelo corpo da requisição.
    		const { id } = request.body;
    		
    		// consulta para achar a ong no banco de dados.
        const ong = await connection('ongs')
          .where('id', id)
          .select('name')
          .first();
    		
    		// verifica se a ong está cadastrada no banco de dados.
        if(!ong) {
    			// se a ong não for encontrada então retorna um status code 400
    			// com a mensagem de erro de que não foi possível encontrar a ong por esse id.
          return response.status(400).json({ error: 'No ONG found with this ID'});
        }
    		
    		// se a ong estiver cadastrada e com o id correto então é retornado o nome da ong.
        return response.json(ong);
      }
    }

No arquivo `routes.js` adicionamos o seguinte trecho.

    routes.post('/sessions', SessionController.create);

Para os teste enviamos uma requisição no endereço: `http://localhost:3333/sessions`  e enviar no corpo um JSON com o id da ong utlizando o método POST.

**Adicionando o cors**

Para que o nosso backend sejá acessível para outras aplicações, como o nosso frontend é necessário adicionar o cors para que nossa api possa ser utilizada.

Para instalar o cors, no terminal, digite o comando:

`npm install cors`

Depois no arquivo `index.js`

importe o cors e configure-o para a aplicação.

    const express = require('express');
    // importando o cors.
    const cors = require('cors');
    const routes = require('./routes');
    
    const app = express();
    
    // configurando o cors para que seja acessível por qualquer aplicação.
    app.use(cors());
    
    app.use(express.json());
    app.use(routes);
    
    app.listen(3333);

Quando a aplicação for pra produção devemos configurar o endereço para o qual a aplicação terá autorização para utilizar. 

    const express = require('express');
    const cors = require('cors');
    const routes = require('./routes');
    
    const app = express();
    
    // aqui passamos um objeto para o cors indicando qual aplicação está autorizada a utiliza-la.
    app.use(cors({
      origin: 'http://meuapp.com'
    }));
    
    app.use(express.json());
    app.use(routes);
    
    app.listen(3333);