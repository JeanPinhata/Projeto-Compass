# **Atividade de Linux PB COMPASS UOL- JAN 2025 | DevSecOps **

### Este repositório apresenta a implementação de uma infraestrutura em nuvem utilizando serviços da AWS, desenvolvida como parte do Programa de Bolsas (PB) da Compass UOL, na trilha de estudos em AWS e DevSecOps.

# **Índice**
1. Resumo
   

2. Tecnologias Utilizadas

3. Objetivos
      - Configuração ambiente AWS
      - Configuração servidor web 
      - Automatização de alertas 
      - Testes e monitoramento 

4. Passo a Passo 
   - Criando uma VPC
   - Criando uma instância EC2 utilizando o Ubuntu
   - Fazendo uma conexão SSH
   - Instalação do Nginx 
   - Criação de um Webhook usando Discord 
   - Criação de um script de automação utilizando Bash 
   - Automatização de script usando cron 
   - Teste Webhook 
   - Criando página em HTML 
   - Testes finais

5. Testes 
   - Quando o sistema está Online 
   - Quando o sistema está Offline

6. Referências

## *1. Resumo*
A atividade foi realizada no Programa de Bolsas (PB) da Compass UOL, na trilha de estudos em AWS e DevSecOps. O principal objetivo é capacitar os alunos na implementação e configuração de uma infraestrutura em nuvem, seguindo princípios de segurança e automação.
A atividade propõe que os participantes devem criar uma VPC e uma instância EC2 com o sistema operacional Ubuntu. Em seguida, é necessário estabelecer uma conexão via SSH, instalar e configurar o servidor web Nginx e desenvolver uma página web utilizando HTML.

Além disso, foi implementado um sistema automatizado de monitoramento, aproveitando a funcionalidade de Webhook do Discord. Para isso, foi desenvolvido um script em Bash que verifica a disponibilidade do site a cada minuto e envia um alerta para um canal no Discord caso o serviço apresente alguma indisponibilidade.
 
## *2. Tecnologias Utilizadas*

- Linux 
- AWS
- VPC
- EC2
- Ngix
- HTML
- Discord
   
## *3. Objetivos*

- **Capacitação em nuvem:** Adquirir conhecimento prático na criação e configuração de uma estrutura em Cloud dentro da AWS utilizando serviços como VPC e instância EC2. 
- **Configuração servidor web:** Configurar o servidor Nginx para que hospede uma página simples criada com HTML. 
- **Configurações de segurança:** Adquirir conhecimentos de práticas de segurança e acesso remoto via SSH.
- **Testes e monitoramento:** Desenvolver um script que verifique a disponibilidade do site e a integração via Webhook do Discord para alertas automáticos em caso de indisponibilidade. 


## *4. Passo a Passo*

## Criando uma VPC
Para criar uma VPC na AWS, é necessário acessar a página inicial do console da AWS e localizar a opção VPC na tela inicial, ou buscá-la na barra de pesquisas.

![alt text](<imagens/Captura de tela 2025-02-21 154258.png>)

Logo depois clicamos em **Criar VPC**


![alt text](<imagens/Captura de tela 2025-02-21 102740.png>)

Agora faremos as configurações da nossa VPC. Marcamos o campo VPC e muito mais, escolhemos o nome que desejamos, conforme pede o projeto vamos configurar duas sub-redes públicas e duas privadas e então clicamos em criar VPC. 


![alt text](<imagens/Captura de tela 2025-02-21 103133.png>)
![alt text](<imagens/Captura de tela 2025-02-21 103152.png>)

Então faremos a configuração do nosso grupo de segurança (Security Group). Para tal iremos no menu lateral a esquerda da tela e iremos selecionar a opção **Grupos de Segurança**. Seremos direcionados para uma nova página e clicaremos no botão **Criar grupo de segurança** na parte superior do lado direito da tela. 

![alt text](<imagens/Captura de tela 2025-02-24 155332.png>)

Nesta nova página faremos nossas configurações do Grupo de segurança. Escolhemos o nome e também o utilizaremos na descrição, então selecionamos a VPC criada para vincularmos o grupo de segurança e faremos então as configurações de entrada, que nos permitirão o acesso remoto a nossa instância. Clicamos então em adcionar regra e abriremos dois campos para selecionarmos uma porta tipo SSH e a outra porta 80. Então clicamos no botão para criar o nosso grupo de segurança. 

![alt text](<imagens/Captura de tela 2025-02-24 155253.png>)
## Criando uma instância EC2 

Para criar uma instância EC2 na AWS, é necessário acessar a página inicial do console da AWS e localizar a opção EC2 na tela inicial, ou buscá-la na barra de pesquisas.


![alt text](<imagens/Captura de tela 2025-02-21 102649.png>)

Logo seremos direcionados para uma outra tela onde localizaremos o botão **Executar instância**


![alt text](<imagens/Captura de tela 2025-02-21 105321-1.png>)

Então começaremos as configuração de nossa EC2. Começaremos colocando o nome e as tags da nossa instância e escolheremos o servidor Ubuntu, verificamos suas configurações e o tipo de instância que utilizaremos, que em nosso caso será a **T2 micro**.

![alt text](<imagens/Captura de tela 2025-02-21 110228.png>)
![alt text](<imagens/Captura de tela 2025-02-21 110517.png>)

Agora partiremos para a configuração das chaves de segurança que nos permitirão a conexão SSH. Clicamos então em criar novo par de chaves e seremos direcionados para a página de configuração. Inserimos então o nome para o nosso par de chaves, selecionamos o tipo que em nosso caso será RSA e selecionamos o formato .pem que nos permitirá a conexão via SSH e então criamos o par de chaves.

![alt text](<Captura de tela 2025-02-21 110905.png>)
![alt text](<Captura de tela 2025-02-21 111007.png>)

Logo depois faremos as configurações de rede da nossa EC2. Primeiramente selecionaremos a VPC que acabamos de criar, escolheremos uma sub-net pública, habilitaremos o nosso Ip público, clicaremos em selecionar grupo de segurança existente e utilizaremos o que acabamos de criar, selecionaremos então as portas podem ser acessadas a nossa instância. Selecionaremos a **porta SSH** que nos garantirá uma conexão remota segura a instância e também o **Meu IP (HTTP)** que garantirá a conexão ao site hospedado no servidor. Por fim, executaremos a nossa nova instância. 

![alt text](<imagens/Captura de tela 2025-02-24 163924.png>)

## Conectando a instância via SSH 

Para que a conexão aconteça precisamos executar a instância e então clicar no botão **conectar**. Seremos direcionados para uma outra página e devemos selecionar **Cliente SSH** e no final da página copiar a **chave ssh-i** que foi baixada automaticamente em nossa máquina quando criada para conexão a instância pela prompt de comando do windows. 

![alt text](<imagens/Captura de tela 2025-02-21 113337.png>)


Abrimos então a prompt de comando e abrimos a pasta onde se encontra nossa chave .pem, que em nosso caso está na pasta de downloads então usaremos o comando **cd Downloads** e então colaremos a chave copiada na instância e estabeleceremos a conexão. 


![alt text](<imagens/Captura de tela 2025-02-21 114520.png>)


## Instalação do Nginx

Após a conexão na instância utilizamos o comando: `sudo -i` para acessarmos o usuário root e continuarmos com as configurações. Antes de iniciarmos a instalação do servidor executaremos o comando `sudo apt update` que garantirá que todos os pacotes que instaleremos seja feita da versão mais recente possível. Logo depois executaremos o comando `sudo apt install nginx`, esse comando baixará e instalará a versão mais recente do NGINX. Com o comando `nginx -v` podemos verificar a versão que foi instalada em nosso sistema. 


![alt text](<imagens/Captura de tela 2025-02-21 115222.png>)
**_Esta imagem mostra que todos os comandos funcionaram e a instação foi bem sucedida._**

### *Teste Nginx*

Na página da nossa instância EC2 podemos encontrar o nosso IP público, vamos copiar este IP e rodá-lo em nosso navegador, se o Nginx estiver instalado e configurado corretamente veremos a página inicial do Nginx. Se encontrar algum erro precisamos rever as nossas configurações. 


![alt text](<imagens/Captura de tela 2025-02-21 115818.png>)

- Percebemos que o servidor está funcionando perfeitamente, caso o servidor esteja parado pelo comando `system ctl stop nginx`ou qualquer outra razão, não mais aparecerá esta página, mas uma tela de erro e isso mostra que o servidor foi parado sem nenhum erro. 


![alt text](<imagens/Captura de tela 2025-02-21 120011-2.png>)

## Criação do Webhook utilizando Discord 

Precisamos criar um Webhook que será notificado a cada minuto caso haja indisponibilidade do servidor, faremos isso utilizando o **Discord**. Abrimos o Discord e em sua tela inicial no menu a esquerda clicaremos no sinal de **+**, ele abrirá um tela para criação do servidor e escolheremos a opção **"criar o meu"** e em seguida a opção **"para meus amigos e eu"**. Logo depois destes passos damos um nome ao nosso servidor e cliclamos em **"Criar"**. 

![alt text](<imagens/Captura de tela 2025-02-21 120432.png>)


Agora com o servidor criado, vamos habilitar o nosso Webhook, que é uma ferramenta usada para permitir a comunicação entre duas aplicações e enviar notificações em tempo real.
Dentro do servidor monitoramento, no menu a esquerda encontraremos **"geral"** e então clicaremos na engrenagem, logo depois a opção **"integrações"**, selecionaremos o botão **"criar webhook**", abrirá uma nova tela e então selecionaremos o botão "**novo wehook**" e então selecionaremos a aplicação que acabamos de criar e copiaremos a sua URL para que possamos automatizar o serviço. 


![](<imagens/Captura de tela 2025-02-21 120653.png>)

## Criação de script de automação com bash 

O Bash é um interpretador de linha de comando que permite executar comandos e scripts em Linux.Para isso precisamos verificar a instalação do bash em nosso terminal utilizando o comando `bash -- version`, caso não esteja instalado utilizaremos o comando `sudo apt-get install bash`. Com o comando `nano`criaremos o nosso arquivo *nano monitoramento.sh* e então dentro dele armazenaremos o nosso script. Logo depois de escrevermos o nosso script daremos o comando **Ctrl + x** para salvar as alterações em nosso arquivo e então voltaremos ao terminal. Segue abaixo o script utilizado.  

```
#!/bin/bash

WEBHOOK_URL= "Coloque aqui a URL copiada no discord"
SITE_URL="Coloque aqui seu IP público"

send_alert() {
    MESSAGE="$1"
    curl -H "Content-Type: application/json" -X POST -d "{\"content\": \"$MESSAGE\"}" "$WEBHOOK_URL"
}

check_site() {
    RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" "$SITE_URL")

    if [ "$RESPONSE" -eq 200 ]; then
        echo "O site está funcionando corretamente."
    else
        send_alert " O site $SITE_URL está fora do ar!"
    fi
}

check_site
```
![alt text](<imagens/Captura de tela 2025-02-21 132338.png>)

## Automação do script usando Crontab

O crontab é uma ferramenta do Ubuntu que permite agendar e automatizar a execução de tarefas em intervalos de tempo específicos. Para utilizá-lo digitaremos em nosso terminal o comando `crontab -e`. Ele abrirá um arquivo para edição e nele devemos inserir cinco astericos, cada um representa uma execução nos intervalos possíveis (minuto, hora, dia, mês e dia da semana), em seguida especificamos o caminho onde está armazenado nosso script. 


![alt text](<imagens/Captura de tela 2025-02-21 132935.png>)


## Teste Webhook e automação de alertas. 

Realizaremos o teste para verificar o serviço. Inicialmente verificaremos se o serviço nginx está ativo usando o comando `systemctl status nginx`. Caso ele esteja ativo, precisamos interrompê-lo com o comando `systemctl stop nginx`. Se todas as configurações estiverem corretas depois de um minuto começaremos a receber os alertas no discord. 


![alt text](<imagens/Captura de tela 2025-02-21 133356.png>)

## Criando a página em HTML


Neste passo faremos a configuração do Gninx para hospedar a nossa página HTML. Utilizaremos o comando `nano /etc/nginx/sites-available/meusite` para criarmos um arquivo Nano que guardará o código a seguir.

```
server {

   listen 80;

    server_name (Aqui utilize seu IP público)

    root /var/www/meusite;
    index index.html;

    location /{
        try_files $uri $uri/ =404;
    }
}

````
![](<imagens/Captura de tela 2025-02-21 134310.png>)

Logo depois iremos criar uma pasta de armazenamento usando o código `mkdir -p /var/www/meusite`, em seguida dentro dessa pasta criaremos um arquivo para armazenar o nosso script em HTML usando o código `nano /var/www/meusite/index.html`. Segue o código em HTML


```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Programa de Bolsas - Compass UOL</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            text-align: center;
        }
        .container {
            width: 80%;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-top: 20px;
        }
        h1 {
            color: #333;
        }
        .btn {
            display: inline-block;
            padding: 10px 20px;
            margin: 10px;
            color: white;
            background: #0073e6;
            border: none;
            border-radius: 5px;
            text-decoration: none;
            font-size: 16px;
        }
        .btn:hover {
            background: #005bb5;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Programa de Bolsas - Compass UOL</h1>
        <p>O programa de bolsas da Compass UOL oferece oportunidades para estudantes e profissionais que desejam se aprofundar em tecnologia e computação em nuvem.</p>
        <a href="#projeto-linux" class="btn">Saiba mais sobre o Projeto Linux</a>
    </div>
    
    <div class="container" id="projeto-linux">
        <h1>Projeto Linux - Deploy na AWS</h1>
        <p>Este projeto ensina como criar uma infraestrutura na AWS e configurar um servidor web.</p>
        <ul style="text-align: left;">
            <li>Criar uma VPC</li>
            <li>Configurar uma instância EC2</li>
            <li>Instalar e configurar o Nginx</li>
            <li>Criar um Webhook</li>
            <li>Criar e publicar uma página HTML</li>
        </ul>
        <a href="#passos" class="btn">Ver Passo a Passo</a>
    </div>

    <div class="container" id="passos">
        <h1>Passo a Passo</h1>
        <h2>1. Criando uma VPC</h2>
        <p>Usamos o AWS VPC para criar uma rede isolada na nuvem.</p>
        <h2>2. Criando uma Instância EC2</h2>
        <p>Selecionamos uma instância Linux e configuramos regras de acesso.</p>
        <h2>3. Instalando Nginx</h2>
        <p>Com o comando <code>sudo apt update && sudo apt install nginx</code> instalamos o servidor.</p>
        <h2>4. Criando um Webhook</h2>
        <p>Configuramos um webhook para deploy automático de arquivos HTML.</p>
        <h2>5. Criando e Subindo uma Página HTML</h2>
        <p>O arquivo HTML é copiado para <code>/var/www/html</code> no servidor.</p>
    </div>
</body>
</html>
```

![](<imagens/Captura de tela 2025-02-21 135119.png>)

Após o armazenamento do script devemos realizar os seguintes comandos:

- `ln -s /etc/nginx/sites-available/meusite /etc/nginx/sites-enable`. Este comando cria um link simbólico, que é como um atalho para o arquivo original.
- `nginx -t`. Este comando serve para testar a sintaxe dos arquivos de configuração do Nginx. Caso tudo esteja correto, receberemos a seguinte mensagem:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
- `systemctl restart nginx`. Este comando é utilizado para reiniciar o Nginx. 
- `ufw allow 'Nginx Full'`. Este comando abre o firewall para que seu servidor web Nginx possa receber conexões externas.


**Esses comandos são as configurações necessárias para que o script seja executado dentro do Nginx e seja transmitido online via HTTP para outros computadores. E depois disso temos um site, em html rodando dentro do serviço Nginx.**


![alt text](<imagens/Captura de tela 2025-02-21 141515.png>)

## Testes finais

Para finalizar vamos testar a funcionalidade do site e também o envio de alertas ao discord em casos onde o servidor nginx não estiver respondendo. 

*Quando o servidor estiver ativo*

- Utilizamos o comando `systemctl start nginx` para ativar o servidor, então verificamos o status usando o comando `systemctl status nginx`. Essa imagem mostra o status do nginx e que o site está funcionando perfeitamente. 


![alt text](<imagens/Captura de tela 2025-02-21 141728.png>)

*Quando o servidor estiver inativo*

- Utilizamos o comando `systemctl stop nginx` para ativar o servidor, então verificamos o status usando o comando `systemctl status nginx`. Essa imagem mostra que o site está fora do ar, o status inativo do nginx e os alertas chegando no discord. 


![alt text](<imagens/Captura de tela 2025-02-21 142121.png>)

## Considerações finais 

Concluímos que o projeto atingiu com sucesso seus objetivos, entregando uma solução prática e automatizada para o monitoramento de serviços web. A infraestrutura, baseada em VPC, EC2 e Nginx, garante a segurança e o desempenho do sistema de monitoramento. A integração com o Discord garante que as equipes sejam notificadas sobre qualquer falha, possibilitando uma ação rápida e eficiente. Como resultado, o projeto otimiza a eficiência operacional, reduzindo o tempo de resposta a incidentes e aumentando a disponibilidade dos serviços. Além disso, a solução é escalável e pode ser adaptada para monitorar outros serviços no futuro.

## Referências 

- https://support.discord.com/hc/pt-br/articles/228383668-Usando-Webhooks
- https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04-pt
- https://www.hostinger.com.br/tutoriais/como-instalar-editor-de-texto-nano/
