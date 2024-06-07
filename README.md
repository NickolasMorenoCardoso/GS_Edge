# GS_Edge

Monitoramento de Qualidade da Água usando o Arduino

Este projeto tem como principal objetivo, solucionar o problema que a "Economia Azul" passou para nós alunos da fiap, resolvermos. Nosso grupo teve a ideia de fazer um sistema de monitoramento de qualidade da água utilizando o site Tinkercad. Todos os detalhes do projeto estarão abaixo.

Este sistema utiliza algumas coisas como, um Arduino Uno, um sensor de temperatura LM35, um LCD 16x2 e três LEDS para monitorar a qualidade da água. O sistema lê o PH e a temperatura da água, então mostra os valores no LCD e usa os LEDS para indicar se a qualidade da água está dentro do aceitável.

Componentes Necessários:
1 x Arduino Uno
1 x LCD 16x2
1 x Potenciômetro (10kΩ)
1 x Sensor de Temperatura LM35
3 x LEDS (verde, amarelo e vermelho)
3 x Resistores (220Ω)
1 x Breadboard

Montagem do Sistema:

LCD 16x2:
VCC: Conecte ao 5V.
GND: Conecte ao GND.
VO: Conecte ao pino central do potenciômetro.
RS: Conecte ao pino digital 12 do Arduino.
RW: Conecte ao GND.
E: Conecte ao pino digital 11 do Arduino.
D4: Conecte ao pino digital 5 do Arduino.
D5: Conecte ao pino digital 4 do Arduino.
D6: Conecte ao pino digital 3 do Arduino.
D7: Conecte ao pino digital 2 do Arduino.
A: Conecte ao 5V através de um resistor de 220Ω.
K: Conecte ao GND.

Potenciômetro:
Terminal 1: Conecte ao 5V.
Terminal 2: Conecte ao pino VO do LCD.
Terminal 3: Conecte ao GND.
Sensor de Temperatura (LM35)
Pino 1: Conecte ao 5V.
Pino 2: Conecte ao pino A1 do Arduino.
Pino 3: Conecte ao GND.

LEDS:
LED Verde:
Anodo: Conecte ao pino digital 9 através de um resistor de 220Ω.
Catodo: Conecte ao GND.
LED Amarelo:
Anodo: Conecte ao pino digital 10 através de um resistor de 220Ω.
Catodo: Conecte ao GND.
LED Vermelho:
Anodo: Conecte ao pino digital 8 através de um resistor de 220Ω.
Catodo: Conecte ao GND.

Código:
Copie e cole o seguinte código no Arduino

#include <LiquidCrystal.h>

// Definições de pinos
const int pH_pin = A0;
const int temp_pin = A1;
const int ledVerde = 9;
const int ledAmarelo = 10;
const int ledVermelho = 8;

// Configurações do LCD
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  // Inicializa o LCD
  lcd.begin(16, 2);
  lcd.print("Monitoramento");
  delay(2000);
  lcd.clear();

  // Configura os pinos dos LEDs como saída
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);

  // Teste inicial dos LEDs
  digitalWrite(ledVerde, HIGH);
  delay(1000);
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, HIGH);
  delay(1000);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, HIGH);
  delay(1000);
  digitalWrite(ledVermelho, LOW);
}

void loop() {
  // Leitura dos sensores
  int pH_value = analogRead(pH_pin);
  int temp_value = analogRead(temp_pin);

  // Conversão dos valores para unidades adequadas
  // Simulação do pH: valor do potenciômetro de 0 a 1023 mapeado para 0 a 14
  float pH = map(pH_value, 0, 1023, 0, 14);
  // Conversão do valor de temperatura
  float temp = temp_value * (5.0 / 1023.0) * 100;

  // Exibe os valores no LCD
  lcd.setCursor(0, 0);
  lcd.print("pH:");
  lcd.print(pH);
  lcd.setCursor(8, 0);
  lcd.print("Temp:");
  lcd.print(temp);

  // Avaliação da qualidade da água
  if (pH >= 6.5 && pH <= 8.5 && temp >= 10 && temp <= 30) {
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
  } 
  else if ((pH >= 6.0 && pH < 6.5) || (pH > 8.5 && pH <= 9.0) || (temp >= 5 && temp < 10) || (temp > 30 && temp <= 35)) {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
  } 
  else {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
  }

  // Delay para atualização
  delay(2000);
}

Como Funciona?:
Leitura dos Sensores: O Arduino lê os valores de PH e a temperatura da água através dos pinos analógicos.
Exibição no LCD: Os valores de PH e a temperatura são exibidos no LCD.
Indicação com LEDs: Dependendo dos valores lidos, o sistema acende um dos três LEDs:
LED Verde: Qualidade da água dentro dos parâmetros aceitáveis.
LED Amarelo: Qualidade da água levemente fora dos parâmetros aceitáveis.
LED Vermelho: Qualidade da água fora dos parâmetros aceitáveis.
Ajuste e Calibração
Potenciômetro: Use o potenciômetro para ajustar o contraste do LCD até que o texto seja claramente visível.
Valores de pH e Temperatura: Ajuste os valores no código para refletir os limites desejados para a qualidade da água.
Teste e Solução de Problemas
LCD não mostra texto: Verifique a conexão do potenciômetro e certifique-se de que está conectado corretamente ao VO do LCD.
LEDs não acendem: Verifique se os LEDs estão corretamente conectados e se os resistores estão em série com os LEDs.
Sensor de temperatura não funciona: Verifique as conexões do LM35 e certifique-se de que está conectado ao 5V, GND e pino analógico A1.
