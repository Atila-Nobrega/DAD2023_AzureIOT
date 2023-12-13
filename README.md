# Azure - IoT Hub (Parte 1)
O Azure IoT Hub é um serviço robusto e escalável da Microsoft projetado para simplificar a conectividade e a gestão de dispositivos na Internet of Things (IoT). Essa plataforma poderosa oferece uma variedade de recursos que facilitam o desenvolvimento, implantação e manutenção de soluções IoT inovadoras.

O IoT Hub funciona como um hub de mensagens bidirecional, permitindo a comunicação eficiente entre dispositivos distribuidos de IoT e a nuvem, se baseado em uma arquitetura de nuvem escalável da Azure, o serviço suporta protocolos padrão do setor, incluindo MQTT, HTTP e AMQP, proporcionando flexibilidade na integração com uma ampla variedade de dispositivos.

## Principais Recusos
1. Registro e Autenticação de Dispositivos: O serviço fornece um sistema seguro para registrar e autenticar dispositivos, garantindo que apenas dispositivos autorizados possam interagir com o IoT Hub. Essa autorização é feita por meio de Keys e será utilizado no projeto.
2. Comunicação Bidirecional: Permite a transmissão eficiente de mensagens entre dispositivos IoT e aplicativos na nuvem, possibilitando ações remotas e atualizações em tempo real. No exemplo do projeto, veremos como enviar uam mensagem por meio de um processo para a núvem e em seguida direcionar essa mensagem para outro sistema, que possui um banco PostgresSQL, assim salvando so resultados em tempo real.
4. Escalabilidade Automática: O IoT Hub escala automaticamente para lidar com grandes volumes de dispositivos, garantindo desempenho consistente mesmo em ambientes dinâmicos.

## Casos de uso:
1. Monitoramento Industrial: É ideal para monitorar e gerenciar uma variedade de sensores e dispositivos em ambientes industriais, permitindo a otimização de processos e a detecção precoce de falhas.
2. Saúde Conectada: Na área da saúde, o IoT Hub pode ser utilizado para conectar dispositivos médicos, monitorar pacientes remotamente e fornecer dados em tempo real para profissionais de saúde.
3. Cidades Inteligentes: Facilita a implementação de soluções para cidades inteligentes, permitindo a gestão eficiente de recursos, monitoramento do tráfego e melhorias na qualidade de vida dos cidadãos.
4. Agricultura Inteligente: Ppode ser aplicado na agricultura para monitorar condições climáticas, otimizar a irrigação e melhorar a eficiência na produção de alimentos.

O Azure IoT Hub, com sua flexibilidade e robustez, torna-se uma escolha estratégica para organizações que buscam aproveitar todo o potencial da Internet of Things, conectando dispositivos e transformando dados em insights acionáveis.

## Integrações no Azure
Além disso vale ressaltar que o IoT Hub possui conexão e com várias outros serviços do Azure, como por exemplo:
1. Azure Stream Analytics: Integrado ao IoT Hub, este recurso permite a análise em tempo real de grandes volumes de dados gerados pelos dispositivos, possibilitando a extração de insights valiosos.
2. Azure Functions: como funções serverless, podem ser acionadas por eventos do IoT Hub. Isso permite a execução de lógica personalizada em resposta a eventos específicos, sem a necessidade de gerenciar a infraestrutura.
3. Azure IoT Central: é uma solução SaaS que simplifica a criação e gerenciamento de soluções IoT. Ele pode ser conectado ao IoT Hub para facilitar a criação de aplicativos de IoT personalizados sem a necessidade de habilidades avançadas de programação.
4. Azure Machine Learning: pode ser usado em conjunto com o IoT Hub para treinar modelos de machine learning com dados provenientes de dispositivos IoT. Esses modelos podem ser implantados para realizar inferências em tempo real.

# O Projeto (Parte 2)
Visando monitorar a qualidade do ar em ambientes controlados, propomos um sistema que seja capaz de identificar a qualidade do ar, baseado nos gases presentes naquela atmosfera, bem como a temperatura e a umidade do local e para ambientes fechados também será possível reconhecer incêndios, permitindo a emissão de possíveis alertas para uma solicitação mais rápida de socorro. Tendo em vista que é muito mais difícil realizar um controle de qualidade em ambientes muito grandes, a ideia é, inicialmente, utilizar esse conceito para locais pequenos e fechados, como buffets, clubes,restaurantes e cinemas, gerando um ambiente confortável e saudável para seus visitantes, depois expandindo para clínicas, hospitais e, eventualmente, vários ambientes, diminuindo a exposição da população à ambientes nocivos à saúde.

O projeto propõe a implementação dos seguintes serviços:
+ Monitoramento dos componentes gasosos do ar
+ Monitoramento da umidade e temperatura do local
+ Acesso à notificações e alarmes para situações críticas (configuradas pelo usuário)
+ Sistema de detecção de incêndios para locais fechados (com ajuda integrada)

Dessa maneira será feita uma Coleta e envio de dados de sensores, envio desses dados para o serviço na núvem e encaminhamento dessas informações para um banco de dados SQL, quem em seguida poderia ser conectado a uma tecnologia de analiticas de dados para monitoramento.

## Materias utilizados para o dispositivo:
+ Sensor De Qualidade Do Ar Mq-135
+ Sensor Dht11 Sensor De Temperatura E Umidade
+ Buzina Buzzer Ativo 5v
+ Arduino
+ Cabo USB interface para o computador (Necessário somente pois o arduino não possui modulo WIFI)

## Fluxo Geral:

![1](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/d5f6149f-11d3-4b27-b997-5cfc4b4b7cca)

!!! Não Implementamos os envios de comandos para os dispositivos, porém esta implementação seguiria a mesma lógica de envio de dados.

## Fluxo dos Sensores:

![2](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/ec6498a4-3f78-4ee2-903d-d31c6432b652)

## Fluxo Sensores + IoT + Backend (publish/Subscriber):

![6](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/f7515338-1d4d-43e1-ae81-cd50ef89ac45)


## Resultados:
Segue algumas imagens demonstrando o funcioanmento:
+ Os dados são enviados por meio do protocolo MQTT para o Azure IoT hub. Aqui é o resultado de um encaminhamento da mensagem de um processo a outro por meio do IoT Hub, onde o processo a esquerda exemplifica o envio para a Nuvem e o o processo a direita mostra o retorno da mensagem. 

![13](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/054d66d3-75d0-471e-a579-2108b008641a)

+ O Azure IoT hub permite a criação de dashboards para apresentação dos dados recebidos pelos dispositivos conectados:
  
![12](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/39012edf-b8d8-44d9-adfc-d73dc04a8df5)

+ Contudo, é de interesse elaborar uma aplicação para apresentar os dados ao usuário, foi criado, com o uso da Ferramenta de BI open source Metabase, um conjunto de gráficos integrados a um banco de dados local do PostgreSQL. Assim, podemos enviar os dados do dispositivo para a nuvem e receber em outro computador, onde os dados serão recebidos e estarão prontos para serem visualizados no dashboard do metabase.

![11](https://github.com/Atila-Nobrega/DAD2023_AzureIOT/assets/49105715/819fbdce-8697-4eb8-b6b1-39ee79fb8877)
