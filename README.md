# Demanda 3 

Desenvolvida em ambiente WSL com Ubuntu 22.04.4 LTS e executada localmente utilizando o IP da máquina obtido com o comando ```ip a```.    

## **Como executar o projeto**  

1. Clone o repositório
```
git clone https://github.com/anycarolinys/jenkins_pipeline.git
```

2. Execute o container em segundo plano 
```
docker-compose up -d 
```

3. Deve haver um container em execução (Jenkins) confira com
```
docker ps
```

4. Ao consultar o serviço em
```
http://<localhost>:8082
```
É necessário acessar
```
docker logs <nome_do_container>
```
E copiar a senha exibida após "Please use the following password to proceed to installation:" para acessar a interface do Jenkins

5. Na próxima página da interface, instale as bibliotecas necessárias recomendadas

6. Faça login com
Nome de usuário: root  
Senha: root_password  
Nome: root 
E-mail: root@gmail.com (email de exemplo, não precisa ser real)

7. Clique em "New job"
    - Crie um nome (recomendado: flask-mysql)
    - Selecione Pipeline
    - Em "Definition" selecione a caixa de texto em "Pipeline script" e copie o conteúdo de ```Jenkinsfile```
    - Salve o job

8. No painel do job criado, clique em "Build now"

9. Acompanhe a execução do job em "Builds"

7. Para encerrar a execução do container  
```
docker-compose down
```

## **Como foi desenvolvido o projeto**  
**1. Busca por material de referência**
- Breve leitura em conteúdos para compreensão do conceito/propósito do Jenkins, referências utilizadas:
    - [Introduction To Jenkins Pipeline As Code (Jenkinsfile)](https://www.geeksforgeeks.org/introduction-to-jenkins-pipeline-as-code-jenkinsfile/)
    - [Using a Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
    - [Instalando jenkins com Docker](https://youtu.be/1ksmAvsxwxs?si=2sdLo7Slrgs5sqwg)
    - [Criando primeira Pipeline no Jenkins](https://youtu.be/BVE9En1lIx8?si=TTrLTnSP0XBFhI6k)  
    \*Os materiais não são lidos/vistos por completo, normalmente vou pulando até a parte que me interessa para agilizar o desenvolvimento

**2. Implementação do Docker Compose**  
- Segui os passos indicados no vídeo citado acima realizei a configuração necessária para o container do Jenkins

**3. Desenvolvimento do arquivo Jenkinsfile**
- Escolhi o repositório da [Demanda 1](https://github.com/anycarolinys/dockerized_flask_api.git) visto que são utilizados dois containers (Aplicação Flask e banco MySQL) como exigido por essa demanda
- A partir de então foram identificados e implementados os passos necessários para o pipeline

**4.Configuração do Jenkins e teste do pipeline**
- Execução do arquivo Jenkinsfile na interface e correção de erros

## **Resultados**
- Faça o clone de um repositório público do GitHub  
Stage 'Clone GitHub Repository' no Jenkinsfile  
- Execute um container Docker dentro da pipeline e Execute um container qualquer na etapa de Deploy  
Stage 'Deploy - Run Flask Project' no Jenkinsfile  
- Aplique um comando ou script dentro do container após sua execução  
Stage 'POST to Flask API' no Jenkinsfile 

## **Principais dificuldades**  
**1. Funcionamento do container da aplicação Flask**  
- Outro erro obtido foi a incapacidade do Jenkins se conectar com a aplicação Flask para realizar a requisição, tentei resolver colocando os containers na mesma rede da seguinte maneira:
    - Adicionando no docker-compose do repositório clonado e do Jenkins:
        ```
            networks:
            - my_network
        networks:
            my_network:
                driver: bridge
        ```

**2. Funcionamento do container da aplicação Flask**
- Obtive o erro "Could not import 'app'" nos logs do container do Flask, tentei resolver das seguintes maneiras:
    - Testar se o container exibia esse erro executando localmente, não exibia
    - Mudar o comando de execução de ```CMD ["flask", "run", "--host=0.0.0.0"]``` para ``` CMD ["sh", "-c", "flask run --host=0.0.0.0"]``` no Dockerfile
    - Criar um novo repositório (para não interferir na demanda 1) e explorar diferentes estruturas de pastas para aplicação 
        - As estruturas testadas podem ser vistas nos commits do [repositório criado](https://github.com/anycarolinys/flask_api_for_jenkins.git)  
    - As tentativas não foram eficazes e o erro persiste no pipeline 


## **O que poderia ter sido melhorado/realizado com mais tempo**  
- Os erro da aplicação Flask poderia ser corrigido ><
- Poderia ser criada outra aplicação para que se pudesse executar um comando dentro do container

## **Principais aprendizados**
- Instalação de Docker e Docker Compose em ambiente Linux
- Compreensão do conceito/propósito do Jenkins e familiarização sua interface

