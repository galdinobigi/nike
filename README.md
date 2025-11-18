Enzo Galdino
Guilherme Bigi
Samuel Matias
Lucas Pereira
PROJETO DO LUSTRE 
// --- Definição dos Pinos ---
const int ledFixo = 13;      // LED 1: Permanece aceso (Pino 13 é um bom padrão)
const int ledPiscante = 10;  // LED 2: Pisca a cada 3 segundos
const int ledBotao = 9;      // LED 3: Controlado por botão
const int pinoBotao = 2;     // Pino digital para o botão (com resistor pull-down ou pull-up interno)

// --- Variáveis de Controle do LED Piscante ---
const long intervaloPisca = 3000;  // Intervalo de 3 segundos (3000 milissegundos)
unsigned long tempoAnterior = 0;   // Armazena o último momento em que o LED foi atualizado
int estadoLedPiscante = LOW;       // Estado atual do LED piscante

// --- Variáveis de Controle do Botão ---
int estadoBotaoAnterior = LOW;    // Armazena o estado anterior do botão (para detecção de borda)
int estadoLedBotao = LOW;         // Armazena o estado atual do LED de botão (liga/desliga)

void setup() {
  // Configura os pinos dos LEDs como SAÍDA
  pinMode(ledFixo, OUTPUT);
  pinMode(ledPiscante, OUTPUT);
  pinMode(ledBotao, OUTPUT);

  // Configura o pino do botão como ENTRADA com PULL-UP interno
  // Isso simplifica a fiação: o botão deve ser conectado entre o pino e o GND (terra)
  pinMode(pinoBotao, INPUT_PULLUP);
  
  // LED Fixo é ligado uma vez no setup
  digitalWrite(ledFixo, HIGH);
}

void loop() {
  // 1. Controle do LED Piscante (a cada 3 segundos)
  // Usa millis() para não bloquear a execução
  unsigned long tempoAtual = millis();

  if (tempoAtual - tempoAnterior >= intervaloPisca) {
    // Salva o novo tempo
    tempoAnterior = tempoAtual;

    // Inverte o estado do LED
    if (estadoLedPiscante == LOW) {
      estadoLedPiscante = HIGH;
    } else {
      estadoLedPiscante = LOW;
    }

    // Aplica o novo estado ao pino
    digitalWrite(ledPiscante, estadoLedPiscante);
  }

  // 2. Controle do LED com o Botão (Liga/Desliga com cada pressão)
  // Leitura do estado atual do botão
  // Como usamos PULL-UP, o estado é LOW quando o botão é pressionado
  int estadoBotaoAtual = digitalRead(pinoBotao);

  // Detecta a "borda de descida" (o momento em que o botão é pressionado)
  if (estadoBotaoAtual == LOW && estadoBotaoAnterior == HIGH) {
    // Atraso curto (debounce) para garantir uma única leitura
    delay(50); 
    
    // Inverte o estado do LED de botão
    estadoLedBotao = !estadoLedBotao; 

    // Aplica o novo estado
    digitalWrite(ledBotao, estadoLedBotao);
  }
  
  // Atualiza o estado anterior do botão para a próxima iteração
  estadoBotaoAnterior = estadoBotaoAtual;

  // 3. O LED Fixo (ledFixo) já está aceso e não precisa de controle no loop.
}
<img width="474" height="266" alt="image" src="https://github.com/user-attachments/assets/d65e7123-b645-4c92-a111-53f9abe437ed" />

