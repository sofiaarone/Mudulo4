### Vídeo de demonstração

[Assista ao vídeo](https://drive.google.com/file/d/1I2mIvDBV0WJUdCS0wWyRFL0ZWJbDMf7m/view?usp=sharing)

---

### 1. Introdução

Este experimento demonstra a medição contínua da potência do sinal Wi-Fi (RSSI, em dBm) utilizando um microcontrolador ESP32.
O dispositivo foi programado para:

- conectar-se à rede Wi-Fi do celular;

- medir a intensidade do sinal a cada segundo;

- enviar esses valores para a plataforma Adafruit IO via protocolo MQTT;

- exibir os dados em uma dashboard com gráfico em tempo real.

A demonstração inclui um teste prático no elevador, simulando um ambiente de gaiola de Faraday, no qual o sinal de rádio sofre forte atenuação.

---

### 2. Configuração do Sistema
#### 2.1. Hardware utilizado

- ESP32

- Notebook com Arduino IDE

- Hotspot móvel do celular da autora (iPhone)

- Conexão Wi-Fi 2.4 GHz

- Elevador do INTELI para simulação

#### 2.2. Software e serviços

- Arduino IDE (para programação e monitor serial)

- Biblioteca PubSubClient (MQTT)

- Plataforma Adafruit IO (dashboard + feed MQTT)

- Gráfico configurado para exibir dBm em tempo real

#### 2.3. Funcionamento do programa

O ESP32 realiza a seguinte sequência:

1. Conecta-se ao ponto de acesso Wi-Fi do celular.

2. Obtém o valor do RSSI através da função WiFi.RSSI().

3. Publica esse valor no feed MQTT configurado no Adafruit IO.

4. Repete essa medição a cada 1 segundo, permitindo análise contínua.

---

### 3. Descrição da Demonstração em Vídeo

A seguir, está a documentação completa e sequencial da demonstração registrada em vídeo.

#### 3.1. Conexão inicial ao Wi-Fi

No início do vídeo, o ESP32 já está ligado e a autora mostra no monitor serial que o dispositivo se conecta com sucesso ao hotspot do seu celular.

No monitor aparecem mensagens como:

```
WiFi conectado!
RSSI = -69
RSSI = -68
RSSI = -71
```


Isso demonstra que:

- o microcontrolador está ativo;

- a rede está acessível;

- as medições de intensidade de sinal estão funcionando.

#### 3.2. Caminhada até o elevador

Com o sistema rodando, ao caminhae para o elevador, o gráfico no notebook continua mostrando o RSSI em tempo real.

Durante esse trajeto, o gráfico apresenta pequenas oscilações naturais do sinal, típicas de ambientes internos.

#### 3.3. Entrada no elevador - Simulação de Gaiola de Faraday

Ao entrar no elevador, observa-se imediatamente:

- queda abrupta do RSSI;

- oscilações fortes;

- eventual desconexão do MQTT.

O monitor serial passa a exibir valores muito baixos, como:

```
RSSI = -85
RSSI = -92
RSSI = -100
```

E pode até parar de atualizar se a conexão for totalmente perdida.

Na dashboard da Adafruit IO, isso aparece como:

- queda brusca no gráfico;

- falha temporária na publicação dos dados;

- ausência de pontos durante o período dentro do elevador.

#### 3.4. Saída do elevador - Restauração do sinal

Ao sair do elevador, o sinal retorna quase imediatamente.

No monitor serial:

```
RSSI = -70
RSSI = -68
RSSI = -65
```

No gráfico do Adafruit IO:

- surge um ponto marcando o retorno do sinal,

- a linha volta ao comportamento normal,

- a continuidade da publicação MQTT é restabelecida.

Esse contraste comprova de forma clara a diferença entre um ambiente blindado e um ambiente aberto.

#### 3.5. Exibição da dashboard ao final

Após o teste, é mostrado o dashboard com:

- o gráfico contínuo tempo × dBm,

- o “buraco” no gráfico durante o período no elevador,

- a queda e retomada do sinal registradas com sucesso.

Essa visualização confirma que o experimento funcionou conforme esperado e que a plataforma registrou corretamente todos os eventos.

---

### 4. Código 

O código está disponível ao final do vídeo.

---

### 5. Conclusão

A demonstração realizada validou plenamente o sistema IoT desenvolvido.
O ESP32, aliado à plataforma Adafruit IO, foi capaz de monitorar dinamicamente a potência do sinal Wi-Fi e mostrar em tempo real como o ambiente interfere na propagação das ondas de rádio.

Dessa forma, o experimento do elevador registrou de forma clara e visual o comportamento da conexão MQTT sob interferência e a capacidade do sistema de retomar imediatamente o envio dos dados após a saída.