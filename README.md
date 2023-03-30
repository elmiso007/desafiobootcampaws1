# Desafio de Projeto

# Adicionando Segurança em APIs na AWS com Amazon Cognito

Ofereçer autenticação, autorização e gerenciamento de usuários para suas aplicações Web e Mobile com o Amazon Cognito. Esse serviço, totalmente gerenciado pela AWS, suporta os principais mecanismos de segurança do mercado, além da integração com terceiros, como Facebook, Google, Apple ou a própria Amazon. Bootcamp organizado pela [DIO](https://web.dio.me "Site da DIO").

# Serviços usados pela AWS
- Amazon Cognito
- Amazon DynamoDB
- Amazon API Gateway
- AWS Lambda

## Etapas do desenvolvimento
### Criando uma API REST no Amazon API Gateway

  - No API Gateway Dashboard foi criada uma API, usando a opção REST API.
  - No protocolo REST ao creiar a nova API nomeei a API [dio_bootcamp_aws], configurei o Endpoint Type como Regional Create API
  - Na sequencia foi criado um recurso usando o caminho Resources -> Actions -> Create Resource -> Resource Name nomeando como [Items] -> Create Resource
  
### Na dashboard do Amazon DynamoDB

  - Foi criada uma tabela usando o seguinte caminho DynamoDB Dashboard -> Tables -> Create table -> Table name nomeando como [Items] -> Partition key [id] -> Create table
  
### Na dashboard do AWS Lambda

  - Foi criada uma função para inserir item usando o caminho abaixo
  - Lambda Dashboard -> Create function -> Name [dio_put_items_function] -> Create function
  - Inseri o código da função put_item_function.js disponível na pasta /src -> e foi feito o Deploy
  - Configuration -> Execution role -> Abrir a Role no console do IAM
  - IAM -> Roles -> Role criada no passo anterior -> Permissions -> Add inline policy
  - Service - DynamoDB -> Manual actions -> add actions -> putItem
  - Resources -> Add arn -> Selecionei o arn da tabela criada no DynamoDB -> Add
  - Review policy -> Name [lambda_dynamodb_putItem_policy] -> Create policy
  
### Integrando o API Gateway com o Lambda backend

  - API Gateway Dashboard -> Selecionei a API criada -> Resources -> Selecionei o resource criado -> Action -> Create method - POST
  - Integration type -> Lambda function -> Use Lambda Proxy Integration -> Lambda function -> Selecionei a função Lambda criada -> Save
  - Actions -> Deploy API -> Deployment Stage -> New Stage [dev] -> Deploy
  
### Usando o programa POSTMAN

  - Add Request -> Method POST -> Copiar o endpoint gerado no API Gateway
  - Body -> Raw -> JSON -> Adicionar o seguinte body
{
  "id": "400",
  "price": 1500
}
  - Send
  
### Na dashboard do Amazon Cognito

  - Configurei o Amazon Cognito usando os seguintes passos
  - Cognito Dashboard -> Manage User Pools -> Create a User Pool -> Pool name [TestPool]
  - How do you want your end users to sign in? - Email address or phone number -> Next Step
  - What password strength do you want to require?
  - Do you want to enable Multi-Factor Authentication (MFA)? Off -> Next Step
  - Do you want to customize your email verification messages? -> Verification type - Link -> Next Step
  - Which app clients will have access to this user pool? -> App client name [TestClient] -> Create App Client -> Next Step
  - Create Pool
  - App integration -> App client settings -> Enabled Identity Providers - Cognito User Pool
  - Callback URL(s) [https://example.com/logout]
  - OAuth 2.0 -> Allowed OAuth Flows - Authorization code grant -Implicit grant
  - Allowed OAuth Scopes - email - openid
  -Save Changes
  - Domain name -> Domain prefix [diolive] -> Save
  
### Criando um autorizador do Amazon Cognito para uma API REST no Amazon API Gateway

  - API Gateway Dashboard -> Selecionei a API criada -> Authorizers -> Create New Authorizer
  - Name [CognitoAuth] -> Type - Cognito -> Cognito User Pool [pool criada anteriormente] -> Token Source [Authorization]
  - Resources -> selecionei o resource criado -> selecionei o método criado -> Method Request -> Authorization - Selecionei o autorizador criado
  
## Retornando ao POSTMAN

  - Add request -> Authorization
  - Type - OAuth 2.0
  - Callback URL [https://example.com/logout]
  - Auth URL [https://desafiodio.auth.us-east-1.amazoncognito.com/login]
  - Client ID - obter o Client ID do Cognito em App clients
  - Scope [email - openid]
  - Client Authentication [Send client credentials in body]
  - Get New Acces Token
  - Copiar o token gerado
  - Selecionar a request para inserir item criada -> Authorization -> Type - Bearer Token -> Inserir o token copiado
  - Send
  
## Resultado Final

  - A aplicação está funcionando com autorização via token e inserindo dados para o AWS Lambda como mostra a imagem abaixo do POSTMAN.
  
  
![Mobile 1](https://github.com/acenelio/assets/raw/main/sds1/mobile1.png) ![Mobile 2](https://github.com/acenelio/assets/raw/main/sds1/mobile2.png)

## Layout web
![Web 1](https://github.com/acenelio/assets/raw/main/sds1/web1.png)

![Web 2](https://github.com/acenelio/assets/raw/main/sds1/web2.png)

## Modelo conceitual
![Modelo Conceitual](https://github.com/acenelio/assets/raw/main/sds1/modelo-conceitual.png)

# Autor

Emerson Pereira Ramos

https://www.linkedin.com/in/emerramos

