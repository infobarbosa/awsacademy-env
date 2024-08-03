Author: Prof. Barbosa<br>
Contact: infobarbosa@gmail.com<br>
Github: [infobarbosa](https://github.com/infobarbosa)

# Objetivo

O objetivo deste repositório é criar uma instância (máquina virtual) no serviço AWS EC2 que servirá aos propósito de ambiente de laboratório.

## Atenção aos Custos!

A utilização dos serviços AWS pode gerar custos significativos, especialmente se os recursos forem deixados ativos após a conclusão do laboratório. É altamente recomendável monitorar os custos através do console de faturamento da AWS.

#### Recomendações:
- **Monitore os custos:** Utilize o AWS Cost Explorer para acompanhar os gastos.
- **Elimine recursos desnecessários:** Após finalizar o laboratório, certifique-se de eliminar todos os recursos criados, como instâncias EC2, VPCs, subnets, e qualquer outro serviço utilizado.

Utilize o procedimento ao final desta página para eliminar os recursos criados.


## Região AWS

1. No console AWS verifique a região no topo à direita do console AWS, normalmente é **Norte da Virgínia** (`us-east-1`).<br>
**Não** altere essa configuração!
<div align="left">

</div>

## CloudShell
2. Selecione o ícone do serviço *CloudShell*.<br>
![img/001-cloudshell-icon.png](img/001-cloudshell-icon.png)

Será disponibilizada o terminal do CloudShell:
![img/002-cloudshell-terminal.png](img/002-cloudshell-terminal.png)

### Clonagem do repositório
3. Execute a seguinte instrução para clonar este repositório:
```
git clone https://github.com/infobarbosa/awsacademy-env.git
```

Output experado:
```
[cloudshell-user@ip-10-138-189-141 ~]$ git clone https://github.com/infobarbosa/awsacademy-env.git
Cloning into 'awsacademy-env'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
Receiving objects: 100% (6/6), done.
[cloudshell-user@ip-10-138-189-141 ~]$ 
```

4. Navegue para o diretório clonado:
```
cd awsacademy-env/
```

### CloudFormation - Criação do stack
Para criação da instância, vamos utilizar o script cloudformation `template.yaml` disponibilizado no repositório.<br>

5. A criação deve ser feita através do comando a seguir:
```
aws cloudformation create-stack --stack-name infobarbosa-stack --template-body file://template.yaml
```

Output esperado:
```
[cloudshell-user@ip-10-138-189-141 awsacademy-env]$ aws cloudformation create-stack --stack-name infobarbosa-stack --template-body file://template.yaml
{
    "StackId": "arn:aws:cloudformation:us-east-1:898985761452:stack/infobarbosa-stack/24d09450-51e3-11ef-a1f3-0affc644647b"
}
```

### CloudFormation - Conferindo o stack

6. Verifique o status de criação através do seguinte comando:
```
aws cloudformation describe-stack-events --stack-name infobarbosa-stack
```

Output esperado:
```
[cloudshell-user@ip-10-138-189-141 awsacademy-env]$ aws cloudformation describe-stack-events --stack-name infobarbosa-stack  
{
    "StackEvents": [
        {
            "StackId": "arn:aws:cloudformation:us-east-1:898985761452:stack/infobarbosa-stack/24d09450-51e3-11ef-a1f3-0affc644647b",
            "EventId": "392903b0-51e3-11ef-a6ba-0affd5ec2d63",
            "StackName": "infobarbosa-stack",
            "LogicalResourceId": "infobarbosa-stack",
            "PhysicalResourceId": "arn:aws:cloudformation:us-east-1:898985761452:stack/infobarbosa-stack/24d09450-51e3-11ef-a1f3-0affc644647b",
            "ResourceType": "AWS::CloudFormation::Stack",
            "Timestamp": "2024-08-03T21:56:23.004000+00:00",
            "ResourceStatus": "CREATE_COMPLETE"
        },
...
...

```

## Acesso à instância
7. Digite `EC2` na barra de busca do console AWS.<br>
![img/003-search-bar-ec2.png](img/003-search-bar-ec2.png)

8. A seguir clique no link **EC2**.
9. No menu lateral esquerdo clique em **Instances**.
10. Na lista de instâncias clique no link do *Instance ID* da instância com status *Running*.<br>
![img/004-ec2-console-select-instance.png](img/004-ec2-console-select-instance.png)
11. Clique no botão *Connect*<br>
![img/005-ec2-connect-button.png](img/005-ec2-connect-button.png)
Será aberta a tela de conexão via *ssh*.
![img/006-ec2-connect-ssh-screen.png](img/006-ec2-connect-ssh-screen.png)
12. Role a tela até o final e clique no botão **Connect**
![img/007-ec2-connect-button.png](img/007-ec2-connect-button.png)

Será aberta uma nova aba com um terminal.

![img/008-ec2-ssh-terminal.png](img/008-ec2-ssh-terminal.png)

# Parabéns!
Você concluiu a criação e o acesso à instância **EC2**.<br>
Agora é possível prosseguir para as próximas etapas do seu treinamento.


# CloudFormation - Destruindo o stack
Após a finalização do laboratório, você deve eliminar os componentes através do comando:
```
aws cloudformation delete-stack --stack-name infobarbosa-stack
```

Este comando não produz uma saída. Porém, é possível conferir através do `describe-stack-events` novamente:
```
aws cloudformation describe-stack-events --stack-name infobarbosa-stack 
```

Output esperado:
```
[cloudshell-user@ip-10-138-189-141 awsacademy-env]$ aws cloudformation describe-stack-events --stack-name infobarbosa-stack  
{
    "StackEvents": [
        {
            "StackId": "arn:aws:cloudformation:us-east-1:898985761452:stack/infobarbosa-stack/7359cb80-51e5-11ef-9996-0e7eadbf076b",
            "EventId": "MyRoute-DELETE_COMPLETE-2024-08-03T22:59:47.828Z",
            "StackName": "infobarbosa-stack",
            "LogicalResourceId": "MyRoute",
            "PhysicalResourceId": "rtb-0b3256cbc6841e285|0.0.0.0/0",
            "ResourceType": "AWS::EC2::Route",
            "Timestamp": "2024-08-03T22:59:47.828000+00:00",
            "ResourceStatus": "DELETE_COMPLETE",
            "ResourceProperties": "{\"RouteTableId\":\"rtb-0b3256cbc6841e285\",\"DestinationCidrBlock\":\"0.0.0.0/0\",\"GatewayId\":\"igw-0f00f82ad01666d4e\"}"
        },
```
