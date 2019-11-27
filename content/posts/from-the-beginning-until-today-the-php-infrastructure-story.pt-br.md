+++
title = "Desde o começo até os dias de hoje, a história do PHP e sua infraestrutura."
slug = "desde-o-começo-ate-os-dias-de-hoje-a-historia-do-php-e-sua-infraestrutura"
date = "2019-11-26"
author = "rtio"
cover = "infrastructure.jpg"
tags = ["php", "serverless"]
keywords = ["lambda", "php", "serverless", "aws", "infrastructure"]
description = "Nós podemos começar pensando, por que algumas linguagens de programação passam através das décadas e outras, não? Por que as principais linguagens de programação como o Fortran não se mantêm entre as 10 principais linguagens hoje em dia? Se você der uma olhada no índice TIOBE, poderá ver o top 10 dos anos 90 até 2019; a maioria deles não está presente hoje."
showFullContent = false
+++

Vou contar uma breve história de vários cenários de infraestrutura PHP e sua evolução nos últimos anos. Como foco principal, apresentarei alguns dos muitos cenários de infraestrutura passados na vida do PHP, desde servidores locais até nossa infraestrutura atual, como Kubernetes, e tecnologias sem servidor. Tudo baseado em muitas histórias ouvidas nos últimos anos.

# Pensando fora da caixa

Podemos começar a pensar por que algumas linguagens de programação passam por décadas e outras, não? Por que as principais linguagens de programação como Fortran ou COBOL não se mantêm entre as 10 principais linguagens hoje em dia? Se você der uma olhada no índice TIOBE, poderá ver o top 10 dos anos 90 até 2019; a maioria deles não está presente hoje. Mas, por exemplo, Java, continua presente, por quê? Este gráfico mostra as 10 principais linguagens de programação mais usadas nos últimos 20 anos:

![TIOBE index chart](/tiobe_index_last_20_years.png)

Acredito que uma dessas razões para essa mudança de tecnologia seja porque alguns idiomas não foram projetados para serem executados em nossos ambientes atuais. Depois de pensar muito, descobri que a infraestrutura diz quais idiomas mantêm o uso e quais se tornam obsoletos.

# Virtudes do PHP

Disse que vou começar a falar sobre PHP e seu cenário ao longo dos anos. O PHP foi criado nos anos 94 durante o início do desenvolvimento da web. Não vou me aprofundar nos detalhes técnicos sobre a criação do PHP. Ainda assim, preciso falar sobre servidores da Web e por que o PHP é tão bem projetado para eles.

Arquitetura *Shared-nothing* que costumava ser motivo de riso - hoje em dia é obviamente conhecida como uma vantagem significativa - o PHP pode ser dimensionado sem problemas.

Cada solicitação da web é executada em um único encadeamento PHP. Parece a princípio, como uma limitação. Como seu programa é executado em um contexto de servidor da web, temos uma abordagem natural de simultaneidade disponível: solicitações da web. Enrolando de forma assíncrona no servidor, fornecendo uma maneira de compartilhar / copiar e copiar nada para explorar o paralelismo. Na prática, isso é mais seguro e resistente a erros do que a abordagem de bloqueios e estado compartilhado que a maioria das outras linguagens de uso geral fornece.

# No ínicio

**Servidores locais** - Todo mundo sabe, o PHP teve seus dias sombrios, e eu adoro começar a falar sobre esse momento importante para a linguagem. A internet estava se tornando popular em paralelo com o crescimento do PHP lado a lado. O PHP originalmente significava Página Pessoal Pessoal.

Foi lançado pela primeira vez na década de 95 por Rasmus Lerdorf. Ele foi usado para projetos muito mais complicados do que os criadores esperavam, e sua reputação se revela como uma linguagem punk. Cada implantação foi feita manualmente e ninguém sabe qual caminho seguir. Terra de ninguém.

**XAMPP** - Não preciso perder tempo falando sobre este, que é apenas uma caixa de ferramentas que significa X (Server), A (Apache), M (MySQL / MariaDB), (P) Pearl e (P) PHP - feito para ser usado como um AMBIENTE DE DESENVOLVIMENTO rápido e de fácil configuração. Sei que você já o usou em ambientes de produção e que vergonha!

# Infância

**Servidores compartilhados** - No momento, começamos a usar o controle de versão, como final_version_3.zip, e usamos o S / FTP antigo, mas dourado, para implantar nossos sites. Afinal, não nos preocupávamos muito com segurança e desempenho, eles eram muito baratos e nossos chefes aprovavam.

**VPS** - Here we start thinking about deploy strategies, SSH, and security. At least, we had more control here.

# Juventude

**IaaS** - Infraestrutura como serviço. Em meados de 2010, tornando-se popular, o IaaS começa a fazer parte de nossas vidas. AWS, Azure, Google Cloud, esses gigantes fornecem muitos serviços para melhorar nosso dia, mas é claro, não são nada baratos.

**Docker** - Em palavras fracas, o Docker é uma ferramenta que envolve nosso aplicativo como uma máquina virtual, mas a principal diferença é que ele compartilha os recursos da máquina host. O Docker é executado com o mínimo de programas e recursos locais, exatamente o que precisa para suas propostas, como servidor da web e ambiente de desenvolvedor. Se você precisar de mais detalhes sobre o que o Docker significa, pode dar uma olhada [aqui](https://blog.usejournal.com/what-is-docker-in-simple-english-a24e8136b90b).

Com essa evolução, nossas necessidades crescem, por exemplo, começamos a fazer estratégias de implantações, reversões mais complexas e também precisamos de aplicativos mais resilientes. Vendo isso, as empresas lançam serviços como Kubernetes e AWS ECS para orquestrar nossos muitos serviços e cenários que temos. Insatisfeitos com isso, também precisamos de ferramentas de gerenciamento de configuração para manter nossas máquinas "reais" provisionadas automaticamente quando nosso aplicativo precisar. É muito difícil de manter, não é?

**Serverless** - Para tornar mais claro, vamos começar a falar sobre a função como serviço. Uma função Lambda pega um evento e faz o que quer que seja uma vez e depois retorna um resultado. Muito legal, né?

Então, o que é um evento? Pode ser uma solicitação HTTP, uma mensagem, um cron e muitas coisas na Amazon, e a nuvem do Google faz. Vamos manter nosso foco em solicitações HTTP. 

![Lambda infrastructure](/lambda.png)

Como pagamos por isso? Deixe-me mostrar um cálculo simples para você saber como vai cobrar. A pedido 300ms = $0.0000002 + (3 X $0.000000208). 

E quanto à escalabilidade? Como funciona? Como é escalado? Em uma explicação simples, podemos falar assim: quando um evento acontece, a Amazon pega seu código e o coloca em algo como um contêiner (na verdade, não é um contêiner, mas funciona da mesma forma), e eles o contêiner processam o evento e Ele morre. Cada execução é sem estado. Isso significa que cada contêiner lida com cada solicitação que possui, cria "escalabilidade infinita". Não mate seu banco de dados!

Existem alguns provedores desse tipo de serviço, como AWS (não suporta PHP pronto para uso), IBM OpenWhisk (na verdade, ele executa o código em contêineres), Azure (existe um tipo de suporte), Google Cloud (não suporte). A melhor recomendação é usar a AWS porque eles são os principais fornecedores no momento.

Em novembro do ano passado, a Amazon abriu sua API de código de tempo de execução porque não deseja manter outros idiomas no AWS Lambda. Eles abriram o sistema para outras pessoas "para adaptar" o Lambda para executar outros idiomas no AWS Lambda.

**Lambda layers** - que podemos criar disponibilidade para tempos de execução personalizados, como PHP. Provavelmente você não quer saber os detalhes técnicos sobre isso. Então, vamos começar a falar sobre **Bref**, a idéia por trás disso que cria tudo o que você precisa para executar o PHP no AWS Lambda e criar aplicativos sem servidor.

Existem três maneiras de executar o PHP no Lambda com Bref, como função do PHP, como aplicativo HTTP e como aplicativo de console. Com o primeiro, você pode executar uma função como o JavaScript e o Python. No segundo, você pode executar um aplicativo que recebe uma solicitação HTTP para fazer algum processo e retornar uma resposta. E o terceiro, você pode executar aplicativos de console, como o console Symfony ou os comandos do artesão do Laravel, porque o Lambda não tem ssh para acessar o aplicativo diretamente.               

**Cold starts** - Nem sempre uma cama de rosas. Durante a execução, a mesma função pode ser reutilizada várias vezes, mas após essas chamadas, ela morre e, na próxima solicitação, o contêiner deve ser iniciado e leva mais ou menos 300 ms apenas para iniciar o contêiner, e é o início a frio. Existem maneiras de "aquecer", não se preocupe.

Não pretendo cobrir todos os detalhes. Se você está interessado no Lambda, dê uma olhada neste [vídeo](https://www.youtube.com/watch?v=4xdFQfOJgYI), o criador do Bref explica isso muito melhor do que eu. Além disso, que um olhar sobre o Bref [website](https://bref.sh/).

# O que temos pela frente?

PHP ainda na vanguarda. Torna-se bastante claro acompanhar sua evolução. Todos os dias temos uma nova ferramenta, linguagem disponível para escolher, mas, do meu ponto de vista, escolher PHP para um novo projeto é uma coisa boa nos próximos anos.
 
Take care!