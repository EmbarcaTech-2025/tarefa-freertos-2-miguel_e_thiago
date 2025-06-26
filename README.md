# **Fly-Pong**

**Descrição**

Fly-Pong é um jogo de ping‑pong multiplayer desenvolvido para Raspberry Pi Pico W, embutido em uma BitDogLab. Duas unidades se comunicam via MQTT ( Mosquitto ), trocando o estado da bola em tempo real e permitindo que cada jogador controle sua raquete por botões físicos.

---

## 📋 Requisitos

- Raspberry Pi Pico W
- BitDogLab (LED Matrix 5×5, display OLED SSD1306, botões e buzzer)
- Broker MQTT (Mosquitto)
- Ambiente de desenvolvimento com Pico SDK (versão ≥ 2.1.1)
- FreeRTOS (configurado via FreeRTOSConfig.h)
- lwIP (configurado via lwipopts.h)

---

## ⚙️ Estrutura do Projeto

**src/**

- `main.c` — inicializa periféricos, tarefas FreeRTOS e coordena comunicação WiFi/MQTT
- `ball.c` — lógica de movimento, colisão e pontuação da bola
- `led_matrix.c` — driver para matriz de LEDs (5×5)
- `oled.c` & `ssd1306_i2c.c` — controle do display OLED SSD1306
- `mqtt_comm.c` — setup, subscribe e publish em tópicos MQTT
- `wifi_conn.c` — conexão WiFi via CYW43
- `button.c` — leitura e debounce de botões
- `buzzer.c` — sinaliza eventos sonoros
- `led_rgb.c` — LED RGB indicador de status
- `xor_cipher.c` — (opcional) cifra XOR de mensagens MQTT

**include/**

- `ball.h`, `button.h`, `comm.h`, `led_matrix.h`, `led_rgb.h`, `mqtt_comm.h`, `oled.h`, `wifi_conn.h`
- Outros headers:

  - `FreeRTOSConfig.h`
  - `lwipopts.h`
  - `ssd1306.h`, `ssd1306_font.h`, `ssd1306_i2c.h`

**pio/**

- `ws2818b.pio` — programa PIO para controle de LEDs

---

## ▶️ Como usar

1. Clone o repositório do FreeRTOS Kernel na pasta `lib/FreeRTOS-Kernel`:

   ```bash
   git clone https://github.com/FreeRTOS/FreeRTOS-Kernel lib/FreeRTOS-Kernel
   ```

2. Conecte sua Pico W à rede WiFi definindo `WIFI_SSID` e `WIFI_PASSWORD` em `main.c`.
3. Ajuste o endereço IP do broker MQTT (`IP`, `PORT`, `MOSQUITTO_USER`).
4. Compile com CMake e Pico SDK:

   ```bash
   mkdir build && cd build
   cmake ..
   make
   ```

5. Flash no Pico W:

   ```bash
   cp fly_pong.uf2 /media/<seu-usuario>/RPI-RP2
   ```

6. Abra dois jogadores (uma BitDogLab configurada como **fly**, outra como **pong**) e pressione os botões para controlar a raquete.

---

## 📹 Demonstração

\[[Vídeo demonstrando jogabilidade](https://youtu.be/hFv3upQNCCw)]

---

## 📝 Comentários e Melhorias Futuras

> **Espaço para observações pessoais:**
>
> - Facilitar a seleção de modo **fly** ou **pong** (incluir opção de seleção no início ou via menu serial).
> - Permitir configuração de credenciais WiFi por interface serial ao inicializar.
> - Fly aguardar que Pong esteja conectado antes de iniciar o jogo.
> - Implementar reconexão automática ao WiFi em caso de falha inicial.
> - Corrigir bugs de estabilidade (crashes inesperados e falhas de renderização).
> - Resolver casos em que a bola atravessa a raquete (colisões perdidas, bem raro).
> - Ajustar comportamento da bola ao bater nos cantos para evitar trajetórias incorretas, como uma linha reta vertical na parede.

---

Autores: _Miguel Carvalho e Thiago Carrijo_

Curso: Residência Tecnológica em Sistemas EmbarcadosMore actions

Instituição: EmbarcaTech - HBr

Brasília, 25 de Junho de 2025

---

**Licença**

Este projeto é distribuído sob a licença GNU GPL-3.0.
