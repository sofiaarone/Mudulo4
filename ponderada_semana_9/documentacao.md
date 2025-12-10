## Visão Geral sobre Segurança em IoT (Atividade Ponderada em Sala)

Este documento consolida a análise de segurança e o teste de penetração (Pentest) realizado em um servidor web básico embarcado em um microcontrolador ESP32, conforme o roteiro proposto.

---

### 1. Metodologia e Solução Analisada

A solução analisada é um servidor web local implementado no ESP32, destinado a controlar periféricos (como um LED) através de uma interface HTTP simples.

* **Referência do Código Base:** O código é baseado no exemplo de servidor web local do ESP32 ([https://randomnerdtutorials.com/esp32-web-server-arduino-ide/]

* **Metodologia:** Análise Estática do código-fonte (identificação de vulnerabilidades) seguida pela Análise de Risco (probabilidade, impacto) e Análise Dinâmica (teste de ataque prático).

---

### 2. Análise Estática: Confirmação de Vulnerabilidades

O código utiliza o objeto `WiFiServer(80)` e a lógica de processamento manual de requisições dentro do `loop()`.

#### 2.1. Vulnerabilidades Confirmadas (Pontos Fracos)

| Vulnerabilidade | Localização no Código | Descrição Detalhada |
| :--- | :--- | :--- |
| **Falta de Controle de Acesso (Alto Risco)** | `WiFiServer server(80);` e ausência de lógica de autenticação. | **Confirmação:** Não há nenhuma verificação de usuário ou senha. Qualquer dispositivo conectado à mesma rede Wi-Fi pode acessar o IP do ESP32 e enviar requisições de controle (`/26/on`, `/27/off`, etc.) com total autoridade. |
| **Comunicação Não Criptografada (Médio Risco)** | `WiFiServer server(80);` | **Confirmação:** O uso da porta 80 e HTTP significa que todas as requisições (incluindo o caminho e o estado dos GPIOs) e a resposta HTML são transmitidas em texto simples, tornando-as vulneráveis a sniffing na rede local. |
| **Tratamento Inseguro de Conexão (Novo Ponto)** | `while (client.connected() && currentTime - previousTime <= timeoutTime)` | **Confirmação:** Embora haja um *timeout* (`timeoutTime = 2000ms`), o loop `while` que processa a requisição é bloqueante e pode ser facilmente sobrecarregado por múltiplas conexões concorrentes ou conexões lentas, contribuindo para o risco de DoS. |
| **Risco de XSS Inexistente (Reavaliação)** | `client.println(...)` | **Reavaliação:** O código não reflete nenhuma entrada não sanitizada do `header` (URL) de volta para o HTML, exceto as strings de estado (`output26State` e `output27State`), que são controladas internamente (`"on"` ou `"off"`). A vulnerabilidade de XSS Refletido é mitigada neste código específico. |

---

### 3. Refinamento dos Ataques e Análise de Risco

Devido à mitigação do XSS, o Ataque 2 será substituído por um ataque de risco similar, explorando a falta de controle de acesso.

#### Ataque 1: Negação de Serviço (DoS) por Flood de Requisições

| Categoria | Disponibilidade |
| :--- | :--- |
| **Vulnerabilidade Explorada** | Falta de Limitação de Taxa (Rate Limiting) e Processamento Bloqueante na função loop(). |

**Passo-a-Passo da Exploração (Confirmado)**

1.  **Reconhecimento:** O atacante descobre o IP do ESP32 (impresso via `Serial.println(WiFi.localIP());`).
2.  **Execução:** O atacante utiliza ferramentas de teste de estresse (ex: `ab`, `locust`) para iniciar centenas de requisições concorrentes.
3.  **Resultado:** O ESP32, com CPU limitada, fica preso no loop `while (client.connected() && currentTime - previousTime <= timeoutTime)` para vários clientes simultaneamente, exaurindo recursos e paralisando a aplicação (serviço parado).

**Análise de Risco**

| Fator | Avaliação | Justificativa |
| :--- | :--- | :--- |
| **Probabilidade** | **Alta** | Extremamente fácil de executar. Requer apenas saber o IP e o caminho da URL (que são previsíveis). |
| **Impacto** | **Médio** | Compromete a Integridade do sistema de controle, permitindo a manipulação não autorizada do estado físico do dispositivo. |
| **Risco Resultante** | **Médio-Alto** | **Justificativa:** A alta probabilidade de ocorrência (facilidade de execução) combinada com a capacidade de alterar o estado físico do dispositivo (Impacto Médio na Integridade) resulta em um risco que deve ser tratado com prioridade. |

---

#### **Ataque 2: Controle Não Autorizado (Unauthenticated Command Execution - UCE)**

Este ataque explora a falta de controle de acesso para realizar ações maliciosas, um risco mais direto do que o XSS neste código.

| Categoria | Integridade e Confidencialidade |
| :--- | :--- |
| **Vulnerabilidade Explorada** | Falta de Autenticação/Autorização (`CWE-306`). |

**Passo-a-Passo da Exploração**

1.  **Reconhecimento:** O atacante descobre o IP do ESP32 e os *endpoints* de controle (identificados pelas strings "GET /26/on" e "GET /27/off" no código).
2.  **Execução (Exploração Manual ou Script):** O atacante envia requisições HTTP maliciosas diretamente para os *endpoints* sem interação com a página HTML.
    * *Exemplo (usando `curl`):* O atacante desliga um sensor crítico: `curl http://192.168.1.100/27/off`
3.  **Resultado:** O ESP32 executa o comando porque a lógica de controle (`if (header.indexOf("GET /27/off") >= 0)`) não verifica a origem ou a identidade do cliente, permitindo a manipulação do estado físico do dispositivo (e.g., desativar um alarme, ligar um aquecedor).

**Análise de Risco**

| Fator | Avaliação | Justificativa |
| :--- | :--- | :--- |
| **Probabilidade** | **Alta** | Extremamente fácil de executar. Requer apenas saber o IP e o caminho da URL (que são previsíveis). |
| **Impacto** | **Médio** | Compromete a Integridade do sistema de controle, permitindo a manipulação não autorizada do estado físico do dispositivo. |
| **Risco Resultante** | **Médio-Alto** | A combinação de probabilidade alta com impacto moderado resulta em um risco significativo que exige mitigação imediata, como implementação de autenticação, uso de tokens ou restrição baseada em IP.

---

### 4. Tabela de Riscos

A tabela de risco é atualizada e reordenada.

| Ordem | Título Representativo do Ataque | Probabilidade | Impacto | Risco (P x I) |
| :---: | :--- | :---: | :---: | :---: |
| **1** | Negação de Serviço (DoS) por Flood de Requisições | Alta | Alto | **Alto** |
| **2** | Controle Não Autorizado (UCE) / Falta de Autenticação | Alta | Médio | **Médio-Alto** |

---

**Conclusão da Análise de Riscos**

A análise consolidada indica que o ataque de DoS representa o maior risco devido ao seu alto impacto na disponibilidade do sistema e pela facilidade com que pode ser executado contra servidores embarcados. Em seguida, o ataque de Controle Não Autorizado aparece com risco “Médio-Alto”, evidenciando vulnerabilidades sérias relacionadas à falta de autenticação e validação de comandos.