# Tutorial: Como Instalar um Servidor de Banco de Dados SQL no Docker

Bem-vindo ao tutorial que irá guiá-lo através do processo de instalação de um servidor de banco de dados SQL utilizando o Docker. Neste exemplo, abordaremos a instalação do PostgreSQL e do PGADMIN, além de demonstrar como criar uma network no Docker para facilitar a comunicação entre os containers.

## Passo 1: Preparação do Ambiente

Antes de começarmos, certifique-se de ter o Docker instalado em sua máquina. Caso ainda não o tenha, você pode encontrar instruções detalhadas de instalação no site oficial do Docker.

## Passo 2: Vamos criar a network

vamos criar uma rede dedicada para a nossa aplicação.
No seu terminal digite:

```bash
docker network create --drive bridge nome-da-rede
```

Esse comando criará uma rede no Docker, denominada "nome-da-rede". Com a rede estabelecida,
estaremos prontos para instalar o PostgreSQL e o PGADMIN,
conectando-os à network recém-criada, permitindo uma comunicação eficiente entre eles.

## Passo 3: Instalando o postgres eo pgadmin

Utilizaremos o seguinte comando para adicionar o PostgreSQL à nossa rede:

```bash
docker run --name postgres --network=nome-da-rede -p 5433:5432 -e POSTGRES_PASSWORD=postgres -d postgres
```

Agora, para instalar o PGADMIN, utilize o comando:

```bash
docker run --name meu-pgadmin --network=nome-da-rede -p 80:80 -e PGADMIN_DEFAULT_EMAIL=seu_email -e PGADMIN_DEFAULT_PASSWORD=sua_senha -d dpage/pgadmin4
```

1. **`docker run`**: Esse comando é utilizado para criar e executar um novo container a partir de uma imagem Docker.

2. **`--name meu-postgres`**: Especifica um nome personalizado para o container. Neste caso,
3. o container será nomeado como "meu-postgres". Isso facilita a referência ao container em vez de usar o ID do container.

4. **`--network=nome-da-rede`**: Indica qual rede Docker o container deve ser associado.
5. No exemplo, "nome-da-rede" é o nome da rede que você criou anteriormente com o comando `docker network create`.
6. Isso permite a comunicação entre containers na mesma rede.

7. **`-p 5433:5432`**: Mapeia a porta do host para a porta do container. Neste caso,
8. a porta 5432 do container (que é a porta padrão do PostgreSQL) é mapeada para a porta 5433 do host.
9. Isso significa que você pode acessar o PostgreSQL no host usando a porta 5433.

10. **`-e POSTGRES_PASSWORD=sua_senha`**: Define uma variável de ambiente no container,
11. que é utilizada pelo PostgreSQL para configurar a senha do usuário padrão.
12. Substitua "sua_senha" pela senha desejada.

13. **`-d`**: Executa o container em segundo plano (modo detached),
14. o que significa que o terminal fica liberado para ser usado enquanto o container continua rodando em segundo plano.

15. **`postgres`**: Especifica a imagem Docker a ser usada para criar o container.
16. Neste caso, estamos utilizando a imagem oficial do PostgreSQL.

Resumindo, o comando cria um container chamado "meu-postgres" associado à rede "nome-da-rede",
mapeia a porta 5432 do container para a porta 5433 do host, define uma senha para o PostgreSQL e executa o container em segundo plano.
Esse container estará pronto para ser utilizado como um servidor PostgreSQL isolado no ambiente Docker que você configurou.

## Ou...

Você pode usar o comando `docker-compose` para inicializar e gerenciar a inicialização de vários containers, incluindo a configuração de redes. O Docker Compose permite definir, configurar e executar aplicativos Docker com vários containers.

Aqui estão os passos básicos usando o Docker Compose:

1. **Crie um arquivo `docker-compose.yml`**: Este arquivo contém a configuração dos serviços, redes e volumes necessários para sua aplicação. Aqui está um exemplo para o PostgreSQL e PGADMIN:

   ```yaml
   version: "3"
   services:
     meu-postgres:
       image: postgres
       container_name: meu-postgres
       environment:
         POSTGRES_PASSWORD: sua_senha
       ports:
         - "5433:5432"
       networks:
         - nome-da-rede

     meu-pgadmin:
       image: dpage/pgadmin4
       container_name: meu-pgadmin
       environment:
         PGADMIN_DEFAULT_EMAIL: seu_email
         PGADMIN_DEFAULT_PASSWORD: sua_senha
       ports:
         - "80:80"
       networks:
         - nome-da-rede

   networks:
     nome-da-rede:
       driver: bridge
   ```

2. **Execute os containers com o Docker Compose**:

   ```bash
   docker-compose up -d
   ```

   O parâmetro `-d` é usado para iniciar os containers em segundo plano.

3. **Para parar os containers**:

   ```bash
   docker-compose down
   ```

   Este comando desligará e removerá os containers.

Lembre-se de ajustar os valores de senha, e-mail e outras configurações conforme necessário. O Docker Compose oferece uma maneira mais limpa e gerenciável de lidar com vários containers e suas configurações associadas.

## Conclusão

Agora, você possui um ambiente funcional com PostgreSQL e PGADMIN instalados e prontos para serem utilizados. Em futuros tutoriais, exploraremos como interagir com esses serviços e realizar operações básicas de banco de dados. Divirta-se explorando o mundo dos bancos de dados SQL no Docker!
