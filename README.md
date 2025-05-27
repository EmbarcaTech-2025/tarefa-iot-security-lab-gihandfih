[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/G8V_0Zaq)

# Tarefa: IoT Security Lab - EmbarcaTech 2025

Autor: **Filipe Alves de Sousa e Giovana Ferreira Santos**

Curso: Residência Tecnológica em Sistemas Embarcados

Instituição: EmbarcaTech - HBr

Brasília, Maio de 2025

---

## 📋 README — Autenticação, criptografia e proteção contra ataques em comunicação MQTT com a BitDogLab.

### 📌 Objetivo

Demonstrar a comunicação entre um **...** e um **broker MQTT (Mosquitto)**, onde o ... envia dados (ex: temperatura) para um tópico MQTT, e o computador com Mosquitto exibe as mensagens recebidas.

---

## ✅ Etapas do Projeto

---

### 🧩️ **1. Instalação do Broker Mosquitto (PC)**

* Baixado de: [https://mosquitto.org/download/](https://mosquitto.org/download/)
* Instalação no Windows com opções padrão.
* Verificado com o comando:

```bash
mosquitto -v
```

Saída esperada:

```
mosquitto version 2.x.x starting
...
mosquitto running
```

---

### ⚙️ **2. Código no ESP32 (usando LWIP MQTT)**

Arquivo: `mqtt_comm.c`

* Configura conexão com broker local (ex: IP `192.168.0.100`)
* Publica mensagens no tópico: `escola/sala1/temperatura`

Trecho principal do código:

```c
mqtt_client_connect(client, &broker_addr, 1883, mqtt_connection_cb, NULL, &ci);
mqtt_publish(client, "escola/sala1/temperatura", data, len, 0, 0, mqtt_pub_request_cb, NULL);
```

---

### 📦 **3. Conexão ESP32 com o Wi-Fi**

* O ESP32 foi configurado com SSID e senha locais.
* Após conexão, ele inicia o cliente MQTT automaticamente.

---

### 📡 **4. Broker Mosquitto rodando no PC**

* Iniciado com o comando:

```bash
mosquitto -v
```

* Mostra mensagens no console como:

```
New connection from ...
Received PUBLISH from ESP32...
```

---

### 🔍 **5. Escutando o tópico com Mosquitto**

Terminal adicional aberto com o comando:

```bash
mosquitto_sub -h 127.0.0.1 -t escola/sala1/temperatura
```

* Mostra mensagens publicadas pelo ...:

```
26.3
26.4
```

---

### 🔪 Testes

* Testado envio de dados numéricos (temperatura simulada)
* Mensagens recebidas corretamente no terminal Mosquitto

---

### 📝 Conclusão

A integração ... + MQTT + Mosquitto foi realizada com sucesso. O ... é capaz de se conectar a um broker local e publicar dados, que são recebidos pelo PC.

---

## 📜 Licença
GNU GPL-3.0.
