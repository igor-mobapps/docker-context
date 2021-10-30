## Docker Build Context
O contexto é o local onde reside o conjunto de arquivos que serão referenciados dentro do container.

Sintaxe

```
docker build    [Options]   [Context]
docker build -t tag-custom     .
```

Isso é importante quando o Dockerfile não está localizado no mesmo nível do contexto a ser construído dentro do container.

Exemplo:


```
// Estrutura do projeto

|project
  |_ backend
    |_ .docker
      |_ Dockerfile
    |_ src
    |_ public
  |_ frontend
    |_ public
```

No exemplo acima o `Dockerfile` está dentro de um sub-diretório `.docker`, e para que possa buildar o contexto do diretório `backend`, seria necessário dizer qual é o contexto a ser construído, e isso depende do path que o comando `docker build ...` está sendo executado.

Exemplo: Levando a estrutura de diretórios acima e tendo o contexto principal o conteúdo do `backend`.

```
// docker build sendo executado a partir do project/backend/.docker
docker build -t custom -f Dockerfile ../

// docker build sendo executado a partir do project/backend
docker build -t custom -f .docker/Dockerfile .

// docker build sendo executado a partir do project
docker build -t custom -f backend/docker/Dockerfile backend
```

### Docker Compose
O contexto do `docker-compose` é sempre relativo a partir da construção de onde o `docker-compose.yml` está localizado.

Exemplo 01:
```
// Estrutura do projeto

|project
  |_ backend
    |_ .docker
      |_ Dockerfile
    |_ src
    |_ public
  |_ frontend
    |_ public
  |_ docker-compose.yml
```

```
version: '3.9'
services:
  api:
    build:
      context: backend
      dockerfile: .docker/Dockerfile
    ports:
      - 8888:80
```

Exemplo 02:
```
// Estrutura do projeto

|project
  |_ backend
    |_ .docker
      |_ docker-compose.yml
    |_ src
    |_ public
    |_ Dockerfile
  |_ frontend
    |_ public
```

```
version: '3.9'
services:
  api:
    build:
      context: ..
      dockerfile: Dockerfile
    ports:
      - 8888:80
```


Exemplo 03:
```
// Estrutura do projeto

|project
  |_ backend
    |_ .docker
      |_ docker-compose.yml
      |_ Dockerfile.x.dev
    |_ src
    |_ public
  |_ frontend
    |_ public
```

```
version: '3.9'
services:
  api:
    build:
      context: ..
      dockerfile: .docker/Dockerfile.x.dev
    ports:
      - 8888:80
```
