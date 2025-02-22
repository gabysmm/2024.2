# Docker compose com Django e banco de dados

## Informações gerais

- Assunto: Docker, conteinizar aplicativos
- Disciplina: *Sistemas operacionais*

## Relatório

### Aluno

- *nome:* Gabrielly Stéphany Siqueira Magalhães Martins
- *matrícula:* 20232014040001

### Relato
Para realizar a atividade, foi criada uma branch chamada docker no repositório do PDS Web Bizzu. Dentro dela, foram adicionados dois arquivos principais: Dockerfile e docker-compose.yml.

No início, o Dockerfile foi configurado para gerar a imagem da aplicação, utilizando como base uma imagem do Python. Durante esse processo, foi criado o diretório /bizzu dentro do container e instaladas todas as dependências listadas no arquivo requirements.txt.

Entretanto, a criação da imagem por si só não executa a aplicação. Por isso, foi necessário configurar o arquivo docker-compose.yml, que permite a execução da aplicação através da definição de um service. Nesse service, foram configurados alguns elementos essenciais para o funcionamento correto do sistema, como:

- Build: Define o caminho para o Dockerfile, usando . para indicar que ele está no mesmo diretório.
- Command: Especifica o comando a ser executado para rodar a aplicação. No caso do Django, utilizamos bash -c para concatenar os comandos de migrate e runserver, garantindo que ambos sejam executados corretamente.
- Ports: Define a conexão entre a porta do container e a porta na qual a aplicação será acessível.

Além disso, para configurar o banco de dados, foi necessário modificar o arquivo de settings do Django, substituindo o SQLite pelo PostgreSQL. Para isso, adicionamos um novo service no docker-compose.yml e fizemos algumas configurações, como:

- container_name: Define o nome do container que armazenará o banco de dados.
- ports: Especifica a conexão entre a porta do container e a porta do banco.
- image: Utiliza a imagem oficial do PostgreSQL.
- environment: Define variáveis de ambiente, como usuário e senha, para a conexão com o banco.
- volumes: Configura um armazenamento persistente para os dados do banco.

Com essas configurações, criamos um novo serviço chamado db, que encapsula o PostgreSQL dentro de um container. Vale ressaltar que não foi necessário criar um novo Dockerfile para o banco de dados, pois o PostgreSQL já possui sua própria imagem oficial, que pode ser utilizada sem modificações adicionais.<br>
### Arquivos docker e de configuração do django
Os arquivos não são compatíveis com Markdown, então foram adicionados no mesmo diretório do relatório. Cada arquivo foi nomeado de forma intuitiva para indicar seu conteúdo:

- Dockerfile: Contém as instruções para criar a imagem da aplicação.
- settings.py: Inclui as configurações do projeto no Django, incluindo a configuração do banco de dados.
