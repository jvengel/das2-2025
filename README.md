Aula dia 27/02

TradeOf

    Fazer Escolhas;
    Cada vez que for projetar um sistema voce precisa escolher em qual irá programar.
    
    Consiste em consistência, tamanho durabilidade, espaço, latência e performance.

        Consistência: O sistema ficar disponivel e consistente.

        Durabilidade: Gravar uma informação no database e garantir que essa informação não seja perdida. O que fazer quando as query não estão dando mais conta e você não tem mais opções? --> Pense em Cache para resolver se ja tentou de tudo. O cache é mais rápido. No cache não podemos manter a informação de dados(Incosistência, Sobrecarga da memoria). As vezes você vai abrir mão da durabildiade para ter performance.

        Espaço: Terceira forma normal --> Serve para reduzir redundancia. Usa uma quantidade minima para usar o banco.


        Exemplo de TradOf: (Ganho em uma coisa e ganho em outra):
        Site da univille lento -> precisa de mais computadores para não travar quando está cheio de gente e precisa ter mais computadores. Logo realizamos um TradeOf na AWS requisitando mais uma maquina EC2 com um alarme para Auto Scaling onde iremos ganhar consistencia, experiencia de usuario e em contrapartida perdemos dinheiro.


    Maquina que estava sendo usada pela AWS crashou:
        1. Poderia ter observalidade para detectar crashes e possiveis sobrecargas de sistema.
        2. Disparar um alarme
            2.1 Criar uma nova maquina para manter a consistencia(Primeiro bate no LoadBalancer e resolve isso.) 


    Tratar os recursos como descartavel.



Aula dia 06/03

IAC - Automatizar. E é a capacidade de provisionar e dar suporte à infraestrutura de computação usando código em vez de configurações e processos manuais.

Pesquisar sobre Tofu e ArdCD(eu acho)

Escrever um teplate usando terraform ou cloud formation para IAC

Servidor com nome = DANGER (Não descartavel)

Um Plano de dados é a máquina ser discartada.

O que é Acomplamento
Uma coisa que não consigo substituir com facilidade para outra. Exemplo: Qualquer dispositivo que tem uma porta HDMI, ele tem alto acomplamento ou baixo? Baixo pois podemos trocar facilmente,

AntiPatern: Basta cair o banco de dados que aplicação vai juntor, pois geralmente o banco tem um alto acomplamento com o serviço.

Best Practice: Para resolver o problema do antipatern, basta criar varias replicas e no meio colocar um Load Balancer para quando qualquer maquina que cair do banco tenha uma outra pronta. (Distribuição de carga) E se cair o LoadBalancer? Basta ter duplicação, triplicação (redundancia)

Frase do dia:
FAÇA DESIGN DE SERVIÇO NÃO DE SERVIDORES!


Aula 10/03/2025

Apenas um pouco da aula passada

Global Insfrascrutcture
AWS Regions;
Conjunto de um ou mais data-centeres
Availability Zones
Pode haver 5 data-centers mas eu não enxergo;
Sempre que tiver mais em uma zona sempre vai representar disponibilidade.
AWS Local Zones
Estrutura parecida com uma AZ(mas é menor) Argentinha Buenos Aires; Para que server: rodar servidor de fortnite para tu jogar(computação, storage ou no geral serviços gerais)
AWS Data Centers
AWS Points of presence(PoPs)
Edge location e Regional Edge Cache
Carregar site angular mais rapido, o ultimo.

Securing Access

Aula 13/03/2025

Modelo de responsabilidade compartilhada
Principio do privilegio minimo
Autenticação
Autorização
Identity and Access Management

Aula 17/03/2025

IAM Policies and permissions
Toda nuvem tem que ter um mecanismo de permissões e grupos. Há das polices:

Identity-based: Attach to an IAM user group OR role
Resource-based: Fica no recurso
Diferença: na de identidade você coloca a police no zezinho enquanto na de recurso você tem que especificar um usuario, mesmo que seja um "*" (todo mundo)

S3
Block of Storage
Data is stored on a device in fixed-sized blocks
File Storage
Data is stored in a hierarchical structure
File share = Centenas de maquinas trocando de arquivos atraves desses servidores
Object Storage
Data is sotred as objects based on attributes and metadata
Dados, apenas
Serviço mais famoso da AWS

Limite 5tb por objeto/arquivo

Aula 20/03/2025

S3 O que ele guarda a gente chama de objetos(metadados) Não pode ter dois bucket com o nome igual mesmo que na URI a definição da zona seja diferente. Você até pode criar uma pasta mas o S3 não guarda arquivos e sim objetos então ele se torna um prefixo.

Como utilizam o S3

Spikes in Demand
Host web content that needs bandwidth to address extreme spikes in demand
Static Site
Host a static site that consists of HTML files, images, and videos..
Financial Analysis
Store data that other service scan use for analysis
Disaster recovery
Support disaster recovery or data backup solutions
Store objects do S3 agora ele criptografa e quando voce pede para baixa ele descriptografa o arquivo para poder baixar. Para subir um arquivos podemos subir pelo AWS(MAXIMO DE 160GB pelo Console, A linha de comando é a forma mais simples de fazer uma copia de arquivo.)

aws s3 cp arquivoorigem s3://bucketdestino

Arquivo de 10GB ou 1 TB

Para subir precisa de uma internet muito boa;
Parelilizar o arquivo(Multiparts[Fatiar em arquivos de tamanho iguail]) Limite de 5tb um objeto do S3(>5gb obrigatorio separar(aws faz isso))
General Purpose
S3 STANDARD (Mais barato preço de download, mais disponibilidade, preço de armazenar é o mais caro de todos.)
Intelligent tiering
S3 Intelligent-Tiering() ElE CUIDA DE TUDO.
Infrequent Access
(Arquivo que conhecemos o padrão de acesso(Exemplo: Acessar 2 vezes no mes))

S3 Standard-IA(Armazenamento mais barato mas o download é mais caro)
S3 One Zone IA
Perde durabilidade mas é mais barato para guardar e mais caro que o standard para baixar
Archive
S3 GLACIER DEEP ARCHIVE
Modelo mais barato de Armazenamento

S3 GLACIER FLEXIBLE RETRIEVAL
Pode levar horas ou minutos para pegar um arquivo para baixa.Tem que pagar para pegar dnv. Mais caro que o de backup, para baixar pode aumentar

S3 Glacier instant Retrieval
Não posso esperar mintuos

S3 on outposts

7. Aula 24/03/2025


Configurando Amazon S3 Lifecycle Are a set of rules that define actions that Amazon s3 applies to a group of objects.

Transition actions transition to another storage class
Expiration action define when objects expire.
Set an S3 lifecycle police -> Data will automatically transfer to a different storage class without any changes to your application.

Prefixo: Exemplo: *.jpg(Aceitamos tudo que contenham a extensão .jpg)

Para o ciclo de vida, nós escolhemos quais rules definimos para o lifecycle do S3.

CORS: Uma proteção para o seus arquivos(Objects)

Configurando CORS no S3

[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST",
            "DELETE"
        ],
        "AllowedOrigins": [
            "http://127.0.0.1:5500"
        ],
        "ExposeHeaders": [
            "x-amz-server-side-encryption",
            "x-amz-request-id",
            "x-amz-id-2"
        ],
        "MaxAgeSeconds": 3000
    }
]

Segundo Bimestre
Aula 05/05 – Redes Básicas na Nuvem
VPC (Virtual Private Cloud)
Cria uma rede isolada dentro da AWS, onde você pode lançar recursos de forma segura. Permite total controle sobre IPs, sub-redes, rotas e segurança.

CIDR (Classless Inter-Domain Routing)
Define o bloco de endereços IP da VPC (ex: 10.0.0.0/16). Um número menor após a barra representa mais IPs disponíveis.

Subnet Pública
Sub-rede com acesso direto à internet através de um Internet Gateway. Utilizada para recursos que precisam estar acessíveis externamente, como servidores web.

Aula 12/05 – Laboratórios Práticos
Laboratórios Canvas
Guided Lab: Creating a Virtual Private Cloud ✔️ Nota: 50.4/56
Challenge Lab (Café): Creating a VPC Networking Environment for the Café ✔️ Nota: 45/56
Aula 15/05 – Reforço de VPC
Laboratórios Canvas (Revisão e prática)
Guided Lab: Creating a Virtual Private Cloud ✔️ Nota: 50.4/56
Challenge Lab (Café): Creating a VPC Networking Environment for the Café ✔️ Nota: 45/56
Aula 19/05 – Conectividade entre Redes
VPC Peering
Permite a comunicação entre duas VPCs, mesmo em contas ou regiões diferentes. Ideal para integrar diferentes ambientes de forma segura e privada.

AWS VPN Site-to-Site
Conexão criptografada entre a AWS e um data center local (on-premises) via internet pública.

AWS Direct Connect
Conexão física dedicada entre a AWS e o seu ambiente local. Garante menor latência e maior estabilidade em comparação com VPN.

Aula 26/05 – Identidade e Acesso
IAM Groups
Conjunto de usuários IAM com políticas de permissão aplicadas em grupo, facilitando o gerenciamento de acesso.

IAM Roles + AWS STS
Roles: Concedem permissões temporárias a usuários, serviços ou contas.
STS (Security Token Service): Gera credenciais temporárias seguras com validade limitada.
AWS Cognito
Serviço de gerenciamento de identidade para autenticação de usuários em aplicações web e mobile. Suporta login com redes sociais e provedores externos.

Aula 29/05 – Criptografia
Criptografia Simétrica
Usa a mesma chave para criptografar e descriptografar. Rápida, ideal para dados armazenados. Exemplo: AES.

Criptografia Assimétrica
Utiliza um par de chaves (pública e privada). Mais segura para troca de dados entre partes. Exemplo: RSA.

Aula 09/06 – Alta Disponibilidade
Laboratórios Canvas
Guided Lab: Creating a Highly Available Environment ✔️ Nota: 41.07/56
Challenge Lab (Café): Creating a Scalable and Highly Available Environment for the Café ✔️ Nota: 56/56
Aula 12/06 – Continuação da Alta Disponibilidade
Laboratórios Canvas
Guided Lab: Creating a Highly Available Environment ✔️ Nota: 41.07/56
Challenge Lab (Café): Creating a Scalable and Highly Available Environment for the Café ✔️ Nota: 56/56
Aula 16/06 – Balanceamento e Resolução de Nomes
Load Balancer
Distribui automaticamente o tráfego entre várias instâncias para garantir desempenho e tolerância a falhas. Tipos: ALB, NLB, GLB.

DNS (Domain Name System)
Resolve nomes de domínio em endereços IP. O serviço Amazon Route 53 fornece DNS escalável, seguro e com integração à AWS.

Aula 23/06 – Infraestrutura como Código (IaC)
IaC (Infrastructure as Code)
Permite definir e provisionar a infraestrutura via código, garantindo consistência, automação e versionamento.
Ferramentas populares:

AWS CloudFormation (nativo da AWS)
Terraform (multi-cloud)
