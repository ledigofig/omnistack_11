## Dia 3 - Construindo a interface web

Estrutura de pastas e arquivos de um projeto ReactJS, com arquivos que não seram necessários já excluídos. 

![](https://i.imgur.com/kZCl1eH.png)

**Como o ReactJS funciona**

O primeiro arquivo a ser executado em um projeto reactjs é o `public/index.html` 

```
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <meta name="theme-color" content="#000000" />
        
        <title>Be The Hero</title>
      </head>
      <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <div id="root"></div>
      </body>
    </html>
```

Depois que esse arquivo é carregado, automaticamente o ReactJS faz a montagem dos elementos dentro da `div` com `id` root. 

O arquivo `src/index.js` é o primeiro arquivo JavaScript a ser executado.

```
    // importando o react
    import React from 'react';
    // importanto o react-dom responsável pela arvoré de elementos.
    import ReactDOM from 'react-dom';
    // importando o primeiro componente JSX. 
    import App from './App';
    // e aqui o ReactDOM renderizar o componente colocando-o dentro daquela div com id root.
    ReactDOM.render(<App />, document.getElementById('root'));
```

O componente App: 

```
    import React from 'react';
    
    function App() {
      return (
        <h1>Hello Universe!</h1>
      );
    }
    
    export default App;
```

Um componente em ReactJS é uma função que retorna HTML, porém quando o HTML é escrito dentro de um arquivo JavaScript ele é chamado de JSX (Javascript XML). 

As **propriedades** em React são como atributos em elementos html, só que em vez disso são atributos de componentes. 

Crie o arquivo `Header.js`, todo componente deve ser escrito com a primeira letra maiúscula.

```
    // todo componente que for utilizar jsx deve importar o react.
    import React from 'react';
    
    // exemplo de criação de um novo componente Header
    // export default já deixa o componente pronto pra ser exportado.
    export default function Header() {
    		// quando retornar mais de uma linha de elementos HTML o conteúdo retornado deve
    		// estar entra parenteses.
        return (
    				// elemento header do HTML, diferente do Header o componente criado.
            <header>
                <h1>Be The Hero</h1>
            </header>
        );
    }
```

Para importar o componente `Header.js` no componente principal `App.js` fazemos da seguinte forma.

```
    import React from 'react';
    // importamos o Header.
    import Header from './Header';
    
    function App() {
      return (
    		// e utilizamos ele aqui. Como esse componente não terá conteúdo podemos fecha-lo assim mesmo.
    		// se ele tivesse conteúdo ele deveria envolver o conteúdo. Ex.: <Header>Conteúdo.</Header>.
    		// Aqui estamos enviado uma propriedade definido o título que queremos.
        <Header title="Be The Hero" />
      );
    }
    
    export default App;
```

E para acessar esse atributo ele é recebido no componente como um parâmetro.

```
    import React from 'react';
    // recebemos as propriedades por parâmetros as props.
    export default function Header(props) {
        return (
            <header>
    						// para utilizamos qualquer contéudo javascript dentro do HTML utilizamos
    						// as chaves. Caso contrário será mostrado somente como texto.
                <h1>{props.title}</h1>
            </header>
        );
    }
```

Também podemos usar uma propriedade que tem por padrão no reactjs que pega o conteúdo que estiver no componente. Por exemplo ao alterar apenas o trecho de código: 

```
    <Header>
    	Be The Hero
    </Header>
```

Para acessarmos essa informaçãos tambem utilizamos `props` só que usando o `children`.

```
    import React from 'react';
    
    export default function Header(props) {
        return (
            <header>
    						// utilizamos o children para pegar o conteúdo do componente 
                <h1>{props.children}</h1>
            </header>
        );
    }
```

É bastante comum utilizar a desestruturação de propriedades: 

```
    import React from 'react';
    // utilizando destructuring para pegar somente a propriedade que queremos.
    export default function Header({children}) {
        return (
            <header>
    						
                <h1>{children}</h1>
            </header>
        );
    }
```

**Estado**

Estado é uma informação que vai ser mantida pelo componente.

Tentar alterar um valor de uma variável de forma não adequada em ReactJS não sofre efeito na interface, ou seja, não podemos alterar um variável diretamente por questões de performance do ReactJS. O ReactJS tem tem um função (`useState`) que auxilia nisso e faz com que a alteração do estado e mantem a **imutabilidade** do estado. 

Uso do `useState`.
```

    // importa o useState
    import React, { useState } from 'react';
    
    import Header from './Header';
    
    function App() {
    	// estado do componente.
      // retorna um Array [valor, funcaoDeAtualizacao]
      const [counter, setCounter] = useState(0); // valor inicial 0 
    
      function increment() {
    		// só é possível alterar um estado utilizando a função de atualização do estado.
        setCounter(counter + 1);
      }
      
      return (
        <div>
          <Header>Contador: {counter}</Header>
          <button onClick={increment}>Incrementar</button>
        </div>
      );
    }
    
    export default App;
```

**Criação da página de login**

Instalação do pacote de icones para o ReactJS 

`npm install react-icons`

Esses pacotes de icones podem ser utilizado na aplicação como um componente.

O pacote react-icons vem com todos os pacotes de icones mais populares.

Aqui estamos utilizando o pacote Feather.

Ex.: `import { FiArrowLeft } from 'react-icons/fi'` 

Fonte utilizada Roboto: [https://fonts.google.com/?selection.family=Roboto:400,500,700](https://fonts.google.com/?selection.family=Roboto:400,500,700)

Para que o nosso projeto frontend se conecte com a nossa api é necessário instalar um pacote http.

`npm install axios`

Criamos um arquivo de configuração 
```
    import axios from 'axios';
    
    // definindo o endereço base da nossa api.
    const api = axios.create({
      baseURL: 'http://localhost:3333',
    });
    
    export default api;
```
Rotas em um projeto ReactJS

```
    import React from 'react';
    // para trabalharmos com rotas devemos utilizar esse pacote.
    // npm install react-router-dom
    import { BrowserRouter, Route, Switch } from 'react-router-dom';
    
    import Logon from './pages/Logon';
    import Register from './pages/Register';
    import Profile from './pages/Profile';
    import NewIncident from './pages/NewIncident';
    
    export default function Routes() {
        return (
            <BrowserRouter>
                <Switch>
    								// a propriedade exact é utilizada para executar extamente essa rota
    								// caso contrário irá sempre executar essa rota já que todas começam com / (barra)
                    <Route path="/" exact component={Logon} />
                    <Route path="/register" component={Register} />
    
                    <Route path="/profile" component={Profile} />
                    <Route path="/incidents/new" component={NewIncident} />
                    
                </Switch>
            </BrowserRouter>
        )
    }
```

**Cadastro de ong**

```
    // já que vamos trabalhar com estado, além do react importamos o useState.
    import React, { useState } from 'react';
    // pra trabalhar com links importamos Link para a navegação entre rotas o useHistory.
    import { Link, useHistory } from 'react-router-dom';
    import { FiArrowLeft } from 'react-icons/fi';
    
    // importando a api.
    import api from '../../services/api';
    
    import './styles.css';
    
    import logoImg from '../../assets/logo.svg';
    
    export default function Register() {
    	// definindo os estados iniciais e funções de alteração.
      const [name, setName] = useState('');
      const [email, setEmail] = useState('');
      const [whatsapp, setWhatsapp] = useState('');
      const [city, setCity] = useState('');
      const [uf, setUf] = useState('');
    
      const history = useHistory();
    	
    	// recebemos o event padrão do formulário.
      async function handlerRegister(e) {
        // evitando o evento padrão do formulário.
    		e.preventDefault();
    		
    		// dados que foram definidos utilizando useState com a função de alteração de cada estado..
        const data = {
          name,
          email,
          whatsapp,
          city,
          uf
        };
    		
        try {
    			// fazendo o requisição para a api via post com os dados da ong.
    			// recebendo a resposta da requisição.
          const response = await api.post('ongs', data);
    			// alerta com o ID da ong.
          alert(`Seu ID de acesso: ${response.data.id}`);
    			// redirecionado para a pagina inicial (login).
          history.push('/');
        } catch(err) {
          alert(`Erro no cadastro tente novamente.`)
        }
    
      }
    
      return (
        <div className="register-container">
          <div className="content">
            <section>
              <img src={logoImg} alt="Be The Hero"/>
    
              <h1>Cadastro</h1>
              <p>
                  Faça seu cadastro, entre na plataforma e ajude pessoas a encontrarem os casos da sua ONG.
              </p>
    
              <Link to="/" className="back-link">
                <FiArrowLeft size={16} color="#E02041" />
                Não tenho cadastro
              </Link>
            </section>
    				// quando o clicar no botão de submit então a função handlerRegister será executada.
            <form onSubmit={handlerRegister}> 
              <input 
                type="text" 
                placeholder="Nome da ONG" 
                value={name}
    						// quando este campo for alterado então a função de alteração de 
    						// name (setName) será executada. Assim como todos os campos desse formulário.
                onChange={e => setName(e.target.value)}
              />
    
              <input 
                type="email" 
                placeholder="E-mail" 
                value={email}
                onChange={e => setEmail(e.target.value)}
              />
    
              <input 
                type="text" 
                placeholder="Whatsapp" 
                value={whatsapp}
                onChange={e => setWhatsapp(e.target.value)}
              />
    
              <div className="input-group">
                <input 
                  type="text" 
                  placeholder="Cidade" 
                  value={city}
                  onChange={e => setCity(e.target.value)}
                />
    
                <input 
                  type="text" 
                  placeholder="UF" 
                  value={uf}
                  style={{ width: 80 }} 
                  onChange={e => setUf(e.target.value)}
                />
              </div>
    
              <button className="button">Cadastrar</button>
            </form>
          </div>
        </div>
      );
    }
```

Login

```
    // além do react importamos o useState para fazer alterações no estado do componente.
    import React, { useState } from 'react';
    // para trabalharmos com link importamos Link e com navegação entre rotas useHistory
    import { Link, useHistory } from 'react-router-dom';
    import { FiLogIn } from 'react-icons/fi';
    
    // sempre que precisarmos se comunicar com a api devemos importar-la.
    import api from '../../services/api';
    
    import './styles.css';
    
    import logoImg from '../../assets/logo.svg';
    import heroesImg from '../../assets/heroes.png';
    
    export default function Logon() {
    	// definindo o estado inicial do estado e a função de alteraçao de id.
      const [id, setId] = useState('');
    	// definindo history para navegação entre rotas.
      const history = useHistory();
    
    	// função responsável pelo login.
      async function handleLogin(e){
    		// em projeto react devemos sempre previnir a ação padrão de um formulário.
    		// evitando que recarregue a página inteira.
        e.preventDefault();
    		
        try {
    			// buscando na api o ip da ong. 
          const response = await api.post('sessions', {id});
    			// se a ong for encontrada então armazena os dados no localStorage.
          localStorage.setItem('ongId', id);
          localStorage.setItem('ongName', response.data.name);
    			// e depois redireciona para profile.
          history.push('/profile');
        } catch (err) {
    			// se der erro então esse alert é executado.
          alert('Falha no login, tente novamente');
        }
    
      }
    
      return (
        <div className="logon-container">
          <section className="form">
            <img src={logoImg} alt="Be The Hero"/>
    
            <form onSubmit={handleLogin}>
              <h1>Faça seu logon</h1>
    
              <input 
                type="text" 
                placeholder="Sua ID" 
                value={id}
                onChange={e => setId(e.target.value)}
              />
              <button className="button">Entrar</button>
    
              <Link to="/register" className="back-link">
                <FiLogIn size={16} color="#E02041" />
                Não tenho cadastro
              </Link>
            </form>
          </section>
    
          <img src={heroesImg} alt="Heroes"/>
        </div>
      );
    }
```

Cadastro de um caso

```
    import React, { useState } from 'react';
    import { Link, useHistory } from 'react-router-dom';
    import { FiArrowLeft } from 'react-icons/fi';
    
    // importamos a api.
    import api from '../../services/api';
    
    import './styles.css';
    
    import logoImg from '../../assets/logo.svg';
    
    export default function NewIncident() {
    	// definindo o estado inicial e a função de alteração.
      const [title, setTitle] = useState('');
      const [description, setDescription] = useState('');
      const [value, setValue] = useState('');
    	
      const history = useHistory();
    	// pegamos o id da ong que está logada.
      const ongId = localStorage.getItem('ongId');
    
      async function newIncident(e) {
        e.preventDefault();
    		
    		// dados do formulário.
        const data = {
          title,
          description,
          value,
        };
    
        try {
    			// faz uma requisição para a api enviando os dados e o header indicando a ong que está cadastrando.
          await api.post('incidents', data, {
            headers: {
              Authorization: ongId,
            }
          })
    			// quando cadastrado então é redirecionado para profile.
          history.push('/profile');
        } catch (err) {
          alert('Erro ao cadastrar caso, tente novamente.');
        }
      }
    
      return (
        <div className="new-incident-container">
          <div className="content">
            <section>
              <img src={logoImg} alt="Be The Hero"/>
    
              <h1>Cadastrar novo caso</h1>
              <p>
                  Descreva o caso detalhadamente para encontrar um herói para resolver isso.
              </p>
    					// exemplo de link para volta pra home.
              <Link to="/profile" className="back-link">
                <FiArrowLeft size={16} color="#E02041" />
                Voltar para home
              </Link>
            </section>
    				// formulário de  cadastro de caso.
            <form onSubmit={newIncident}>
              <input type="text" 
                placeholder="Título do caso" 
                value={title}
    						// quando este campo for alterado irá executar a função de alteração do estado title. 
                onChange={e => setTitle(e.target.value)}
              />
    
              <textarea 
                placeholder="Descrição" 
                value={description}
                onChange={e => setDescription(e.target.value)}
              />
    
              <input 
                placeholder="Valor em reais" 
                value={value}
                onChange={e => setValue(e.target.value)}
              />
    
              <button className="button">Cadastrar</button>
            </form>
          </div>
        </div>
      );
    }
```

**Listagem de Casos**
```
    import React, { useState, useEffect } from 'react';
    import { Link, useHistory } from 'react-router-dom';
    import { FiPower, FiTrash2 } from 'react-icons/fi';
    
    import api from '../../services/api';
    
    import './styles.css';
    
    import logoImg from '../../assets/logo.svg';
    
    export default function Profile() {
    	// definindo o estado inicial e função de alteração de incidents.
    	// como será um array de objetos, definimos como valor inical um array.
      const [incidents, setIncidents] = useState([]);
    
      const history = useHistory();
    
      const ongId = localStorage.getItem('ongId');
      const ongName = localStorage.getItem('ongName');
    	// useEffect passamos dois parâmetros, a função que será executada e o estado que
    	// ele irá "observar". 
    	// ele será executado quando o componente for montado.
      useEffect(() => {
        api.get('/profile', {
          headers: {
            Authorization: ongId,
          }
        }).then(response => {
          setIncidents(response.data);
        })
      }, [ongId]);
    	// função para deletar caso. Recebendo o id.
      async function handleDeleteIncident(id) {
        try {
    			// fazemos a requisição de delete para nossa api. Enviando também o header
    			// contendo a informação de cabeçalho que deve ser enviado junto com a requisição.
          await api.delete(`incidents/${id}`, {
            headers: {
              Authorization: ongId,
            }
          });
    			// depois de deletar, devemos atualizar o array de casos.
          setIncidents(incidents.filter(incident => incident.id !== id));
        } catch (err) {
          alert('Erro ao deletar caso, tente novamente.');
        }
      }
    	
    	// função para deslogar
      function handleLogout() {
    		// limpa todo o localStore.
        localStorage.clear(); 
    		// redireciona para a página de login.
        history.push('/');
      }
    
      return (
        <div className="profile-container">
          <header>
            <img src={logoImg} alt="Be The Hero"/>
            <span>Bem vinda, {ongName}</span>
            
            <Link className="button" to="/incidents/new">Cadastrar novo caso</Link>
            <button onClick={handleLogout} type="button">
              <FiPower size={18} color="#E02041" />
            </button>
          </header>
    
          <h1>Cados cadastrados</h1>
    
          <ul>
    				// utilizando o map para listar todos os casos cadastrados da ong.
            {incidents.map(incident => (
    					// em toda listagem no reactjs devemos informar no primeiro elemento a propriedade key
    					// e deve ser passado um valor único, nesse caso o id dos casos.
              <li key={incident.id}>
                <strong>CASO:</strong>
                <p>{incident.title}</p>
    
                <strong>DESCRIÇÃO:</strong>
                <p>{incident.description}</p>
    
                <strong>VALOR:</strong>
    						// formatando o valor para o formato moeda brasileira.
                <p>{Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL'}).format(incident.value)}</p>
    						// para executar essa função de delete devemos fazer isso usando
    						// uma função anonima, caso contrario iria dar erro ao tentar associar a função ao onClick.
                <button onClick={() => handleDeleteIncident(incident.id)} type="button">
                  <FiTrash2 size={20} color="#a8a8b3" />
                </button>
            </li>
            ))}
          </ul>
        </div>
      );
    }
```