# Potenciometro_Botao_LED
Controle de LED com Potenciômetro e Botão
# Controle de LED com Potenciômetro e Botão

Este projeto utiliza um Arduino para controlar um LED que combina duas funcionalidades:
1. **Controle de brilho** via potenciômetro.
2. **Liga/Desliga** do LED via botão.

## Funcionalidades
- **Botão**: Alterna entre ligar e desligar o LED.
- **Potenciômetro**: Ajusta o brilho do LED quando ele está ligado.

## Componentes Necessários
- 1 Arduino (Ex.: Uno, Mega)
- 1 LED
- 1 Resistor de 220Ω (para o LED)
- 1 Potenciômetro
- 1 Botão
- 1 Resistor de 10kΩ (para o botão - pull-down)
- Fios de conexão
- Protoboard

## Circuito

### Conexões:
1. **Potenciômetro**:
   - Terminal 1 -> GND
   - Terminal 2 -> Pino A0 (`potPin`)
   - Terminal 3 -> VCC
2. **Botão**:
   - Um terminal -> Pino 2 (`buttonPin`)
   - Outro terminal -> GND
   - Resistor de 10kΩ conectado entre o pino 2 e o GND (pull-down)
3. **LED**:
   - Ânodo (perna longa) -> Pino 6 (`ledPin`)
   - Cátodo (perna curta) -> Resistor de 220Ω -> GND

## Código

```cpp
// Definição dos pinos e variáveis
#define potPin 0     // Pino analógico para o potenciômetro
#define ledPin 6     // Pino PWM para o LED compartilhado
#define buttonPin 2  // Pino digital para o botão

int valPot = 0;      // Variável para armazenar o valor do potenciômetro
bool ledState = LOW; // Estado atual do LED (ligado ou desligado)

void setup() {
  pinMode(ledPin, OUTPUT);    // Configura o LED como saída
  pinMode(buttonPin, INPUT);  // Configura o botão como entrada
}

void loop() {
  // Verifica o estado do botão
  if (digitalRead(buttonPin) == HIGH) {   // Se o botão está pressionado
    ledState = !ledState;                 // Alterna o estado do LED
    delay(300);                           // Pequeno atraso para evitar "rebotes"
  }

  // Se o LED estiver ligado, controla o brilho pelo potenciômetro
  if (ledState) {
    valPot = analogRead(potPin);          // Lê o valor do potenciômetro
    valPot = map(valPot, 0, 1023, 0, 255); // Converte para escala de 0-255
    analogWrite(ledPin, valPot);          // Ajusta o brilho do LED
  } else {
    analogWrite(ledPin, 0);               // Desliga o LED
  }
}
