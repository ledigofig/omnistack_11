# Dia 4 - Desenvolvendo o app mobile

Instalação do Expo para que seja utilizado em qualquer aplicação mobile. 

`npm install -g expo-cli`

o parêmetro -g significa que será um pacote instalado de maneira global, ou seja, será acessível em qualquer projeto.

Digite o comando:

`expo -h`

Se apareceu informações sobre o expo então ele foi instalado corretamente.

**Criando o projeto mobile**

Para criar um projeto

`expo init mobile`

Selecione o template: `blank`

Para executar no celular:

`npm start`

Irá abrir uma página no navegador e lá irá aparecer um QR Code. 

Com o aplicativo instalado no celular é só fazer a leitura desse QR Code.

Estrutura de pastas de uma projeto React Native

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b0aeb8c-587d-402d-b97b-bb1c961054e6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b0aeb8c-587d-402d-b97b-bb1c961054e6/Untitled.png)

Arquivo App.js 

    import React from 'react';
    import { StyleSheet, Text, View } from 'react-native';
    
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello Universe!</Text>
        </View>
      );
    }
    
    const styles = StyleSheet.create({
      container: {
        flex: 1, // ao adicionar outra propriedade css adicionarmos uma vírgula.
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center',
      },
    });

É uma sintaxe bem parecida com o nosso projeto frontend feito em ReactJS. 

Porém há algumas diferença, em um projeto React Native não temos as tags do HTML.

Mas temos componentes que podem ser usados como um substituto para as tags. Eles não tem semânticas.

Como no código acima: 

    import React from 'react';
    import { StyleSheet, Text, View } from 'react-native';
    
    export default function App() {
      return (
    		// o componente View pode ser utilizado como uma div.
    		// para estilizar utilizamos a propriedade style passando um objeto com as estilizações.
        <View style={styles.container}>
    			// o componente Text é utilizado para colocar texto, os nomes são bem intuitivos.
          <Text>Hello Universe!</Text>
        </View>
      );
    }
    
    // a estilização são todas feita dessa forma, utilizando somente javascript.
    const styles = StyleSheet.create({
    	// todos os elementos do React Native já tem display flex por padrão.
      container: {
        flex: 1, // para utilizar a página inteira.
        backgroundColor: '#fff', // as propriedades de estilização ao invez de hífen colocamos a próxima letra maiúscula.
        alignItems: 'center',
        justifyContent: 'center',
      },
    });

No react native não existe herança de estilização, ou seja, caso queira estilizar um componente que está dentro de outro componente é necessário criar uma estilização própria para ele. 

    import React from 'react';
    import { StyleSheet, Text, View } from 'react-native';
    
    export default function App() {
      return (
        <View style={styles.container}>
    			// para alterarmos a cor do texto devemos criar um estilo próprio
          <Text style={styles.title}>Hello Universe!</Text>
        </View>
      );
    }
    
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        backgroundColor: '#7159c1',
        alignItems: 'center',
        justifyContent: 'center',
      },
    	// separados por vírgula podemos criar uma nova estilização aqui.
    	title: {
    		color: '#fff',
    	}, // lembre sempre de adicionar vírgula quando for criar uma nova estilização.
    });

**Criação básica de um componente em React Native**

Exemplo da criação do componente de detalhes do nosso app mobile.

    import React from 'react';
    import { View } from 'react-native';
    
    export default function Detail() {
      return (
        <View />
      )
    }

**Rotas**

Para utilizarmos rotas em um projeto expo React Native devemos utilizar o React Navigation 

`npm install @react-navigation/native`

Como estamos utilizando o expo então devemos executar esse comando em nosso projeto:

`expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view`

Se não estivermos utilizando expo:

`npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view`

Como iremos utilizar somente botões como navegação então instalmos o seguinte pacote:

`npm install @react-navigation/stack`

Para resolver o problema a tela não ficar na status bar 

`expo install expo-contants`

Pacote para fazer escrita de e-mail, abrir o e-mail: 

`expo install expo-mail-composer`

Cliente http para nos comunicar com nossa api

`npm install axios`

Pacote que adicionar a formatação do valor em reais, quando não presente no sistema operacional por padrão que no caso só o iOS que tem.

`npm install intl`