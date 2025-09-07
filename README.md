## Desafio DIO - Step Function

Este laboratório tem como objetivo consolidar o conhecimento sobre **AWS Step Functions**.  

O **AWS Step Functions** é um serviço da AWS que permite criar e coordenar fluxos de trabalho entre aplicativos e microserviços.  
Ele oferece uma interface visual que facilita a construção desses fluxos, definindo cada etapa (workflow) e a ordem em que os recursos serão executados.  

### Vamos então dar início ao nosso workflow:  

1. No console, acesse o **Step Functions** e clique em **Get Started**.
   
   <img width="1887" height="459" alt="image" src="https://github.com/user-attachments/assets/6b006765-bbeb-42a0-bb90-92b83c8c92fa" />

3. Crie seu próprio fluxo de trabalho do zero.
   
   <img width="1208" height="643" alt="image" src="https://github.com/user-attachments/assets/2362ca0f-4c95-4012-aa58-eebf19a98016" />

5. Crie a máquina de estado:  
   - Nome da máquina de estado: **Desafio-DIO**  
   - Tipo: **Padrão**  
   - Clique em **Continuar**
     
   <img width="1743" height="583" alt="image" src="https://github.com/user-attachments/assets/e97ab51b-9d2d-4001-bd81-97d04ce072da" />

   Em seguida, arraste os serviços da AWS necessários para a criação:  
   
   <img width="1910" height="737" alt="image" src="https://github.com/user-attachments/assets/5a012e96-5d47-4deb-956a-7a1868f2e7f3" />

7. Criei o fluxo de trabalho de acordo com a arquitetura definida no desafio anterior:  
   👉🏻 [Desafio DIO - Santander Code Girls](https://github.com/DrikaDev/Desafio-DIO-Santander-Code-Girls/tree/main)  
   
   <img width="792" height="502" alt="image" src="https://github.com/user-attachments/assets/545ee980-2529-464e-b63f-2b30f70acd44" />

---

### Explicação do fluxo criado no Step Function:

1. O usuário faz o upload do arquivo no **S3** - o evento de upload é o gatilho inicial;  
2. Configuramos uma notificação de evento no bucket S3 para acionar o **Step Function** via **Lambda**;
3. Os estados do **Step Function** via **Lambda** seriam:
```
{
  "Comment": "Fluxo de processamento de arquivos no S3",
  "StartAt": "ProcessarArquivo",
  "States": {
    "ProcessarArquivo": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGIAO:ID:function:NomeDaFuncaoLambda",
      "Next": "EnviarParaEC2"
    },
    "EnviarParaEC2": {
      "Type": "Task",
      "Resource": "arn:aws:states:::ec2:sendTask", 
      "Next": "Finalizar"
    },
    "Finalizar": {
      "Type": "Succeed"
    }
  }
}
```
> Onde:
> "Processar arquivo": chama a função Lambda para validar o arquivo;
> "EnviarParaEC2": envia o documento validado para a instância;
> "Finalizar": encerra o fluxo.

4. O Lambda então lê o objeto do S3;
5. Faz a validação e envia para o EC2;
6. Ao chegar no **EC2**, o **EBS** que está atachado pega o resultado e faz a gravação.

---
