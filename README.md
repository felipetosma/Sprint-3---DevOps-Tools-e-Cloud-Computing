OdontoFast
Sistema de gerenciamento de clínicas odontológicas implantado no Azure usando Java, Spring Boot e SQL Server.
🦷 Sobre o Projeto
OdontoFast é uma aplicação web que permite o gerenciamento de clínicas odontológicas, oferecendo funcionalidades como cadastro de dentistas, login e atualização de perfil. Toda a infraestrutura do projeto está hospedada na nuvem Azure utilizando serviços como Azure Web App e SQL Database.
🚀 Tecnologias Utilizadas

Backend: Java 21, Spring Boot
Banco de Dados: SQL Server (Azure SQL Database)
Hospedagem: Azure Web App
Build e Deploy: Maven, Azure CLI
Controle de Versão: Git/GitHub

🛠️ Requisitos

Java 21
Maven
Azure CLI
Conta Azure (recomendada Azure for Students)
Git

⚙️ Configuração da Infraestrutura no Azure
1. Criar Grupo de Recursos
bashCopiaraz group create --name rg-webapp-odontofast --location eastus2
2. Configurar o SQL Server
bashCopiar# Criar SQL Server
az sql server create --resource-group rg-webapp-odontofast --name serversql-odontofast --location eastus2 --admin-user sqladmin --admin-password Fiap@2ds2025

# Criar a base de dados
az sql db create --resource-group rg-webapp-odontofast --server serversql-odontofast --name database-odontofast --service-objective S0

# Configurar regras de firewall (permitir todos os IPs)
az sql server firewall-rule create --resource-group rg-webapp-odontofast --server serversql-odontofast --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
3. Configurar o Azure Web App
bashCopiar# Criar plano de serviço
az appservice plan create --name plan-webapp-odontofast --resource-group rg-webapp-odontofast --sku FREE

# Criar o Web App
az webapp create --resource-group rg-webapp-odontofast --plan plan-webapp-odontofast --name webapp-odontofast

# Configurar string de conexão com o banco de dados
az webapp config connection-string set \
    --resource-group rg-webapp-odontofast \
    --name webapp-odontofast \
    --settings "DefaultConnection=Server=tcp:serversql-odontofast.database.windows.net,1433;Initial Catalog=database-odontofast;Persist Security Info=False;User ID=sqladmin;Password=Fiap@2ds2025;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;" \
    --connection-string-type SQLAzure
📋 Configuração do Projeto
1. Clone do Repositório
bashCopiargit clone https://github.com/seu-usuario/odontofast.git
cd odontofast
2. Verificação do Ambiente
Certifique-se de ter Java 21 e Maven instalados:
bashCopiarjava --version
mvn --version
3. Configuração do Banco de Dados
Verifique o arquivo src/main/resources/application.properties para garantir que as credenciais do banco de dados estejam corretas:
propertiesCopiarspring.datasource.url=jdbc:sqlserver://serversql-odontofast.database.windows.net:1433;database=database-odontofast;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=sqladmin
spring.datasource.password=Fiap@2ds2025
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.SQLServerDialect
🚀 Build e Deploy
Compilando o Projeto
bashCopiarmvn clean package
Realizando Deploy no Azure
bashCopiarmvn azure-webapp:deploy
📊 Estrutura do Projeto
O projeto segue a estrutura padrão do Spring Boot:
Copiarsrc
├── main
│   ├── java
│   │   └── br
│   │       └── com
│   │           └── odontofast
│   │               ├── controllers
│   │               ├── models
│   │               ├── repositories
│   │               ├── services
│   │               └── OdontofastApplication.java
│   └── resources
│       ├── static
│       ├── templates
│       └── application.properties
└── test
    └── java
        └── br
            └── com
                └── odontofast
🔌 Dependências Principais
O arquivo pom.xml contém as dependências necessárias para o projeto:

Spring Boot Starter Web: Para criar a aplicação web
Spring Boot Starter Data JPA: Para acesso a dados com JPA
Microsoft SQL Server Driver: Para conexão com o SQL Server
Azure WebApp Maven Plugin: Para facilitar o deploy no Azure
Spring Boot Starter Security: Para autenticação e autorização
Spring Boot Starter Thymeleaf: Para templates HTML

🌐 Acessando a Aplicação
Após o deploy, a aplicação estará disponível em:

Login: https://webapp-odontofast.azurewebsites.net/dentista/login
Cadastro: https://webapp-odontofast.azurewebsites.net/dentista/cadastro

📝 Gerenciamento do Banco de Dados
Para acessar e gerenciar o banco de dados diretamente no Azure:

Acesse o portal do Azure
Navegue até o grupo de recursos rg-webapp-odontofast
Selecione o banco de dados database-odontofast
Use o "Editor de Consultas" no menu lateral
Insira as credenciais:

Usuário: sqladmin
Senha: Fiap@2ds2025
