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


![image](https://github.com/user-attachments/assets/a957fdd2-c468-4a6c-8f42-0f6055e1f8a4)


---

### ⚙️ **2. Criando o arquivo de senha e Alterando o arquivo .conf **

Arquivo: `mosquitto.conf`

* Criando o arquivo de senha
![image](https://github.com/user-attachments/assets/668663e3-2602-4912-b8a8-0342662ed447)
* Arquivo de senha criado
![image](https://github.com/user-attachments/assets/9e01713b-5a28-4c18-b49f-84af3f2d2cda)
* Edita o arquivo mosquitto.conf 
![image](https://github.com/user-attachments/assets/a94c0051-3758-46ae-9b69-4e0c594f565e)
* Verificando se deu certo
![image](https://github.com/user-attachments/assets/b64f5dd9-c026-4651-be9b-82bdce610b55)

---

### ⚙️ **3. Escutando o tópico com Mosquitto **

* Publisher. Terminal adicional aberto com o comando:
![Captura de tela 2025-05-26 190327333333](https://github.com/user-attachments/assets/5780ce92-89fd-4f27-91f8-ff92460e555d)
* Subscriber.  Mostra mensagens publicadas pelo ...:
![Captura de tela 2025-05-26 190327222](https://github.com/user-attachments/assets/7c51ac63-1220-4398-be1d-7730ee1d0e72)


---

### ⚙️ **4.  Simulando criptografia leve (XOR)**



---

### 📦 **3. Conexão ... com o Wi-Fi**

* O ESP32 foi configurado com SSID e senha locais.
* Após conexão, ele inicia o cliente MQTT automaticamente.

---


###  Questionamentos

* Quais técnicas são escaláveis?

* Como aplicá-las com várias BitDogLab em rede escolar?


---

### 📝 Conclusão

A integração ... + MQTT + Mosquitto foi realizada com sucesso. O ... é capaz de se conectar a um broker local e publicar dados, que são recebidos pelo PC....

---

## 📜 Licença
GNU GPL-3.0.
