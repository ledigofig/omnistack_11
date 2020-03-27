# Dia 1 - Conhecendo a Semana Omnistack 11 evento online da Rocketseat

23 a 29 de Março. 

Ao longo da semana será desenvolvido uma aplicação fullstack com as stacks Node.js, ReactJS e React Native. 

A aplicação é **The Be Hero.**

- ONGs sem fins lucrátivos podem cadastrar seu casos, por exemplo, uma ONG que resgata animais cadastra o caso para que as pessoas que entrem na aplicação possa entrar em contato para ajudar com uma doação financeira.

## **Instalação do ambiente de desenvolvimento.**

- Instale o gerenciador de pacote Chocolatey ([https://chocolatey.org/docs/installation](https://chocolatey.org/docs/installation))
- Execute o comando no terminal para instalar o Visual Studio Code e o Node.js: `choco install node-lts vscode`

Arquitetura da aplicação 

## **Backend**

O Backend de uma aplicação tem a responsabilidade de:

- Regra de negócio;
- Conexão banco de dados;
- Envio de e-mail;
- Comunicação com webservices;
- Autenticação do usuário;
- Criptografia e segurança;

O backend é o coração da aplicação que retorna JSON (JavaScript Object Notation) dados que são enviados para servir para outras aplicações, como o frontend e outras aplicações. Uma API restiful que seguirar algumas regras.

Criando um projeto de exemplo com Node.js:

`npm init -y`

Automaticamente cria um arquivos package.json onde é armazenados as depências da aplicação, também é gerado em aplicações ReactJS e React Native. 

Instalando um framework para dar inicio ao projeto de exemplo. 

`npm install express`

Crie um arquivo `index.js` no diretório do projeto. 

Para iniciar uma aplicação.

    const express = require('express');
    
    // iniciando a aplicação.
    
    const app = express();
    
    // criando uma rota.
    
    app.get('/', (request, response) ⇒ {
    	return response.json({
    		evento: 'Semana Omnistack',
    		aluno: 'Emerson'
    	});
    });
    
    // definir uma porta para aplivação "ouvir".
    app.listen(3333); // porta da aplicação

Para executar a aplicações de exemplo abra o terminal no diretório do projeto e digite o comando: `node index.js` 

Para testar a aplicação vamos no navegador e digite `localhost:3333`

## **Entendendo o React**

**Abordagem tradicional**

Na abordagem tradicional, a cada requisição, o servidor retorna o conteúdo completo da página, contento todo HTML e CSS. 

Essa abordagem limita o front-end para o browser já que o aplicativo mobile ou serviços externos não vão conseguir interpretar o HTML. 

**Abordagem SPA (Single-Page Applications)**

Na abordagem de SPA, as requisições trazem apenas dados como respostas e, com esses dados, o front-end pode preencher as informações em tela.

A página nunca recarrega, otimizando a performance e dando vida ao conceito de SPA. Retornando apenas JSON podemos ter quantos front-end quisermos.

Na renderização da página o React garante fazer isso de maneira mais rápida criando cada elemento da página. 

## **Projeto de Hello World com ReactJS**

npx é um compando do npm para executar pacotes sem a necessidade de instalação. 

Comando para criar um novo projeto ReactJS:

`npx create-react-app nomeDoProjeto`

Para executar um projeto ReactJS digite o comando: 

`npm start`

No arquivo package.json em um projeto React vem configurado um script de execução para o projeto ReactJS. 

## **Entendendo o React Native**

Abordagem Tradicional 

Na abordagem tradicional, criamos uma aplicação para iOS e outra para Android, e nesses casos, o trabalho se torna repetido tanto para criação quanto para as alterações no projeto. 

Abordagem do React Native

Todo código feito é em JavaScript, esse código não é convertido em código nativo, o dispositivo passa a entender o código JavaScript e a interface gerada é totalmente nativa. 

Isso nos traz um ganho de desempenho e produtividade. 

## **Por que utilizar o Expo?**

Sem o Expo, precisamos instalar em nosso sistema tanto o Android Studio para obter a SDK de desenvolvimento Android, e o Xcode (apenas Mac) para obter a SDK do iOS. 

Nesse caso, nossa iniciação no desenvolvimento fica mais penosa, já que essas SDK's não são extremamente simples de instalar e livres de erros. 

**Arquitetura do Expo**

Com o Expo, nós instalamos um aplicativo no celular chamdo Expo, e dentro dele, tudo o que precisamos para desenvolver no React Native já está instalado, como as API's de mapas, geolocalização, câmera, sensores, calendário, etc... (Obs.: Expo não possui acesso nativo ao Bluetooth).

Com isso, não precisamos nos preocupar em gerar o aplicativo pra Android e iOS já que o app do Expo instalado tem tudo o que precisamos e assim usamos apenas React.