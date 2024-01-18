# Lambda AWS java e API Gateway

Esse projeto foi executado a fim de estudar sobre AWS lambda e API Gateway em java utilizando o Intellij o plugin AWS toolkit:

- HelloWorldFunction/src/main - Código para a função Lambda da aplicação.
- events - ventos de invocação que você pode usar para chamar a função.. 
- template.yaml -Um modelo que define os recursos AWS da aplicação.


## Ferramentas para a aplicação
- LocalStack — https://localstack.cloud/
- AWS CLI — https://aws.amazon.com/pt/cli/
- SAM CLI — https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
- Java 11 — https://adoptium.net/
- Maven — https://maven.apache.org/install.html
- Docker — https://hub.docker.com/search/?type=edition&offering=community
- IntelliJ — https://www.jetbrains.com/pt-br/idea/
- AWS Toolkit — https://plugins.jetbrains.com/plugin/11349-aws-toolkit

 - OBS:  nesse repositorio tem o passo a passo opara instalar o localstack na sua máquina :

```bash
 https://github.com/IrisRPerrorni/StudyDocker
```

## Passo a passo para fazer essa aplicação rodar no localstack
### 1) Ter o AWS CLI instalado
Caso queira conferir se tem o aws instalado na sua máquina usar o seguinte comando no seu terminal :
```bash
aws --version
```
### 2) Ter o AWS CLI instalado
Caso queira conferir se tem o aws instalado na sua máquina usar o seguinte comando no seu terminal :
```bash
sam --version
```
### 3) Instalar o plugin da AWS no Intellij
Vai em file -> setting -> plugins 
e procura aws:
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/a0b61e20-9494-4002-b6dc-bcfe23948ce4)

### 4) Configurar o AWS SAM 
Vai em file -> setting -> AWS 
Normalmente ele ja vai está automaticamente o caminho do AWS SAM na aba SAM CLI executable, caso não estiver coloque o caminho do seu sam cli
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/f3114af3-331f-4473-9408-6cd27fca50d2)

### 5) Criar um projeto em java AWS lambda e colocar as dependências 
Codigo nesse repositório

### 6) Configurar a aplicação para executar o AWS lambda
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/1eeb1a51-e705-4c85-b91a-bb9fcc58fd46)

visualizando a resposta da execução: 
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/4ca2024b-ae4e-47db-bd90-d8b913e29cb8)

## 7) Executar a aplicação: 
- Inserir IAM Role para a execução da lambda:
```bash
aws --endpoint http://localhost:4566 --profile localstack iam create-role --role-name lambda-execution --assume-role-policy-document "{\"Version\": \"2012-10-17\",\"Statement\": [{ \"Effect\": \"Allow\", \"Principal\": {\"Service\": \"lambda.amazonaws.com\"}, \"Action\": \"sts:AssumeRole\"}]}"
```
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/b81f8e52-5a80-47a2-a889-9a397eadfc61)

- Com a IAM Role criada, vamos incluir a policy de execução:
```bash
aws --endpoint http://localhost:4566 --profile localstack iam attach-role-policy --role-name lambda-execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```
- Entrar no diretório onde está a sua função:
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/6084e4d0-4e19-447b-bfb5-6bce3c20755f)

- Para a implantação no LocalStack, devemos gerar o pacote que contém o código do Lambda, executando o seguinte comando:
e executar o seguinte comando:

```bash
mvn clean install -DskipTests
```
- Efetando a implantação do Lambda no LocalStack :
entrar na pasta target e jogar o comando: 

```bash
aws --endpoint http://localhost:4566 --profile localstack lambda create-function --function-name HelloWorld --zip-file fileb://HelloWorld-1.0.jar --handler helloworld.App --runtime java11 --role arn:aws:iam::000000000000:role/lambda-execution
```
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/90406994-3e4f-4e64-9f70-75a8d07baaba)

- Executando o Lambda e verificando se foi instalado corretamente:
```bash
aws --endpoint http://localhost:4566 --profile localstack lambda invoke --function-name HelloWorld out.txt --log-type Tail
```
![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/9ef97f95-21fc-4902-9f66-3b5a6a0d19c0)
Então ele gerou a saida da execução do lambda para o out.txt.
-Caso tenha executado com sucesso, verifique usando esse comando
- Executando o Lambda e verificando se foi instalado corretamente:
```bash
notepad out.txt
```
 o arquivo out.txt terá o seguinte conteúdo:
 ![image](https://github.com/IrisRPerrorni/LambdaAWS/assets/133882090/c42cf235-a281-44c6-b848-45b56c0e2711)

 {"statusCode":200,"headers":{"X-Custom-Header":"application/json","Content-Type":"application/json"},"body":"{ \"message\": \"hello world\" }"}

## Fonte de estudo
https://thomsdacosta.medium.com/java-aws-lambda-montando-um-ambiente-de-desenvolvimento-local-com-localstack-a845624bee40
