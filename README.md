# Centralização de Projetos com Docker

Este projeto visa centralizar e facilitar a utilização de múltiplos projetos através do Docker, permitindo adicionar novos projetos e utilizá-los juntos em um ambiente Docker.

## Estrutura do Projeto

- `ExemploProjeto/`: Contém o projeto Exemplo.
- `docker-compose.yml`: Define os serviços Docker para os projetos.
- `docker-compose.override.yml`: Configurações adicionais para desenvolvimento.

## Pré-requisitos

- [Docker](https://www.docker.com/)
- [Visual Studio](https://visualstudio.microsoft.com/)

## Configuração Inicial

1. Clone este repositório.
2. Abra o projeto desejado no Visual Studio.
3. Execute o projeto uma vez pelo Visual Studio para gerar o certificado HTTPS.
4. Se estiver usando um banco de dados via Docker, gere as migrations normalmente e no `Program.cs` do projeto execute:

```csharp
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<ExemploDbContext>();
    await db.Database.MigrateAsync();
}
```

Isso garante que o banco atualize as migrations quando executado.

## Utilização

### Adicionando um Novo Projeto

1. Crie um novo projeto no Visual Studio.
2. Execute o projeto uma vez pelo Visual Studio para gerar o certificado HTTPS.
3. Adicione o novo projeto ao `docker-compose.yml` e `docker-compose.override.yml` seguindo o exemplo do serviço `exemploprojeto`.

### Executando os Projetos

Para iniciar todos os projetos, execute o comando:

```sh
docker-compose up
```

## Serviços

### ExemploProjeto

- Contexto de Build: `ExemploProjeto`
- Dockerfile: `ExemploProjeto/Dockerfile`
- Portas: 8080 (HTTP), 8081 (HTTPS)

### MySQL

- Imagem: `mysql:8.0`
- Porta: 3307

### Redis

- Imagem: `redis:7.0`
- Porta: 6379

### MariaDB

- Imagem: `mariadb:11.4.2`
- Porta: 3306

## Volumes

- `mysql_data`: Armazena os dados do MySQL.
- `mariadb_data`: Armazena os dados do MariaDB.
- `redis`: Armazena os dados do Redis, utilizado como cache, broker de mensagens e armazenamento de dados.

## Observações

- Certifique-se de executar os projetos pelo Visual Studio pelo menos uma vez para gerar os certificados HTTPS necessários.
- Use as variáveis de ambiente definidas do service ExemploProjeto como referencia nos arquivos `docker-compose.yml` e `docker-compose.override.yml` para configurar os serviços conforme necessário.

## Licença

Este projeto está licenciado sob os termos da licença MIT.