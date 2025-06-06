[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/G8V_0Zaq)

```markdown
# Tarefa: IoT Security Lab - EmbarcaTech 2025
**Autor**: Filipe Alves de Sousa e Giovana Ferreira Santos
**Curso**: Residência Tecnológica em Sistemas Embarcados
**Instituição**: EmbarcaTech - HBr
**Brasília**, Maio de 2025

---

## 📋 README — Autenticação, Criptografia e Proteção contra Ataques em Comunicação MQTT com a BitDogLab

### 📌 Objetivo

Este projeto demonstra a comunicação segura entre uma **placa BitDogLab (com Raspberry Pi Pico W)** e um **broker MQTT (Mosquitto)**. A BitDogLab atua como um dispositivo IoT, enviando dados (como temperatura) para um tópico MQTT. O Mosquitto, rodando em um computador, recebe e exibe essas mensagens, enquanto exploramos diversas camadas de segurança.

---

```

tarefa-iot-security-lab-gihandfih/
├── CMakeLists.txt              # Script de build para compilar o projeto (Pico SDK)
├── iot_security_lab.c          # Arquivo principal da sua aplicação (onde a lógica principal estará)
├── lwipopts.h                  # Configurações para a biblioteca de rede lwIP
├── mosquitto.conf              # Arquivo de configuração para o broker MQTT Mosquitto
├── include/                    # Pasta para arquivos de cabeçalho (.h)
│   ├── mqtt_comm.h             # Declarações para funções de comunicação MQTT
│   ├── wifi_conn.h             # Declarações para funções de conexão Wi-Fi
│   └── xor_cipher.h            # Declarações para funções de criptografia XOR
└── src/                        # Pasta para arquivos de código-fonte (.c)
    ├── mqtt_comm.c             # Implementação das funções de comunicação MQTT
    ├── wifi_conn.c             # Implementação das funções de conexão Wi-Fi
    └── xor_cipher.c            # Implementação das funções de criptografia XOR

````

---

## ✅ Etapas do Projeto

### 1. Conectando a BitDogLab ao Wi-Fi

Nesta etapa inicial, focamos em preparar o ambiente de desenvolvimento e garantir a conectividade da **BitDogLab com Raspberry Pi Pico W** à rede Wi-Fi.

**Compilação e Carregamento do Firmware:**

* **Compilando o projeto:** Utilizamos o CMake e o Pico SDK no VSCode para compilar o projeto.

    ![Image](https://github.com/user-attachments/assets/f6b734ae-739d-4e8f-81dc-865b356f02ed)

    *Descrição da imagem: Captura de tela do terminal do VSCode mostrando o processo de compilação do projeto com CMake e o Pico SDK, indicando que a compilação foi concluída sem erros.*

    Como podemos ver na imagem, a compilação ocorreu sem erros.

* **Carregando o firmware:** Em seguida, carregamos o firmware compilado na placa BitDogLab com Raspberry Pi Pico W.

    ![Image](https://github.com/user-attachments/assets/b67d613c-474d-4892-9af8-7c42ccd3651b)

    *Descrição da imagem: Foto de uma placa BitDogLab conectada a um computador, com a indicação de que o firmware está sendo carregado, e uma mensagem confirmando a conexão Wi-Fi estabelecida e o IP do broker revelado.*

    Após o carregamento, a conexão Wi-Fi foi estabelecida com sucesso e o endereço IP do broker foi revelado, confirmando que a placa está pronta para se comunicar.

* **Código de Conexão Wi-Fi:** O trecho de código abaixo exemplifica a função utilizada para conectar a BitDogLab à rede Wi-Fi:

    ```c
    // Conecta à rede WiFi
    // Parâmetros: Nome da rede (SSID) e senha
    connect_to_wifi("gihandfihwireless", "snickers27");
    ```

---

### 🧩️ 2. Setup MQTT Básico para IoT

Aqui configuramos a BitDogLab para se conectar ao broker MQTT e publicar dados em tópicos específicos.

![Image](https://github.com/user-attachments/assets/7b4cbf47-8f42-470f-8515-ae978a6fd23e)

**Configuração do Cliente MQTT:**

O código a seguir demonstra como configurar o cliente MQTT na BitDogLab:

```c
// Conecta à rede WiFi
// Parâmetros: Nome da rede (SSID) e senha
connect_to_wifi("gihandfihwireless", "snickers27");

// Configura o cliente MQTT
// Parâmetros: ID do cliente, IP do broker, usuário, senha
mqtt_setup("bitdog1", "192.168.152.222", "aluno", "senha123");
````

-----

**Preparação do Ambiente: Instalação e Configuração do Broker Mosquitto (PC)**

Para que a comunicação MQTT funcione, precisamos de um broker. O Mosquitto é uma excelente opção para isso.

  * **Download:** Baixe o Mosquitto em: [https://mosquitto.org/download/](https://mosquitto.org/download/)

  * **Instalação:** Siga as opções padrão de instalação no Windows.

  * **Verificação:** Para iniciar o Mosquitto e verificar se está funcionando, use um dos seguintes comandos no terminal:

    ```bash
    mosquitto -v
    ```

    Ou, se preferir especificar o arquivo de configuração:

    ```bash
    cd "C:\Program Files\mosquitto" # Navegue até a pasta do Mosquitto
    .\mosquitto.exe -c "C:\Program Files\mosquitto\mosquitto.conf" -v # Execute esse comando
    ```

    A saída esperada no terminal deve ser similar a:

    ![image](https://github.com/user-attachments/assets/a957fdd2-c468-4a6c-8f42-0f6055e1f8a4)

    *Captura de tela do terminal do Windows exibindo a saída do comando `mosquitto -v`, mostrando que o broker Mosquitto foi iniciado com sucesso e está escutando na porta padrão.*

-----

**Testando a Publicação e Inscrição MQTT:**

Para demonstrar a comunicação, abriremos dois terminais: um para o **Publisher** (quem envia a mensagem) e outro para o **Subscriber** (quem recebe a mensagem).

  * **Publisher:** Abra um terminal adicional e utilize o seguinte comando para publicar uma mensagem:

    *Captura de tela de um terminal mostrando o comando `mosquitto_pub` sendo executado para publicar uma mensagem "26.5" no tópico "escola/sala1/temperatura" com autenticação de usuário e senha.*

    ```bash
    mosquitto_pub -h localhost -p 1883 -t "escola/sala1/temperatura" -u "aluno" -P "senha123" -m "26.5"
    ```

  * **Subscriber:** Em outro terminal, use este comando para se inscrever no tópico e visualizar as mensagens publicadas:

    *Captura de tela de um terminal mostrando o comando `mosquitto_sub` sendo executado para se inscrever no tópico "escola/sala1/temperatura" com autenticação de usuário e senha, aguardando e exibindo mensagens recebidas.*

    ```bash
    mosquitto_sub -h localhost -p 1883 -t "escola/sala1/temperatura" -u "aluno" -P "senha123"
    ```

    ![Image](https://github.com/user-attachments/assets/cab2015c-471e-4c0e-a8a9-3e68b01462fc)

-----

### ⚙️ 3. Publicação MQTT sem Segurança

Nesta seção, realizamos uma publicação de mensagem em texto claro via MQTT, sem qualquer tipo de criptografia.

**Publicação e Recebimento do Pacote MQTT:**

Antes do envio da mensagem (publicação), o broker Mosquitto foi inicializado com o comando 'mosquitto -v' via terminal.

*Captura de tela do terminal mostrando o broker Mosquitto iniciado e conectado, indicando a porta de escuta e o registro de eventos de conexão e mensagens.*

	![image](https://github.com/user-attachments/assets/374f589a-e3b5-4afd-91f4-c66639e73fe9)

O terminal do subscriber mostra os valores recebidos sem criptografia. A saída serial da **BitDogLab** confirma a publicação realizada. Além disso, o **Wireshark** foi utilizado para capturar e analisar os pacotes MQTT, exibindo o timestamp, os endereços de origem e destino, o protocolo e o tópico MQTT ("escola/sala1/temperatura").

  * **Publisher:** Um terminal adicional é aberto para publicar a mensagem:

    ```bash
    mosquitto_pub -h localhost -p 1883 -t "escola/sala1/temperatura" -u "aluno" -P "senha123" -m "Ola Giovana"
    ```

  * **Subscriber:** Outro terminal mostra as mensagens publicadas:

    ```bash
    mosquitto_sub -h localhost -p 1883 -t "escola/sala1/temperatura" -u "aluno" -P "senha123"
    ```

Para o teste, três janelas de terminal foram abertas simultaneamente:

	
*Uma imagem composta de três seções: a parte superior exibe a interface do Wireshark com pacotes MQTT capturados e filtrados, mostrando detalhes do pacote selecionado. A parte inferior esquerda mostra o terminal da BitDogLab publicando dados. A parte inferior direita exibe o terminal do `mosquitto_sub` recebendo os dados publicados.*

	![image](https://github.com/user-attachments/assets/c1e90e3a-08c8-4231-bc43-6e9cb6d212b3)

Nesta figura, observamos:

  * O **Wireshark** com um filtro aplicado, mostrando apenas pacotes MQTT.
  * O pacote MQTT selecionado, com seu conteúdo detalhado na aba "Packet Details".
  * A tela do terminal da **BitDogLab** publicando os dados.
  * O terminal do `mosquitto_sub` recebendo os dados.

O código-fonte para a publicação da mensagem sem criptografia é o seguinte:

```c
// Mensagem original enviada
const char *mensagem = "Ola Giovana";

// Publica a mensagem original (não criptografada)
mqtt_comm_publish("escola/sala1/temperatura", mensagem, strlen(mensagem));
```

-----

### ⚙️ 4. Autenticação Básica no Mosquitto: Publicação MQTT com Segurança

Nesta etapa, adicionamos uma camada de segurança ao broker Mosquitto, implementando autenticação simples via usuário e senha. Também configuramos o cliente MQTT na **BitDogLab** para se autenticar ao conectar.

**Criando o Arquivo de Senha "passwd" e Alterando o "mosquitto.conf":**

1.  **Criando o arquivo de senha:** Utilizamos a ferramenta `mosquitto_passwd` para criar o arquivo de senha.

    * Criando o arquivo de senha
    ![image](https://github.com/user-attachments/assets/668663e3-2602-4912-b8a8-0342662ed447)
    
    *Captura de tela do terminal mostrando o comando `mosquitto_passwd -c passwd aluno` sendo executado para criar um arquivo de senha chamado `passwd` e adicionar um usuário "aluno" com uma senha.*

    O arquivo `passwd` foi criado com sucesso.
    
    * Arquivo de senha criado
    ![image](https://github.com/user-attachments/assets/9e01713b-5a28-4c18-b49f-84af3f2d2cda)
    
    *Captura de tela mostrando o conteúdo do arquivo `passwd` recém-criado, com o nome de usuário "aluno" e sua senha criptografada.*

2.  **Editando o arquivo `mosquitto.conf`:** Abra o arquivo `mosquitto.conf` em um editor de texto (VS Code ou Notepad++) e adicione (ou descomente) as seguintes linhas no final:

    ```bash
    allow_anonymous false
    password_file "C:\Program Files\mosquitto\passwd"
    listener 1883
    ```

    * Edita o arquivo mosquitto.conf 
    ![image](https://github.com/user-attachments/assets/a94c0051-3758-46ae-9b69-4e0c594f565e)
    
    *Captura de tela de um editor de texto exibindo o arquivo `mosquitto.conf` com as linhas `allow_anonymous false`, `password_file "C:\Program Files\mosquitto\passwd"` e `listener 1883` adicionadas ou descomentadas no final do arquivo, configurando a autenticação.*


    ![Image](https://github.com/user-attachments/assets/d261d3eb-0d7a-4c3b-98a6-03d9193261da)

3.  **Verificando a configuração:** Para ativar o serviço do broker com as novas configurações de autenticação, utilize o seguinte comando:

    ```bash
    mosquitto -c mosquitto.conf -v
    ```

     * Verificando se deu certo
    ![image](https://github.com/user-attachments/assets/b64f5dd9-c026-4651-be9b-82bdce610b55)

    *Captura de tela do terminal mostrando a saída do comando `mosquitto -c mosquitto.conf -v`, indicando que o broker Mosquitto foi iniciado com o novo arquivo de configuração, aplicando as regras de autenticação e exibindo o log detalhado.*

    Com essa configuração, o Mosquitto está rodando com autenticação ativada, garantindo uma comunicação MQTT mais segura.

-----

🛡️ **Verificando e Liberando a Porta 1883 no Firewall (Windows)**

É crucial garantir que a porta 1883, utilizada pelo MQTT, esteja liberada no firewall do Windows para permitir a comunicação.

  * **Verificar a porta:**

    ```bash
    netsh advfirewall firewall show rule name=all | findstr 1883
    ```
    
    ![Image](https://github.com/user-attachments/assets/ac61b286-c87f-489a-9e3f-90b259f1242d)


  * **Para liberar a porta no Firewall (Windows):**
    Siga estes passos:

    1.  Vá para **Painel de Controle \> Sistema e Segurança \> Firewall do Windows Defender \> Configurações Avançadas**.
    2.  Clique em **Regras de Entrada \> Nova Regra**.
    3.  Selecione **Porta** e clique em **Avançar**.
    4.  Escolha **TCP** e digite **1883**.
    5.  Marque **Permitir a conexão**.
    6.  Nomeie a regra como `mosquittomqtt1883` e clique em **Concluir**.

    ![Image](https://github.com/user-attachments/assets/786d07f8-62f5-47a3-bb1c-ffe4ff641432)
-----

**Teste de Autenticação no Mosquitto:**

Após configurar a autenticação e o firewall, podemos testar a comunicação. No terminal, continuamos a ver o valor recebido em texto plano (sem criptografia). O log do servidor MQTT (com o modo verbose ativado) nos fornece detalhes sobre a comunicação. Verificamos as mensagens publicadas e, no Wireshark, a captura dos pacotes MQTT, confirmando que a comunicação está ocorrendo sobre a porta 1883.


* Publisher. Terminal adicional aberto com o comando:
![Captura de tela 2025-05-26 190327333333](https://github.com/user-attachments/assets/5780ce92-89fd-4f27-91f8-ff92460e555d)

* Subscriber.  Mostra mensagens publicadas:
![Captura de tela 2025-05-26 190327222](https://github.com/user-attachments/assets/7c51ac63-1220-4398-be1d-7730ee1d0e72)

*Uma imagem dividida em duas seções; na parte superior, um terminal exibe o log do broker Mosquitto com o modo verbose ativado, mostrando atividades de conexão e mensagens. Na parte inferior, a interface do Wireshark exibe pacotes de rede capturados, com um filtro aplicado para MQTT, destacando informações sobre a comunicação na porta 1883.*




Neste ponto, foi confirmado que:

  * Apenas clientes autenticados conseguem publicar.
  * A porta 1883 foi liberada no firewall.
  * O teste de autenticação foi bem-sucedido.

-----

### 🛡️ 5. Simulando Criptografia Leve (XOR)

Nesta etapa, implementamos uma cifra XOR básica para ofuscar o conteúdo transmitido via MQTT. O objetivo é dificultar a leitura direta dos dados por ferramentas de sniffing, como o Wireshark, mesmo que não seja uma criptografia robusta para produção.

✅ A operação XOR é reversível: a mesma função é usada para cifrar e decifrar a mensagem.

**Implementação e Verificação:**

1.  **Criação do `publisher.c`:** Foi criado um arquivo `publisher.c` responsável por publicar a mensagem criptografada via MQTT.
    ✅ No terminal, observaremos os bytes ofuscados, confirmando que a criptografia XOR foi aplicada à mensagem.

2.  **Criação do `subscriber.c`:** Um arquivo `subscriber.c` foi criado para receber, decifrar e exibir a mensagem original.
    ✅ O `subscriber` aplica a mesma lógica XOR para decifrar a mensagem.
    ✅ O Wireshark confirmará que o payload da mensagem não está legível diretamente.

    ![Image](https://github.com/user-attachments/assets/483d484d-26fb-4e8d-8823-d596498271eb)

*Uma imagem composta de terminais e a interface do Wireshark. Um terminal mostra uma mensagem sendo cifrada e decifrada usando a operação XOR, com o resultado legível. Outro terminal exibe o log do broker MQTT. A interface do Wireshark captura os pacotes, mostrando o payload ofuscado pela criptografia XOR.*

Captura de tela de um terminal exibindo o log de atividades do broker Mosquitto, mostrando a conexão de clientes e o recebimento de mensagens, algumas das quais estão cifradas.*

A interface do Wireshark exibindo pacotes de rede capturados, com o filtro aplicado para MQTT. Um pacote específico está selecionado, e a área de detalhes do pacote revela o payload com bytes ofuscados, confirmando que a mensagem está cifrada.*

A imagem acima mostra a mensagem cifrada e decifrada utilizando XOR.

E nesta imagem, os pacotes no Wireshark, com o payload ofuscado, evidenciam a aplicação da criptografia XOR.

**Testes e Resultados Esperados:**

| Etapa                                 | Esperado                                                                        |
| :------------------------------------ | :------------------------------------------------------------------------------ |
| 📡 Publicação via `publisher.c`       | Payload no MQTT é ofuscado (não aparece como "26.5" no Wireshark)               |
| 🔑 Decifragem via `subscriber.c`      | Mensagem decifrada aparece corretamente como "26.5"                            |
| 🕵️‍♂️ Análise no Wireshark             | Payload com bytes "estranhos", não legíveis diretamente                       |

**Verificações Adicionais:**

  *  A mensagem publicada no MQTT está **ofuscada** (Wireshark não mostra texto claro).
  *  O Subscriber consegue **decifrar** a mensagem aplicando a mesma função XOR.
  *  A porta 1883 está desbloqueada no firewall.

-----

### 📦 6. Proteção contra Replay

Nesta etapa, demonstramos a proteção contra ataques de replay, utilizando timestamps para garantir a unicidade das mensagens.

Os terminais na parte superior da tela ilustram o cenário:

  * À esquerda, a mensagem recebida com um timestamp.
  * À direita, a `bitdoglab1` publicando mensagens.

Na parte inferior, temos a `bitdoglab2`, que atua tanto como publisher quanto como subscriber, recebendo as mensagens da outra placa e exibindo-as com seus respectivos timestamps. Isso permite verificar que cada mensagem é única e não uma retransmissão de dados antigos.

-----

### Questionamentos

  * Quais técnicas são escaláveis para um sistema IoT robusto?
  * Como podemos aplicar essas técnicas de segurança em uma rede escolar com várias BitDogLab?

-----

### 📝 Conclusão

A integração da **BitDogLab (com Raspberry Pi Pico W)** com o MQTT e o broker Mosquitto foi realizada com sucesso. A BitDogLab é capaz de se conectar a um broker local e publicar dados de forma eficiente. Exploramos e implementamos, de forma didática e com rigor científico, conceitos essenciais de segurança, como autenticação de cliente e criptografia leve (XOR), além de abordagens para proteção contra ataques de replay. Este projeto serve como uma base sólida para o desenvolvimento de sistemas IoT mais seguros e confiáveis.

-----

## Relato e Análise da Segurança e Escalabilidade em Redes IoT com BitDogLab e Raspberry Pi Pico W

O uso de dispositivos como o **BitDogLab**, integrados com a **Raspberry Pi Pico W**, possibilita a criação de uma infraestrutura escalável e segura para redes IoT. Abaixo, detalhamos técnicas essenciais para garantir a confiabilidade e a expansão da rede sem comprometer a segurança.

-----

### Técnicas Escaláveis

Para garantir que o sistema IoT se mantenha funcional mesmo com centenas de dispositivos conectados, é essencial adotar práticas seguras e eficientes:

  * **Autenticação Única no Broker MQTT**: Cada **BitDogLab** recebe uma identidade exclusiva durante a configuração do broker MQTT. Isso permite que apenas dispositivos autenticados participem da rede, reforçando a rastreabilidade e evitando conexões indesejadas.
  * **Uso de Tópicos MQTT Hierárquicos**: A organização da estrutura MQTT segue um padrão lógico, como `escola/sala/bancada/dispositivo`, permitindo o gerenciamento eficiente de múltiplos dispositivos e facilitando a escalabilidade da rede.
  * **Criptografia ChaCha20-Poly1305**: Embora a criptografia XOR tenha sido utilizada para demonstração, o algoritmo ChaCha20-Poly1305 é leve e eficiente, sendo ideal para dispositivos embarcados como a **Raspberry Pi Pico W**. Ele proporciona alta segurança mesmo em hardware com poder de processamento limitado, tornando-o uma escolha mais adequada para ambientes de produção.
  * **Proteção Anti-Replay com Timestamps**: Cada mensagem transmitida inclui um carimbo de tempo (timestamp), garantindo que ela seja única e evitando ataques de replay, sem a necessidade de armazenamento de estados complexos.

-----

### Aplicação em Rede Escolar

Para integrar várias **BitDogLab com Raspberry Pi Pico W** em um ambiente acadêmico, é necessário um planejamento cuidadoso da infraestrutura, gerenciamento e implementação.

#### Infraestrutura

  * **Servidores Locais**: Utilização de **BitDogLab** atuando como publishers e subscribers para otimizar a comunicação e reduzir a latência.
  * **Segmentação da Rede Wi-Fi**: Criação de diferentes níveis de acesso à rede Wi-Fi (básico, avançado, demonstrativo), garantindo isolamento de funções e segurança.
  * **Broker MQTT Central**: Um broker MQTT centralizado deve gerenciar as comunicações, aplicando **rate limiting** e controle de **Qualidade de Serviço (QoS)**, além de servir como uma Autoridade Certificadora para o provisionamento seguro de novos dispositivos.

#### Gerenciamento

  * **Dashboard Web**: Desenvolvimento de um dashboard web para monitoramento em tempo real de todos os dispositivos conectados, fornecendo uma visão abrangente da rede.
  * **Provisionamento Automático**: Implementação de um sistema de provisionamento automático de novos dispositivos, simplificando a expansão da rede.
  * **Rotação de Chaves Criptográficas**: Estabelecimento de uma política de rotação programada de chaves criptográficas, idealmente em momentos de menor uso, como finais de semana, para aumentar a segurança.
  * **Logs Centralizados**: Coleta e análise de logs de todos os dispositivos em um local centralizado para identificar anomalias e eventuais tentativas de ataque.

#### Implementação

A implementação deve seguir uma estratégia progressiva para garantir o sucesso e a estabilidade do sistema:

  * **Laboratório Piloto**: Iniciar com um laboratório piloto para validar a configuração inicial e testar as funcionalidades em pequena escala.
  * **Expansão Gradual**: Expandir a rede gradualmente, à medida que os dispositivos forem integrados e testados, garantindo que cada nova adição funcione conforme o esperado.
  * **Treinamento da Equipe**: Treinar a equipe técnica nos procedimentos de segurança e manutenção da rede, garantindo que eles estejam aptos a lidar com eventuais problemas.
  * **Documentação**: Documentar os problemas comuns e as soluções encontradas, criando uma base de conhecimento para garantir um fluxo operacional eficiente e rápido.

Com essa abordagem estruturada, é possível crescer de um pequeno número de dispositivos para centenas de unidades, mantendo a segurança, o controle e a facilidade de gerenciamento em uma rede IoT baseada em **BitDogLab e Raspberry Pi Pico W**.

-----

## 📜 Licença

GNU GPL-3.0.

```
```