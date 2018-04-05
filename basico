#include <LiquidCrystal.h>                          //Biblioteca responsavel pelo funcionamento do display padrão 16x2.
#include <IRremote.h>                               //Biblioteca responsavel pela recepção e decodificação pelo sensor IR.

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);              //Definição dos pinos do arduino ligados ao display. obdecem a uma ordem prevista no arquivo da biblioteca do display.
#define setl 28
#define seth 30

int nivel_max1 = 20;                               //Definimos a variavel nivel maximo da caixa 1 como 20 (será pino 20 do arduino).
int nivel_min2 = 21;                               //Definimos a variavel nivel minimo da caixa 2 como 20 (será pino 20 do arduino).


float armazenavalor;  

int RECV_PIN = 17;                                  //Definimos a variavel a qual conectamos o receptor IR. Será no pino 17.
IRrecv irrecv(RECV_PIN);                           //Definimos a variavel que armazena o valor lido recebido.
decode_results results;

int eolico = A0;                                   //Definimos "eolico" como variavel que indica pino A0 (analog input A0).  
int v_eolico = 0;                                  //Definimos "v_eolico" como variavel que recebe o valor da leitura em A0.

int v_nivel_max1 = 0;                              //Definimos "v_nivel_max1" como variavel que recebe o valor da leitura da porta digital 20 (LOW ou HIGH).
int v_nivel_min2 = 0;                              //Definimos "v_nivel_max2" como variavel que recebe o valor da leitura da porta digital 21 (LOW ou HIGH).

int bomba = 43;                                    //Definimos a variavel bomba como pino 43 (o pino 43 se ligará ao pino do ULN2803 que aciona o relé da bomba.
int comutador = 45;                                //Definimos variavel comutador sendo pino 45 do arduino. Tal pino se liga ao pino do ULN2803 que aciona o relé comutação. 

int power = A1;                                    //Definimos a variavel relativa a porta analogica A1.
int v_power = 0;                                   //Definimos a variavel que recebe o valor lido da porta analogica A1.

int LA = 31;   //sala_de_estar                      //Atribuimos que o pino 31 do arduino se chamará LA.
int LB = 33;   //garagem                             //Atribuimos que o pino 33 do arduino se chamará LB.
int LC = 35;   //cozinha                               //Atribuimos que o pino 35 do arduino se chamará LC.
int LD = 37;   //quarto_1                                //Atribuimos que o pino 37 do arduino se chamará LD.
int LE = 39;   //quarto_2                                  //Atribuimos que o pino 39 do arduino se chamará LE.
int LF = 41;   //banheiro                                    //Atribuimos que o pino 41 do arduino se chamará LF.
int enter;


int a=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int b=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int c=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int d=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int e=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int f=0;                //Definimos uma variavel de controle. Ela será util mais abaixo quando criaremos um mecanismo de ON-OFF com apenas um botão do controle remoto. 
int k=0;

int a_eolico=100;       //"a_eolico" é um valor de ajuste para setarmos abaixo de que valor "o gerador eolico está parado). É comparado com leitura analogica do eolico mais abaixo.


void setup()                              //Função Setup onde definimos e iniciamos algumas aplicações
{
  pinMode(bomba, OUTPUT);                  //Configuramos o pino Bomba como output.
  pinMode(comutador, OUTPUT);              //Configuramos o pino Comutador como output.
  pinMode(nivel_max1, INPUT);              //Configuramos o pino nivel_max1 como input.
  pinMode(nivel_min2, INPUT);              //Configuramos o pino nivel_min2 como input.  
  pinMode(eolico, INPUT);                  //Configuramos o pino eolico como input.  
  pinMode(power, INPUT);                   //Configuramos o pino power como input.  
  
  irrecv.enableIRIn();                     // Inicializa o receptor IR (chamada da função na biblioteca). 
 
  lcd.clear();                             //limpa o display. Garante que o display não apresenta caracteres lixo.
  lcd.begin(16, 2);                        //"lcd.begin(coluna, linha)" - inicia o display como um 16x2.
  lcd.setCursor(1,1);                      //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.
  lcd.print("Iniciando...");               //Escreve "Iniciando..." apartir de onde posicionamos o cursor.
  
  digitalWrite(bomba, LOW);                //digitalWrite(bomba, LOW) - tal linha escreve no pino BOMBA (veja na definição vista a pouco acima) o valor baixo (LOW).     
  v_eolico = analogRead(eolico);           //Armazena em v_eolico o valor da leitura analogica da variavel "eolico". 
  v_power = analogRead(power);             //Armazena em v_power o valor da leitura analogica da variavel "power".
  
  delay(1000);                             //Espera 1 segunda (1000 milisegundos).
  lcd.clear();                             //Limpa LCD novamente.
}

void loop()
{
v_eolico = analogRead(eolico);                   //Armazena em v_eolico o valor da leitura analogica da variavel "eolico". 
v_power = analogRead(power);                     //Armazena em v_power o valor da leitura analogica da variavel "power".
v_nivel_max1 = digitalRead(nivel_max1);          //Armazena em v_nivel_max1 o valor da leitura digital da variavel "nivel_max1" (boia de nivel max da caixa 1).
v_nivel_min2 = digitalRead(nivel_min2);          //Armazena em v_nivel_min2 o valor da leitura digital da variavel "nivel_max2" (boia de nivel min da caixa 2).

if(v_eolico <= a_eolico)                         //Nesta linha de v_eolico for menor que o setpoint "a_eolico" significa que "eolico parou". Programa "entra" no IF. 
{
    digitalWrite(comutador, HIGH);               //digitalWrite(variavel, nivel) - nesta linha faremos o pino da variavel comutador ir para nivel alto (HIGH).
    lcd.setCursor(1,1);                          //seta cursos na coluna 1 e linha 1.
    lcd.print("Modo REDE       ");               //Escreve no display "modo REDE".
    
    if(enter==LOW)                               //"Enter" é uma variavel lida do controle. Se "enter" está em nivel baixo, o programa entra neste IF.
    {                                            //"Enter é uma variavel que aciona ou nao a escrita de v_power na linha 2 do display. 
    lcd.setCursor(1,2);                          //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.        
    lcd.print("V_power:        ");               //Escreve no display "V_power:".
    lcd.print(9,2);                              //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.  Neste caso coluna 9 e linha 2.
    lcd.print(v_power);                          //Escreve no display o valor contido na variavel v_power.
    }
    
}
else                                              //...se a condição não for verdadeira...
{
  digitalWrite(comutador, LOW);                  //o relé comutador fica em BAIXO (LOW), isto é, a energia utilizada continua a ser a do eolico/bateria.
  lcd.setCursor(1,1);                            //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.  Neste caso coluna 1 e linha 1.                 
  lcd.print("Modo EOLICO     ");                 //Escreve no display "modo EOLICO".
  
  if(enter==LOW)                                  //Novamente, se "enter" está em nivel baixo, o programa entra neste IF.
  {
  lcd.setCursor(1,2);                              //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.     
  lcd.print("V_bat:        ");                     //Escreve no display "V_bat:".
  lcd.print(9,2);                                  //"lcd.setCursor(coluna, linha) - posiciona o cursor para posterior escrita.  Neste caso coluna 9 e linha 2.
  lcd.print(v_power);                              //Escreve no display o valor contido na variavel v_power.
  }  
}

//----------------------------------------------------------------BLOCO NIVEL CAIXAS DAGUA---------------------------------------------------------

if((v_nivel_max1 == LOW)&&(v_nivel_min2 == HIGH))      //Condição que avalia estado das caixas d'água. Se nivel da caixa_1 está menor que o máximo e tem água acima de nivel_min2 em 2..
{
    digitalWrite(bomba, HIGH);                         //Bomba liga para fornecer água para caixa_1 sempre a manter esta completa.
    
    if(comutador == HIGH)                              //Esse IF permite adicionar o simbolo [B] na linha 1 do display.
    {
    lcd.setCursor(1,1);                                //Posiciona o cursor.
    lcd.print("Modo REDE    [B]");                     //Observando bem é exatamente a mesma frase vista no inicio da Void_loop() porém com a adição do [B], que indica "bomba ativa".
    }
    else                                               //"...se o comutador não está ativo..."
    {
    lcd.setCursor(1,1);                                //posiciona o cursor na coluna 1 e linha 1
    lcd.print("Modo EOLICO  [B]");                     //Escreve "modo Eolico" com a adição do termo [B] que indica bomba ligada. 
    }                                                  //Esse IF foi um artificio para sobrepor a escrita no display. garantindo que o sistema sempre vai mostrar o status de operação.
    
}
else
{
  digitalWrite(bomba, LOW);                            //Se IF acima não for satisfeito ou não for mais satisfeito, bomba desliga.
}

//------------------------------------------------------------FIM BLOCO NIVEL CAIXA DAGUA ----------------------------------------------------------------

//------------------------------------------------------------INICIO BLOCO DE LEITURA IR-----------------------------------------------------------------

if (irrecv.decode(&results))                            //Esta função dentro do IF é uma especie de interrupção que aguarda um código chegar ao receptor IR. 
  {  
    Serial.print("Valor lido: ");                        //Escreve "valor lido"
    Serial.println(results.value, HEX);                  //Converte resultado da leitura para hexadecimal. Podemos mudar para DEC, caso preferir trabalhar com decimal.
    armazenavalor = (results.value);                      //Guarda em armazenavalor a ultima leitura obtida.
    
    if ((armazenavalor == 2450478335)&&(k==0))                   //Verifica se a tecla foi acionada com tal valor foi acionada. K é uma variavel de controle. 
    {                                                            //K oscila entre 1 e 0. Assim, quando vc pressiona uma vez tal botão K recebe 1. Apertando novamente, recebe 0.
      digitalWrite(enter, HIGH);                                 //Acende o led 
      k=1;                                                       //Observe então que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);                                                //Intervalo pequeno de tempo para reduzir o fenômeno de multiplos cliques (BOUNCE).
    } 
    if((armazenavalor == 2450478335)&&(k==1))                    //Como k agora vale 1 ao pressionar o botao do codigo em questao novamente ele já não entra mais no IF que faz "enter" HIGH.
    {                                                            //Só resta então entrar neste IF que faz enter = LOW. 
      digitalWrite(enter, LOW);                                  //Enter se torna LOW, baixo.
      k=0;                                                       //k recebe 0. Na proxima vez que clicar no botao correspondente o IF a ser verdadeiro será aquele que possui k==0.
      delay(500);                                                //Assim o sistema volta ao estado inicial.                                  
    }  
                                                                //OS BLOCOS A SEGUIR OBEDECEM A MESMA LÓGICA DESSE PROCESSO EXPLICADO ACIMA!
    
    if ((armazenavalor == 2640478335)&&(a==0))                 //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LA, HIGH);                                  //Acende o led A
      a=1;                                                     //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640478335)&&(a==1))
    {
      digitalWrite(LA, LOW);  //apaga o led
      a=0;
      delay(500);
    }  
   
   //----------------------------------------------
   
   if ((armazenavalor == 2640462015)&&(b==0))                 //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LB, HIGH);                                 //Acende o led B
      b=1;                                                    //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640462015)&&(b==1))
    {
      digitalWrite(LB, LOW);                                  //Apaga o led B
      b=0;
      delay(500);
    }  
    
    //----------------------------------------------
    
    if ((armazenavalor == 2640494655)&&(c==0))               //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LC, HIGH);                                //Acende o led C
      c=1;                                                  //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640494655)&&(c==1))
    {
      digitalWrite(LC, LOW);                                //Apaga o led C
      c=0;
      delay(500);
    }  
    
    //----------------------------------------------
    
    if ((armazenavalor == 2640453855)&&(d==0))                       //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LD, HIGH);                                        //Acende o led D
      d=1;                                                           //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640453855)&&(d==1))
    {
      digitalWrite(LD, LOW);                                         //Apaga o led D
      d=0;
      delay(500);
    }  
    
    
    //----------------------------------------------
    
    if ((armazenavalor == 2640486495)&&(e==0))                   //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LE, HIGH);                                   //Acende o led E
      e=1;                                                      //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640486495)&&(e==1))
    {
      digitalWrite(LE, LOW);                                    //Apaga o led E
      e=0;
      delay(500);
    } 
   
   //-----------------------------------------------
    
    if ((armazenavalor == 2640470175)&&(f==0))               //Verifica se a tecla foi acionada  
    {  
      digitalWrite(LF, HIGH);                                //Acende o led F
      f=1;                                                   //Observe que essa variável permite um funcionamento semelhante a um flipflop.
      delay(500);
    } 
    if((armazenavalor == 2640470175)&&(f==1))
    {
      digitalWrite(LF, LOW);                                 //Apaga o led F
      f=0;
      delay(500);
    }  
       
   
  irrecv.resume();                                           //Le o próximo valor  
}

//-------------------------------------------------------------------FIM BLOCO LEITURA IR--------------------------------------------------------------


//-------------------------------------------------------------------BLOCO INDICAÇÃO ILUMINACAO ATIVA-------------------------------------------------
if(enter == HIGH)                                      //A variavel enter vista lá em cima quando está em HIGH habilita a escrita na linha dois do status das lampadas....
{                                                      //...e inibe a escrita das tensões de eolico/bateria e rede eletrica. Porem o sistema continua a monitorar normalmente.
  if(LA == HIGH)                                       //"Se Led A está ativo..."
  {
    lcd.setCursor(1,2);                                //Posiciona o cursor na coluna 1 e linha 1.
    lcd.print("Luz: 1| | | | | ");                     //Observer tal impressão daqui para frente. Foi formatada pra subrepor-se uma a outra. 
  }                                                    //Assim, Cada IF tem um "lcd.print" que escreve sobre outro "lcd.print" de um IF que eventualmente esteja ativo.

  if(LB == HIGH)                                       //O resultado se somente LA e LB estão ativos é algo assim (ja que eles se sobrepoem): "Luz: 1|2| | | | ".
  {
    lcd.setCursor(1,2);                                //Posiciona o cursor na coluna 1 e linha 2.
    lcd.print("Luz:  |2| | | | ");
  }  
  
  if(LC == HIGH)
  {
    lcd.setCursor(1,2);                                 //Posiciona o cursor na coluna 1 e linha 2.
    lcd.print("Luz:  | |3| | | ");
  }  
  
  if(LD == HIGH)
  {
    lcd.setCursor(1,2);                                 //Posiciona o cursor na coluna 1 e linha 2.
    lcd.print("Luz:  | | |4| | ");
  }  
  
  if(LE == HIGH)
  {
    lcd.setCursor(1,2);                                 //Posiciona o cursor na coluna 1 e linha 2.
    lcd.print("Luz:  | | | |5| ");
  }  
  
  if(LF == HIGH)
  {
    lcd.setCursor(1,2);                                 //Posiciona o cursor na coluna 1 e linha 2.
    lcd.print("Luz:  | | | | |6");
  } 
} 


}
  
  
//Codigos dependem do controle remoto 
// *            =   2640500775
//  DNPP         =   2640476805
//  DIRECT       =   2640506895
//  SRC          =   2640496695
//  CIMA         =   2640455895
//  BAIXO        =   2640488535
//  ATT          =   2640472215
//  RETROCEDER   =   2640466095
//  AVANCAR      =   2640498735
//  PAUSE        =   2640474255
//  AM           =   2640457935
//  FM           =   2640490575
//  1            =   2640478335
//  2            =   2640462015
//  3            =   2640494655
//  4            =   2640453855
//  5            =   2640486495
//  6            =   2640470175
//  7            =   2640502815
//  8            =   2640449775
//  9            =   2640482415
