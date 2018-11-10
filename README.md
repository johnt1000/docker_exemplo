# APP Carros

## Descrição
Projeto para a disciplina Tópicos Avançados em Engenharia de Software III, ministrada no programa de mestrado profissional, turma 2018, pelo Professor Dr. Silvio Costa Sampaio.

## Atividade
Suponha que você seja um desenvolvedor Web e necessita de uma estrutura ágil e dinâmica para testar seus programas, sem ter a necessidade de configurar seu ambiente toda vez que desenvolve uma aplicação:
1. Apresente uma solução utilizando containers Docker
2. Implemente um site simples (ou utilize algum template pronto) e mostre que sua infra virtualizada funciona
3. Apresente um relatório com a descrição e configuração do ambiente; e os testes realizados.

## Relatório

### Hardware
- MacBook Air (11-inch, Mid 2013)
- Processador 1,3 GHz Intel Core i5
- Mémoria 4 GB 1600 MHz DDR3
- Gráficos Intel HD Graphics 5000 1536 MB

### Pré-requisito
- macOS Mojave - Versão 10.14.1 (18B75)
- [Homebrew](https://brew.sh/index_pt-br)

### Agenda
1. Instalação do [Docker](https://store.docker.com/editions/community/docker-ce-desktop-mac);
1. Instalação do [Node.Js](https://nodejs.org/en/);
1. Instalação do [Framework VueJs](https://vuejs.org/);
1. Instalação do [Framework Loopback](https://loopback.io/);
1. Criando aplicativo e configurando container para uso do [VueJs](https://vuejs.org/);
1. Criando API e configurando container para uso do [Loopback](https://loopback.io/);
1. Configuração [docker-compose](https://docs.docker.com/compose/) para "orquestração" de containers;
1. Teste de comunicação entre as aplicações.

### Instalação do Docker
Para a instalação simplificada do Docker no macOS, existem duas formas:
- Usando [Homebrew](https://brew.sh/index_pt-br). (Recomendado)
- Baixando manualmente o pacote [Docker](https://store.docker.com/editions/community/docker-ce-desktop-mac) para macOs.

Instale usando [Homebrew](https://brew.sh/index_pt-br) com o seguinte comando:
``` sh
brew cask install docker
```
Após o processo de instalação um novo ícone fica disponível na pasta de aplicativos do macOS:

![icone docker](https://uploaddeimagens.com.br/images/001/714/744/original/icone_docker.png?1541687470)

Ao iniciar o aplicativo um novo ícone será criado na Toolbar do macOS:

![icone docker](https://image.ibb.co/dg15DV/toolbar.png)

Assim finalizamos a instalação do [Docker](https://store.docker.com/editions/community/docker-ce-desktop-mac) no macOS.

### Instalação do Node.js
Para a instalação do Node.js no macOS não é muito diferente do Docker:
- Usando [Homebrew](https://brew.sh/index_pt-br). (Recomendado)
- Baixando manualmente o pacote [Node.js](https://nodejs.org/en/) para macOs.

Instale usando [Homebrew](https://brew.sh/index_pt-br) com o seguinte comando:
``` sh
brew install nodejs
```

Use o comando:
``` sh
node -v
```

O terminal deve retorna a versão do Node.js:

![Terminal Node.js](https://image.ibb.co/mOJoHq/node-install.png)

Assim finalizamos a instalação do [Node.js](https://nodejs.org/en/) no macOS.

### Instalação do Framework VueJs
Ao instalar o [Node.js](https://nodejs.org/en/) o gerenciador de pacotes [NPM](https://www.npmjs.com/) está disponível para uso. Use o seguinte comando para instalar o CLI da [Framework VueJs](https://vuejs.org/):

``` sh
npm install -g @vue/cli
npm install -g @vue/cli-service-global
```

Use o comando:
``` sh
vue --version
```

![Terminal VueJs](https://image.ibb.co/kfahAA/vue-install.png)

[Framework VueJs](https://vuejs.org/) instalado.

### Instalação do Framework Loopback
Use o [NPM](https://www.npmjs.com/) para instalação do [Framework Loopback](https://loopback.io/). Use o seguinte comando para instalar o CLI:

``` sh
npm install -g loopback-cli
```

Use o comando:
``` sh
lb -version
```

![Terminal Loopback](https://image.ibb.co/nfpHAA/loopback-install.png)

[Framework Loopback](https://loopback.io/) instalado.

### Criando aplicativo SPA
Crie uma pasta para o seu projeto e use o CLI do VueJs para criar o aplicativo SPA com o seguinte comando:
``` sh
vue create spa
```

O comando iniciar um wizard, selecione a seguinte opção:
- default (babel, eslint)

Use os seguintes comandos para iniciar a aplicação:
``` sh
cd spa
npm run serve
```

Sua aplicação está disponível em [http://localhost:8080](http://localhost:8080).

![App VueJs](https://image.ibb.co/ibiK7q/vue-app.png)

#### Dockerfile (SPA)
O Dockerfile define o container e suas configurações personalizadas.
```Dockerfile
# spa/Dockerfile
FROM node:8-alpine
# Define diretório que serão executados os próximos comandos
WORKDIR /app
# Arquivos que define as dependências
COPY package.json .
# Instala dependências do aplicativo
RUN ["npm", "install"]
# Porta que será exposta
EXPOSE 8080
```
O Dockerfile automatiza os comandos para suportar o aplicativo SPA. O script Dockerfile será usado posteriormente pelo [docker-compose](https://docs.docker.com/compose/).

### Criando API
Entre na pasta raiz do seu projeto e use o CLI do Loopback para criar a API com o seguinte comando:
``` sh
lb
```

O comando iniciar um wizard, selecione as seguintes opções:
- ? Qual o nome do seu aplicativo? carros
- ? Insira o nome do diretório para conter o projeto: api
- ? Qual versão de LoopBack você gostaria de usar? 3.x (Active Long Term Support)
- ? Que tipo de aplicativo você tem em mente? api-server (Um servidor de API LoopBack com autenticação do usuário local)

Use os seguintes comandos para criar um model de Carro:
``` sh
cd api
lb model
```

O comando iniciar um wizard, siga os passos abaixo:
- ? Insira o nome do modelo: Carro
- ? Selecione a origem de dados para a qual conectar o Carro: db(memory)
- ? Selecione a classe base do modelo PersistedModel
- ? Expor Carro por meio da API REST? Yes
- ? Formulário plural customizado (usado para construir REST URL): Carros
- ? Modelo comum ou apenas servidor? comum

Vamos incluir algumas propriedades Carro agora.
- ? Nome da Propriedade: nome
- ? Tipo de propriedade: string
- ? Necessário? Yes
- ? Valor padrão [deixe em branco para nenhum]: 

Vamos incluir outra propriedade Carro.
- ? Nome da Propriedade: ano
- ? Tipo de propriedade: number
- ? Necessário? Yes
- ? Valor padrão [deixe em branco para nenhum]: 

Vamos incluir outra propriedade Carro.
- ? Nome da Propriedade: preco
- ? Tipo de propriedade: number
- ? Necessário? No
- ? Valor padrão [deixe em branco para nenhum]: 0

Use o comandos a seguir para iniciar a API:
``` sh
node .
```

Sua aplicação está disponível em [http://localhost:3000/explorer](http://localhost:3000/explorer).

![API Loopback](https://image.ibb.co/mrbgnq/loopback-api.png)

#### Dockerfile (API)
O Dockerfile define o container e suas configurações personalizadas.
```Dockerfile
# api/Dockerfile
FROM node:8-alpine
# Define diretório que serão executados os próximos comandos
WORKDIR /app
# Arquivos que define as dependências
COPY package.json .
# Instala dependências do aplicativo
RUN ["npm", "install"]
# Porta que será exposta
EXPOSE 3000
```
O Dockerfile automatiza os comandos para suportar a API. O script Dockerfile será usado posteriormente pelo [docker-compose](https://docs.docker.com/compose/).

### Configurando docker-compose
Crie um arquivo na raiz do projeto com o nome docker-compose.yaml com o seguinte conteúdo:
```yaml
# docker-compose.yaml 
version: '3'

services:
  
  spa:
    image: johnt1000/vue:1.0 # imagem (build)
    ports: # portas que serão liberadas
      - 8080:8080
    build: # aponta Dockerfile
      context: ./spa
    command: npm run serve # comando apra iniciar SPA
    env_file: # arquivo com variáveis globais
      - .env
    volumes: # volumes que serão "espelhados"
      - ./spa:/app
      - /app/node_modules
    depends_on: # dependência de outros containers
      - "api"
  
  api:
    image: johnt1000/loopback:1.0 # imagem (build)
    ports: # portas que serão liberadas
      - 3000:3000
    build: # aponta Dockerfile
      context: ./api
    command: npm start # comando apra iniciar API
    env_file: # arquivo com variáveis globais
      - .env
    volumes: # volumes que serão "espelhados" no container
      - ./api:/app
      - /app/node_modules
```

Execute o comando:
``` sh
docker-compose up --build
```

Acesso as aplicações:
- Sua aplicação SPA está disponível em [http://localhost:8080](http://localhost:8080).
- Sua aplicação API está disponível em [http://localhost:3000/explorer](http://localhost:3000/explorer).


### Teste de comunicação
Acesse a API no endereço [http://localhost:3000/explorer/#!/Carro/Carro_create](http://localhost:3000/explorer/#!/Carro/Carro_create) e crie um novo carro.

![Novo carro](https://image.ibb.co/mQUkiV/new-carro.png)

Acesse a SPA no endereço [http://localhost:8080/](http://localhost:8080/) e veja seu novo carro na lista.

![Lista de carro](https://image.ibb.co/dVgbqA/spa-carros.png)
