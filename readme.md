# ErrorEagle

Repositório consolidado com as entregas do projeto ErrorEagle. O workspace contém três bases principais:

- `ErrorEagle-Jar/` - cliente desktop em Java Swing para autenticação do operador, validação do totem e captura de métricas da máquina.
- `ErrorEagle-Web/` - plataforma web/API mais completa, com dashboard, gestão de empresa, totems, alertas e relatórios.
- `Projeto-ErrorEagle/web-data-viz/` - versão anterior/alternativa da aplicação web, focada em login, cadastro, avisos e visualização de medidas.

## Visão Geral

A solução ErrorEagle foi criada para monitoramento de máquinas/totens e gestão operacional de alertas.

Em alto nível, o ecossistema faz o seguinte:

- identifica o equipamento pelo hostname;
- associa o totem à empresa;
- valida configuração de CPU, RAM, disco e rede;
- coleta métricas periodicamente usando a biblioteca Looca;
- grava os dados em banco local MySQL e/ou banco remoto SQL Server/Azure;
- exibe dashboards e relatórios no frontend web;
- permite login, cadastro, publicação de avisos e manutenção de usuários.

## Tecnologias Utilizadas

### 1. ErrorEagle-Jar

- Linguagem: Java 17
- Interface gráfica: Swing / NetBeans GUI Builder
- Layout: AbsoluteLayout
- Persistência/acesso a banco: Spring JDBC, Apache Commons DBCP2
- Bancos/Drivers: MySQL Connector/J, Microsoft SQL Server JDBC
- Monitoramento de hardware: Looca API
- Empacotamento: Maven Assembly Plugin

### 2. ErrorEagle-Web

- Runtime: Node.js
- Backend: Express.js
- Middlewares: CORS
- Acesso a banco: `mssql`, `mysql2`
- Processo de desenvolvimento: Nodemon
- Frontend: HTML, CSS e JavaScript puro
- Estilização adicional: Bootstrap, Popper.js, Sass

### 3. Projeto-ErrorEagle / web-data-viz

- Runtime: Node.js
- Backend: Express.js
- Middlewares: CORS
- Acesso a banco: `mssql`, `mysql2`
- Processo de desenvolvimento: Nodemon
- Frontend: HTML, CSS e JavaScript puro
- Estilização adicional: Bootstrap, Popper.js, Sass

## O Que Cada Projeto Faz

### ErrorEagle-Jar

Aplicação desktop responsável pelo operador do totem. O fluxo principal é:

- tela de login;
- validação do usuário na base Azure;
- verificação se a empresa e o funcionário estão ativos;
- validação/registro do totem com base no hostname da máquina;
- checagem de configuração do hardware;
- captura contínua de CPU, RAM, disco e rede;
- inserção das medições nas bases local e remota;
- geração de logs do processo.

### ErrorEagle-Web

Plataforma web com foco em operação e acompanhamento. Pelas rotas e modelos existentes, ela cobre:

- cadastro e autenticação de empresa e funcionários;
- manutenção de usuários ativos e inativos;
- visualização de totens por empresa;
- consulta de alertas por totem;
- dashboards consolidados com volume de alertas, porcentagens e filtros;
- relatórios de manutenção;
- atualização de senha.

### Projeto-ErrorEagle / web-data-viz

Versão web mais enxuta, voltada a:

- cadastro e login de usuários;
- publicação, edição, listagem e remoção de avisos;
- consulta de medidas em tempo real e últimas medições;
- visualização de dados por aquário/totem, conforme a modelagem da época;
- telas de cadastro, login, simulador e dashboard.

## Requisitos de Ambiente

- Java 17 para o cliente desktop.
- Maven 3.8+ para compilar o projeto Java.
- Node.js 16+ ou 18+ para os projetos web.
- npm para instalar dependências JavaScript.
- MySQL local, se for usar o ambiente de desenvolvimento.
- SQL Server/Azure SQL, se for usar o ambiente de produção.
- Docker opcional no projeto Java, caso você queira reproduzir a instalação automática do script auxiliar.

## Como Subir o ErrorEagle-Jar

### 1. Preparar o ambiente

Instale:

- JDK 17
- Maven
- Um banco MySQL local ou acesso ao banco SQL Server/Azure usado pelo projeto

### 2. Ajustar as conexões

Revise os arquivos de conexão:

- `ErrorEagle-Jar/tela-swing/src/main/java/conexoes/ConexaoLocal.java`
- `ErrorEagle-Jar/tela-swing/src/main/java/conexoes/ConexaoAzure.java`

Eles possuem credenciais e URLs definidas no código. Ajuste host, banco, usuário e senha para o seu ambiente.

### 3. Criar o banco

O projeto usa tabelas como:

- `Totem`
- `Componente`
- `Configuracao`
- `Medida`
- `TipoAlerta`
- `Limite`

O script de instalação automática em `ErrorEagle-Jar/Assistente_instalacao.sh.bash` mostra a estrutura esperada e também cria um container MySQL com os dados do projeto.

### 4. Compilar

No terminal, dentro de `ErrorEagle-Jar/tela-swing`:

```bash
mvn clean package
```

### 5. Executar

Depois do build, rode um dos JARs gerados em `ErrorEagle-Jar/tela-swing/target`.

Exemplo recomendado:

```bash
java -jar target/ErrorEagle-jar-with-dependencies.jar
```

Se o seu build gerar o arquivo `ErrorEagle.jar`, você também pode executar esse artefato diretamente.

### 6. Fluxo do programa

Ao iniciar, o sistema abre a tela de login. Depois de autenticar, ele valida o totem e começa a coletar métricas automaticamente.

## Como Subir o ErrorEagle-Web

### 1. Preparar o ambiente

Instale:

- Node.js
- npm
- Um banco MySQL ou acesso ao SQL Server/Azure

### 2. Instalar dependências

Na raiz `ErrorEagle-Web`:

```bash
npm install
```

Esse `package.json` raiz delega a instalação para a pasta `site`.

Se preferir, também pode instalar diretamente em `site`:

```bash
cd site
npm install
```

### 3. Configurar ambiente

Edite:

- `ErrorEagle-Web/site/app.js`
- `ErrorEagle-Web/site/src/database/config.js`

Pontos importantes:

- o arquivo `app.js` alterna entre ambiente de desenvolvimento e produção;
- `config.js` define as conexões com MySQL local e SQL Server/Azure;
- os dados de conexão estão hardcoded, então substitua pelas credenciais do seu ambiente.

### 4. Criar as tabelas

Execute o script SQL do projeto no banco escolhido:

- `ErrorEagle-Web/site/src/database/script-tabelas.sql`

### 5. Subir a aplicação

Na raiz `ErrorEagle-Web`:

```bash
npm start
```

Para desenvolvimento com reload, use:

```bash
npm run dev
```

### 6. Acessar

A aplicação sobe na porta definida pelo ambiente:

- `3333` para desenvolvimento
- `8080` para produção

## Como Subir o Projeto-ErrorEagle / web-data-viz

### 1. Preparar o ambiente

Instale:

- Node.js
- npm
- Um banco MySQL ou SQL Server, conforme o ambiente selecionado

### 2. Instalar dependências

Na raiz `Projeto-ErrorEagle`:

```bash
npm install
```

Isso instala dependências como Bootstrap e Sass.

Depois, entre em `web-data-viz/site` e instale as dependências do backend:

```bash
cd web-data-viz/site
npm install
```

### 3. Configurar ambiente

Edite:

- `Projeto-ErrorEagle/web-data-viz/site/app.js`
- `Projeto-ErrorEagle/web-data-viz/site/src/database/config.js`

O `app.js` define o ambiente como desenvolvimento por padrão. O `config.js` traz as credenciais e a URL de banco que devem ser adaptadas antes de subir a aplicação.

### 4. Criar as tabelas

Execute o script de banco deste projeto:

- `Projeto-ErrorEagle/web-data-viz/site/src/database/script-tabelas.sql`

### 5. Subir a aplicação

Dentro de `Projeto-ErrorEagle/web-data-viz/site`:

```bash
npm start
```

Para desenvolvimento com nodemon:

```bash
npm run dev
```

### 6. Compilar CSS, se necessário

Se você quiser recompilar os estilos Sass a partir da raiz `Projeto-ErrorEagle`:

```bash
npm run sass
```

Ou gerar CSS comprimido:

```bash
npm run sass-min
```

## Observações Importantes

- Os três projetos usam credenciais e URLs de banco diretamente no código. Antes de rodar em outro ambiente, revise os arquivos de configuração.
- O projeto Java depende da biblioteca Looca para coletar dados da máquina local.
- O fluxo de produção usa SQL Server/Azure; o fluxo de desenvolvimento usa MySQL local.
- A pasta `target/` do projeto Java já contém artefatos gerados, mas o caminho correto é recompilar antes de executar se houver alteração no código-fonte.

## Estrutura Resumida

```text
ErrorEagle/
├─ ErrorEagle-Jar/
├─ ErrorEagle-Web/
└─ Projeto-ErrorEagle/
```

## Próximos Passos Sugeridos

- Se quiser, posso gerar READMEs separados para cada projeto, mais curtos e específicos.
- Também posso adaptar este README para um tom mais acadêmico ou mais profissional de portfólio.
