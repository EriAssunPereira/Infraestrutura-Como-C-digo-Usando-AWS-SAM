# Infraestrutura-Como-Código-Usando-AWS-SAM

Para configurar infraestrutura como código usando AWS SAM (Serverless Application Model), vou estruturar o projeto em módulos. O AWS SAM simplifica o desenvolvimento de aplicações serverless na AWS, permitindo definir recursos como funções Lambda, APIs Gateway, tabelas DynamoDB, entre outros, de forma declarativa.

### Passo 1: Configuração Inicial

1. **Configuração da AWS:**
   - Certifique-se de ter uma conta AWS e acesso à AWS Management Console.
   - Instale o AWS CLI e configure suas credenciais locais.

2. **Instalação do AWS SAM CLI:**
   - Instale o AWS SAM CLI seguindo as instruções em [AWS SAM CLI Installation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html).

### Passo 2: Estrutura do Projeto AWS SAM

Estruture o projeto AWS SAM com os seguintes arquivos:

```plaintext
sam-project/
│
├── template.yaml
├── README.md
├── events/
│   └── event.json
├── src/
│   └── lambda_function.py
└── tests/
    └── test_lambda.py
```

### Passo 3: Configuração do `template.yaml`

O `template.yaml` é o arquivo principal onde você define os recursos e configurações da sua aplicação serverless.

**`template.yaml`**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'

Resources:
  HelloWorldFunction:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: src/lambda_function.handler
      Runtime: python3.8
      CodeUri: .
      Events:
        HelloWorldApi:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

### Passo 4: Implementação da Função Lambda

Crie o código da função Lambda em Python dentro da pasta `src/`.

**`src/lambda_function.py`**

```python
import json

def handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello, world!')
    }
```

### Passo 5: Implantação usando AWS SAM CLI

1. **Build da Aplicação:**
   - Execute o comando para compilar sua aplicação:
     ```bash
     sam build
     ```

2. **Deploy da Aplicação:**
   - Faça o deploy da aplicação na AWS:
     ```bash
     sam deploy --guided
     ```
     - O comando `sam deploy --guided` guiará você através das opções de configuração para o deploy. Você pode definir parâmetros como nome do stack, região AWS, e outros.

### Passo 6: Teste da Aplicação

Após o deploy, teste a aplicação acessando a URL fornecida pelo AWS API Gateway ou usando o AWS CLI.

### Passo 7: Exemplo de Teste Automatizado

Adicione testes automatizados para sua função Lambda.

**`tests/test_lambda.py`**

```python
import pytest
from src.lambda_function import handler

def test_lambda_handler():
    event = {}
    context = {}
    response = handler(event, context)

    assert response['statusCode'] == 200
    assert 'Hello, world!' in response['body']
```

### Observações Finais

O AWS SAM permite que possamos definir e implantar facilmente aplicações serverless na AWS, gerenciando recursos como funções Lambda, APIs Gateway, e outros serviços. Este exemplo básico pode ser expandido adicionando mais recursos ao `template.yaml`, como tabelas DynamoDB, filas SQS, e configurações de permissões IAM. Certifique-se de explorar a documentação oficial do AWS SAM para aprender mais sobre suas capacidades avançadas e melhores práticas para desenvolvimento serverless na AWS.
