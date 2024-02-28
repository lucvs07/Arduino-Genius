// C++ code
//
// Definir Constantes
#define ledVerde 2
#define ledAmarelo 3
#define ledAzul 4
#define ledVermelho 5
#define buzzer 7

#define btnLedVermelho 13
#define btnLedAzul 12
#define btnLedAmarelo 11
#define btnLedVerde 10


#define umSegundo 1000
#define meioSegundo 500
#define indefinido -1

#define tamanhoSequencia 4

//enumeração para definir os estados do jogo
enum estados {
  proxima_rodada,
  usuario_respondendo,
  jogo_sucesso,
  jogo_falha
};

int sequenciaLuzes[tamanhoSequencia];

int rodada = 0;

int ledsRespondidos = 0;

void setup()
{
  // conectar monitor Serial
  Serial.begin(9600);
  
  iniciarPortas();
  iniciarJogo();
  
}

void loop()
{
  switch(estadoAtual()){
    case proxima_rodada:
      Serial.println("Pronto para próxima rodada");
      prepararNovaRodada();
      break;
    case usuario_respondendo:
      Serial.println("Jogador Respondendo");
      processarRespostaUsuario();
      break;
    case jogo_sucesso:
      Serial.println("Jogo Finalizado com Sucesso");
      jogoSucesso();
      break;
    case jogo_falha:
      Serial.println("Jogo Finalizado com Falha");
      jogoFalha();
      break;
  };
  delay(meioSegundo);
}

//Função Iniciar Portar
void iniciarPortas(){
  // iniciar leds
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledAzul, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  
  // iniciar botões
  pinMode(btnLedVermelho, INPUT_PULLUP);
  pinMode(btnLedAzul, INPUT_PULLUP);
  pinMode(btnLedAmarelo, INPUT_PULLUP);
  pinMode(btnLedVerde, INPUT_PULLUP);
  
  //iniciar buzzer
  pinMode(buzzer, OUTPUT);
};

//função Inciar Jogo
void iniciarJogo(){
  int jogo = analogRead(0);
  randomSeed(jogo);
  
  for (int i = 0; i < tamanhoSequencia; i++){
    sequenciaLuzes[i] = sorteiaCor();
  };
};

//Funçao Piscar Led
int piscarLed(int portaLed){
  
  verificarSomDoLed(portaLed);
  
  digitalWrite(portaLed, HIGH);
  delay(500); // Wait for 1000 millisecond(s)
  digitalWrite(portaLed, LOW);
  delay(500); // Wait for 1000 millisecond(s)
  return portaLed;
};

// Função Checar Resposta do Jogador
int checarResposta(){
  if(digitalRead(btnLedVerde)== LOW){
  	return piscarLed(ledVerde);
  }
  if(digitalRead(btnLedAmarelo)== LOW){
  	return piscarLed(ledAmarelo);
  }
  if(digitalRead(btnLedAzul)== LOW){
  	return piscarLed(ledAzul);
  }
  if(digitalRead(btnLedVermelho)== LOW){
  	return piscarLed(ledVermelho);
  }
  return indefinido;
};

// Função sorteiaCor
int sorteiaCor() {
    return random(ledVerde, ledVermelho + 1);
}

//Função descobrir o estado atual
int estadoAtual(){
  if(rodada <= tamanhoSequencia){
    if(ledsRespondidos == rodada){
      return proxima_rodada;
    } else {
      return usuario_respondendo;
    }
  } else if(rodada == tamanhoSequencia + 1){
    return jogo_sucesso;
  }else{
    return jogo_falha;
  }
};

// Funçao preparar nova rodada
void prepararNovaRodada(){
  rodada++;
  ledsRespondidos = 0;
  if(rodada<tamanhoSequencia){
    tocarLedsRodada();
  };
};

//função tocar leds de uma rodada
void tocarLedsRodada(){
  for (int i = 0; i < rodada; i++) {
    piscarLed(sequenciaLuzes[i]);
  }
};

//função verificar a resposta do usuário
void processarRespostaUsuario(){
  int resposta = checarResposta();
  
  if (resposta == indefinido){
    return;
  };
  
  if(resposta == sequenciaLuzes[ledsRespondidos]){
    ledsRespondidos++;
  }else{
    rodada = tamanhoSequencia + 2;
  };
  
};

// função piscar leds para jogo finalizado com sucesso
void jogoSucesso(){
  tocarSom(600);
  digitalWrite(ledVerde, HIGH);
  digitalWrite(ledAmarelo, HIGH);
  digitalWrite(ledAzul, HIGH);
  digitalWrite(ledVermelho, HIGH);
  delay(meioSegundo);
};

// função piscar leds para jogo finalizado com falha
void jogoFalha(){
  tocarSom(300);
  digitalWrite(ledVerde, HIGH);
  digitalWrite(ledAmarelo, HIGH);
  digitalWrite(ledAzul, HIGH);
  digitalWrite(ledVermelho, HIGH);
  delay(umSegundo);
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledAzul, LOW);
  digitalWrite(ledVermelho, LOW);
  delay(meioSegundo);
};

//função Tocar Som
void tocarSom(int frequencia){
  tone(buzzer, frequencia, 100);
};

//função para verificar o som do Led
void verificarSomDoLed(int portaLed){
  switch(portaLed){
    case ledVerde:
      tocarSom(2000);
      break;
    case ledAmarelo:
      tocarSom(2200);
      break;
    case ledAzul:
      tocarSom(2400);
      break;
    case ledVermelho:
      tocarSom(2500);
      break;
  }
  
};
