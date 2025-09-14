
# AWS CloudFormation - Primeira Stack
Este repositório documenta minha experiência prática com AWS CloudFormation, onde implementei minha primeira stack de infraestrutura como código (IaC). O projeto serviu como introdução aos conceitos fundamentais de provisionamento automatizado de recursos na AWS.

## Índice
Visão Geral do Projeto

Conceitos Fundamentais

Arquitetura da Stack

Implementação

Anotações e Insights

Recursos Úteis

## Visão Geral do Projeto
Este laboratório prático teve como objetivo criar uma stack básica usando AWS CloudFormation, provisionando recursos de forma automatizada e reproduzível através de código.

## Conceitos Fundamentais
O que é AWS CloudFormation?
Serviço de infraestrutura como código (IaC) que permite modelar e provisionar recursos AWS de forma automatizada e segura usando templates JSON ou YAML.

Vantagens do CloudFormation:
Infraestrutura como código: Versionamento e reprodutibilidade

Gerenciamento de dependências: Criação e destruição ordenada de recursos

Rollback automático: Reversão em caso de falha na criação

Drift detection: Detecção de alterações manuais na infraestrutura

Componentes Principais:
Template: Arquivo YAML/JSON que descreve os recursos

Stack: Conjunto de recursos provisionados a partir de um template

Change Set: Preview das mudanças antes da aplicação

Parameters: Valores personalizáveis para o template

## Arquitetura da Stack
Implementei uma stack simples contendo:

```
Stack CloudFormation → EC2 Instance → Security Group
→ S3 Bucket
 ```

##  Implementação
1. Template CloudFormation (YAML)
```
AWSTemplateFormatVersion: '2010-09-09'
Description: Minha primeira stack CloudFormation - Laboratório prático

Parameters:
  InstanceType:
    Description: Tipo de instância EC2
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0c02fb55956c7d316
      KeyName: my-key-pair
      SecurityGroups:
        - !Ref MySecurityGroup
      Tags:
        - Key: Name
          Value: !Sub "EC2 Instance from CloudFormation"

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Security Group para instância EC2
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "my-bucket-${AWS::AccountId}-${AWS::Region}"
      AccessControl: Private
      Tags:
        - Key: Purpose
          Value: Laboratory

Outputs:
  InstanceId:
    Description: ID da instância EC2 criada
    Value: !Ref MyEC2Instance
  BucketName:
    Description: Nome do bucket S3 criado
    Value: !Ref MyS3Bucket
  SecurityGroupId:
    Description: ID do Security Group criado
    Value: !Ref MySecurityGroup
    
```

2. Comandos para implantação

```
#Criar stack
aws cloudformation create-stack \
  --stack-name minha-primeira-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.micro

aws cloudformation describe-stacks --stack-name minha-primeira-stack

aws cloudformation list-stack-resources --stack-name minha-primeira-stack

aws cloudformation delete-stack --stack-name minha-primeira-stack
```

## Anotações e Insights

Lições Aprendidas:
Vantagens do CloudFormation:

Controle de versão da infraestrutura

Documentação automática do ambiente

Recriação rápida de ambientes inteiros

Padronização entre diferentes contas/regiões


Desafios Encontrados:
Curva de aprendizado da sintaxe YAML/JSON

Depuração de erros de template

Gerenciamento de dependências entre recursos

Limites de recursos por stack


Melhores Práticas:
Usar Parameters para valores customizáveis

Implementar Outputs para exportar informações importantes

Utilizar Change Sets antes de atualizar stacks produtivas

Organizar templates complexos com Nested Stacks


Funcionalidades Úteis:
Intrinsic Functions: !Ref, !Sub, !GetAtt

Conditions: Criar recursos condicionalmente

Mappings: Valores baseados em região ou ambiente

Stack Policies: Controlar quais recursos podem ser atualizados


Integrações:
CloudFormation + CodePipeline para CI/CD de infraestrutura

Integração com serviços de monitoramento para detectar drift

Uso com AWS Config para compliance


## Próximos Passos
Para aprofundar os conhecimentos em AWS CloudFormation, planejo:

Explorar Nested Stacks para templates complexos

Implementar Custom Resources para recursos não nativos

Integrar com AWS CodePipeline para CI/CD de infraestrutura

Testar funcionalidades avançadas como Drift Detection

Explorar o CloudFormation Registry para recursos de terceiros

Este repositório será mantido e atualizado conforme novos conhecimentos forem adquiridos sobre AWS CloudFormation e infraestrutura como código.

