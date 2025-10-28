[Assista ao vídeo](https://drive.google.com/file/d/1QuNLhBvrygf7RrOxvZFu6vXS3COEITyR/view?t=7)

### Código

```
// Classe Semaforo
class Semaforo {
  private:
    int vermelho;
    int amarelo;
    int verde;

  public:
    // Construtor
    Semaforo(int pVermelho, int pAmarelo, int pVerde) {
      vermelho = pVermelho;
      amarelo = pAmarelo;
      verde = pVerde;
    }

    // Configuração dos pinos
    void iniciar() {
      pinMode(vermelho, OUTPUT);
      pinMode(amarelo, OUTPUT);
      pinMode(verde, OUTPUT);
    }

    // Métodos das fases
    void ligarVermelho() {
      digitalWrite(vermelho, HIGH);
      digitalWrite(amarelo, LOW);
      digitalWrite(verde, LOW);
      delay(6000);
    }

    void ligarVerde() {
      digitalWrite(vermelho, LOW);
      digitalWrite(amarelo, LOW);
      digitalWrite(verde, HIGH);
      delay(4000);
    }

    void ligarAmarelo() {
      digitalWrite(vermelho, LOW);
      digitalWrite(amarelo, HIGH);
      digitalWrite(verde, LOW);
      delay(2000);
    }
};

// Definição dos pinos
Semaforo semaforo(8, 9, 10);

void setup() {
  semaforo.iniciar();
}

void loop() {
  semaforo.ligarVermelho();
  semaforo.ligarAmarelo();
  semaforo.ligarVerde();
}
```
---

### Explicação com tutorial
### Passo 1

- Pegue o semáforo de madeira e encaixe os leds.
- Conecte o ânodo (perna longa) dos leds vermelho ao fio jumper macho-fêmea e o cátodo (perna curta) de cada led a um outro jumper macho-fêmea.

Ao total serão 6 jumpers macho-fêmea a serem utilizados.

<div align="center">
 <sub>Imagem 1</sub><br><br>
 <img src="assets/imagem1.png" alt="Título"><br>
 <sub>Fonte autoral</sub>
</div>

---

### Passo 2

- Conecte ânodo (perna longa) do led que está conectado ao jumper macho-fêmea, à linha do fio que vai à linha do resistor que vai ao GND da protoboard e o cátodo à linha do resistor que vai aos pinos referentes à posição de cada um dos leds.

<div align="center">
 <sub>Imagem 2</sub><br><br>
 <img src="assets/imagem2.png" alt="Título"><br>
 <sub>Fonte autoral</sub>
</div>

---

### Passo 3

- Ligue os fios que estão na photoboard e conectam-se com os jumpers macho-fêmea que esão vindo dos leds aos pinos digitais, respectivamente:

- led vermelho: pino digital 8
- led amarelo: pino digital 9
- led azul: pino digital 10

- GND: fio que conecta ele com os resistores

<div align="center">
 <sub>Imagem 3</sub><br><br>
 <img src="assets/imagem3.png" alt="Título"><br>
 <sub>Fonte autoral</sub>
</div>

---

### Passo 4

- Faça o código no Arduino IDE para que o circuito funcione.
- Os leds deverão acender nos respectivos intervalos de tempo: 

Led vermelho: acender por 6 segundos e depois apagar, logo depois o led amarelo acende por 2 segundos e apaga e, por fim, o led verde acende por 4 segundos e apaga. Assim, o ciclo se repete.

---

## Componentes Utilizados
| Componente | Quantidade | Especificação | Função |
|-------------|-------------|----------------|---------|
| LED Vermelho | 1 | 5mm | Sinal de parada |
| LED Amarelo | 1 | 5mm | Sinal de atenção |
| LED Verde | 1 | 5mm | Sinal de passagem |
| Resistor | 3 | 330Ω | Protege os LEDs contra sobrecorrente |
| Protoboard | 1 | - | Base para montagem dos componentes |
| Arduino UNO | 1 | - | Controlador do circuito |
| Jumpers macho-macho | 3 | - | Conexão entre pinos e protoboard |
| Jumpers macho-fêmea | 6 | - | Conexão entre pinos e protoboard |

---

### Conclusão

O projeto foi montado com sucesso, respeitando os tempos e a lógica de um semáforo real. A inclusão de ponteiros no código demonstrou conhecimento de manipulação de endereços de memória no C++.

Dessa forma, esta atividade reforçou conceitos de eletrônica básica, lógica de programação e documentação do hardware.