# Dia 5 - Funcionalidades Avançadas

Biblioteca pra fazer validações 

`npm instal celebrate`

A validação fica nas rotas.

Nas rotas de criação e e listagem. 

antes devemos adicionar em nosso arquivo i

    // importa o celebrate
    const { errors } = require('celebrate');
    
    // depois das rotas adicione essa linha
    app.use(errors());

Utilizando o Celebrate para fazer validações:

    const express = require('express');
    // importamos somente o que vamos utilizar (usand destruturing)
    const { celebrate, Segments, Joi } = require('celebrate');
    
    const OngController = require('./controllers/OngController');
    const IncidentController = require('./controllers/IncidentController');
    const ProfileController = require('./controllers/ProfileController');
    const SessionController = require('./controllers/SessionController');
    
    
    const routes = express.Router();
    
    routes.post('/sessions', SessionController.create);
    
    routes.get('/ongs', OngController.index);
    // e aqui fazemos a validação dos dados que são enviados. Corpo da requisição
    routes.post('/ongs', celebrate({
      [Segments.BODY]: Joi.object().keys({
        name: Joi.string().required(),
        email: Joi.string().required().email(),
        whatsapp: Joi.number().required().min(10).max(11),
        city: Joi.string().required(),
        uf: Joi.string().required().length(2),
      })
    }) , OngController.create);
    // validação de header
    routes.get('/profile', celebrate({
      [Segments.HEADERS]: Joi.object({
        authorization: Joi.string().required(),
      }).unknown()
    }), ProfileController.index);
    // validação da query string enviada por rota.
    routes.get('/incidents', celebrate({
      [Segments.QUERY]: Joi.object().keys({
        page: Joi.number(),
      })
    }), IncidentController.index);
    
    routes.post('/incidents', IncidentController.create);
    // validação de parâmetros por rota.
    routes.delete('/incidents/:id', celebrate({
      [Segments.PARAMS]: Joi.object().keys({
        id: Joi.number().required(),
      })
    }), IncidentController.delete);
    
    module.exports = routes;

**Testes**

Os testes são recomendados para grandes aplicações. Precisamos garantir que tudo continue funcionando bem, independente dos novos códigos que vai adicionando no projeto.

TDD - Test-driven Development

Temos algumas abordagem sobre:

Criar os métodos antes de depois fazer os testes.

E a arbordagem de fazer os testes antes e depois códificar. Nesse caso aqui funciona como se fosse um guia. (**Ler mais sobre**)

**Jest - Framework de teste para o Node.js, ReactJS, React Native, etc.**

`npm install jest`

Depois execute:

`npx jest --init`

Para executar o teste:

`npm test`

Instalação do cross-env para ambiente de testes.

`npm install cross-env`

Depois de intalado o cross-env

configure o script de teste no arquivo `package.json`

    "test": "cross-env NODE_ENV=test jest"

Para fazer requisições http para teste 

`npm install supertest -D`

Testes de integração irá testar uma rota inteira. Testa por completo uma funcionalidade.

Teste unitário testa um pequeno trecho, de mais mais isolado. 

Antes dos testes devemos configurar um banco de dados de teste, assim também o nosso servidor.

Exemplo de teste unitário.

    const generateUniqueId = require('../../src/utils/generateUniqueId');
    
    describe('Generate Unique ID', () => {
      it('should generate an unique ID', () => {
        const id = generateUniqueId(); // gera um id 
        expect(id).toHaveLength(8); // verifica se o id tem o tamanho esperado.
      })
    })

Exemplo de teste de integração 

    const request = require('supertest');
    const app = require('../../src/app');
    const connection = require('../../src/database/connection');
    
    describe('ONG', () => {
    	// antes do teste essa função é executada.
      beforeEach(async () => {
    		// desfaz a última modificação feita no banco de dados de testes.
        await connection.migrate.rollback();
    		// executa a migration para criação da tabela
        await connection.migrate.latest();
      });
    	// depois de tudo, o teste finalizado, destrói a conexão com o banco de dados.
      afterAll( async () => {
        await connection.destroy();
      });
    
      it('should be able to create a new ONG', async () => {
        const response = await request(app)
          .post('/ongs')//.set('authorization', 'idvalue') //- com header.
          .send({
            name : "ONG 2",
            email : "pata.carente@email.com",
            whatsapp : "68981050233",
            city : "Rio Branco",
            uf : "AC"
          });
    		// o teste verifica que a resposta no corpo veio um id.
        expect(response.body).toHaveProperty('id');
    		// teste pra verificar o tamanho do id.
        expect(response.body.id).toHaveLength(8);
      });
    });