# Arduino_ligar_e_desligar_luz

const int relePin = 8;       // Pino conectado ao módulo relé
const int botaoPin = 2;      // Pino conectado ao botão
bool estadoLuz = false;      // Estado da luz (desligado inicialmente)
bool ultimoEstadoBotao = HIGH; // Estado anterior do botão (com pull-up)

void setup() {
  pinMode(relePin, OUTPUT);
  pinMode(botaoPin, INPUT_PULLUP);  // Ativa resistor pull-up interno
  digitalWrite(relePin, LOW);       // Garante que a luz começa desligada
  Serial.begin(9600);
}

void loop() {
  bool estadoBotao = digitalRead(botaoPin);

  // Detecta borda de descida (botão pressionado)
  if (estadoBotao == LOW && ultimoEstadoBotao == HIGH) {
    estadoLuz = !estadoLuz; // Inverte o estado da luz
    digitalWrite(relePin, estadoLuz ? HIGH : LOW);
    Serial.println(estadoLuz ? "Luz Ligada" : "Luz Desligada");

    delay(300); // Debounce simples
  }

  ultimoEstadoBotao = estadoBotao;
}
