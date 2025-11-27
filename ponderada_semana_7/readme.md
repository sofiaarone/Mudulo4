### Projeto IoT - Medição de RSSI com ESP32

Este projeto realiza a coleta contínua da intensidade do sinal WiFi (RSSI em dBm) usando um ESP32, enviando os valores para a plataforma Adafruit IO via MQTT e exibindo esses dados em um gráfico em tempo real.

#### Funcionalidades

- Conexão do ESP32 a uma rede WiFi.

- Medição do RSSI (força do sinal WiFi) continuamente.

- Envio dos dados via MQTT para um feed no Adafruit IO.

- Dashboard online com gráfico Tempo × dBm.

- Testes em diferentes ambientes, incluindo simulação de gaiola de Faraday (elevador).

#### Demonstração

O experimento mostra:

- ESP32 conectado à rede do celular.

- Entrada no elevador e a queda do sinal registrada no dashboard.

- Saída do elevador e a reconexão e retorno do RSSI.

- Visualização do gráfico em tempo real na dashboard.