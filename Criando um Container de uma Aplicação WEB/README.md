# Formação Docker Fundamentals

Nesta Formação você irá aprender a criar, testar e implementar contêineres Docker com atividades práticas e reais. Você aprenderá Docker começando por entender o que é um contêiner e como é o seu funcionamento. Irá realizar atividades práticas em laboratórios virtuais para entender como os contêineres são criados e implementados em uma infraestrutura real. Depois de entender o que é um contêiner, você aprenderá como trabalhar com eles usando comandos básicos do CLI do Docker.

## Desafio de projeto: Criando um Container de uma Aplicação WEB

**Descrição**

Neste projeto o expert utilizou o Docker Compose para executar uma aplicação HTML em um Container Apache. Você poderá ir além e fazer alterações mais robustas ao seu projeto, estilizando sua página e utilizando seus conhecimentos em (HTML, CSS e JS). Você também pode buscar outras formas para executar seu arquivo HTML em outras Linguagens de Programação.

1. Criar um arquivo YML com as definições de um servidor Apache (httpd);
2. Especificar no arquivo YML o local onde os arquivos da aplicação estarão. A aplicação pode ser um simples Hello World. Será que você consegue executar uma aplicação web completa?
3. Subir o arquivo YML e a aplicação para um repositório no GitHub.


### Solução

Utilizei **multi-stage-build** do node com o Apache (ver [Dockerfile](./Dockerfile)).

O docker-compose inicial

```yml
version: '3.8'
services:
  apache:
    image: httpd:latest
    container_name: my-apache-app
    ports:
    - "3000:80"
    volumes:
    - ./website/:/usr/local/apache2/htdocs
```

precisa do conteúdo da pasta `website`, criada com o comando `npm run build` (ver [package.json](./package.json)),  para ser utilizado no volume compartilhavél. Com a utilização do Dockerfile com `mult-stage-build`, instalamos as depedências e criamos os arquivos para produção numa primeira etapa chamada ``build` que utiliza uma imagem docker Node. Na segunda etapa, utilizamos uma imagem do Apache e puxamos somente os arquivos necessários da primeira etapa de build. Dessa, o docker-compose fica:

```yml
version: '3.8'
services:
  apache:
    build: ./
    container_name: my-apache-app
    ports:
      - "3000:80"
```

Após utilizar `docker compose up` na raíz do projeto, você pode ver o resultado em http://localhost:3000/