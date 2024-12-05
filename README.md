# Syst-me-domotique-complet-bas-sur-l-IoT
#include <LiquidCrystal.h> //bibliothèques requises
#include <Keypad.h>

int rs = 7 , e = 6 , d4 = 5 , d5 = 4 , d6 = 3 , d7 = 2 ;   // les broches Arduino auxquelles les broches LCD sont connectées
LiquidCrystal lcd ( rs, e, d4, d5, d6, d7 ) ; // initialisation de l'écran LCD

// initialisation du clavier
const byte ROWS  =  4 ; //quatre lignes
const byte COLS  =  4 ; //quatre colonnes
char hexaKeys [ ROWS ][ COLS ]  =  {   // boutons du clavier
  { '1' , '2' , '3' , 'Un' } ,
  { '4' , '5' , '6' , 'B' } ,
  { '7' , '8' , '9' , 'C' } ,
  { '*' , '0' , '#' , 'D' }
} ;
// les broches Arduino auxquelles les broches du clavier sont connectées
byte rowPins [ ROWS ]  =  { A5, A4, A3, 8 } ; // se connecte aux broches de ligne du clavier
byte colPins [ COLS ]  =  { 10 , 11 , 12 , 13 } ; // se connecter aux broches de colonne du clavier

Clavier customKeypad  = Clavier ( makeKeymap ( hexaKeys ) , rowPins, colPins, ROWS, COLS ) ; 

configuration vide ()  {
// initialisation de l'arduino
pinMode ( A0, ENTRÉE ) ;        //LDR
pinMode ( A1, INPUT ) ;        // Capteur de température
                                                                                                                    
pinMode ( 11 , SORTIE ) ;
pinMode ( 10 , SORTIE ) ;

écran lcd.début ( 16 ,2 ) ;


Serial.begin ( 9600 ) ; // débit en bauds de communication de 9600
}
//initialisation des variables
int temp = 0 ;
int lumière = 0 ;
int u = 0 ;
int uavg = 0 ;
int x ;
int broche = 9 ;
int i = 0 ;
int lightavg = 0 ;
int tempavg = 0 ;
int xreq = 70 ;
Corde temporaire ;
int xréf ;

boucle vide ()  {
i = i+1 ;
temp = analogRead ( A1 ) ;   //Capteur de température connecté à la broche A1
light = analogRead ( A0 ) ; //Circuit diviseur de tension LDR connecté à la broche A0

température =( température/2,05 ) ;


écran LCD.setCursor ( 0 ,1 ) ;
lcd.print ( "Lumière=" ) ;
lcd.print ( clair ) ;
x = lumière ;

char customKey  = customKeypad.getKey () ; // obtention de la valeur du clavier enfoncé
if  ( customKey == 'D' ){ // bouton "D"       du clavier initialisé comme bouton Entrée
  xref = temporaire.toInt () ;
  temporaire = "" ;
  }
if  ( customKey == '1'  ||  customKey == '2'  ||  customKey == ' 3 ' ||  customKey == '4' || customKey == '5' || customKey == '6' || customKey == '7' || customKey == '8' || customKey == '9' || customKey == '0' ){    //si la saisie au clavier est un nombre , alors l'enregistrer dans un fichier temporaire et si "D" c'est-à-dire entrer n'est pas enfoncé, alors fusionner la nouvelle valeur avec l'ancienne
       
    temporaire  += customKey ;
     }

si  ( xref ! =  0  && xref < =  1000 ){    //si la valeur de référence spécifiée à l'aide du pavé numérique n'est pas nulle et inférieure à 1000  , enregistrez la valeur dans xreq
  xreq = xréf ;
}
//contrôle de la sortie PWM de la broche interfacée LED dans la plage _-5 de la valeur d'intensité lumineuse requise xreq
si  ( x< = xreq-5 )  { //augmentation de la sortie PWM de la broche de contrôle LED si l'intensité lumineuse actuelle est inférieure à xreq-5
u = u+5 ; r
}
si  ( x> = xreq+5 )  {   //diminution de la sortie PWM de la broche de contrôle LED si l'intensité lumineuse actuelle est inférieure à xreq+5
u = u-5 ;
}
si  ( u> = 250 ){
  vous = 250 ;
}
si  ( u< = 0 ){
  vous = 0 ;
}
lcd.setCursor ( 9 ,1 ) ; //impression des valeurs sur l'écran LCD
écran LCD.print ( "Req=" ) ;
écran LCD.print ( xreq ) ;
lcd.setCursor ( 12 ,0 ) ;
écran.d'impression ( u ) ;
retard ( 150 ) ;
analogWrite ( pin, u ) ;   // contrôle de la broche pour contrôler les LED ou le système d'éclairage intérieur
uavg =( uavg+u ) /2 ; // calcul de la moyenne des valeurs pour un envoi ultérieur au système
lightavg  =  ( lightavg+lumière ) /2 ;
tempavg  =  ( tempavg+temp ) /2 ;
si  ( i == 200 ){    // envoi des valeurs au module bolt via la communication UART toutes les 200e itération, c'est-à-dire toutes les 30 secondes car 1 itération équivaut à 0,15 seconde
  
  écran LCD.setCursor ( 0 ,0 ) ;
  écran.d'impression ( " " ) ;
  
  Série.print ( lightavg ) ;
  Série.print ( "," ) ;
  Série.print ( tempavg ) ;
  Série.print ( "," ) ;
  Série.print ( uavg ) ;
  Série.print ( ",;" ) ;
  
  lcd.setCursor ( 6 ,0 ) ;
  écran.d'impression ( uavg ) ;
  lcd.setCursor ( 3 ,0 ) ;
  écran LCD.print ( tempavg ) ;
  écran LCD.setCursor ( 0 ,0 ) ;
  écran LCD.print ( lightavg ) ;
  températuremoyenne = 0 ;
  je = 0 ;
  moyenne lumineuse = 0 ;
    
  }
  
lcd.setCursor ( 12 ,0 ) ; //affichage des valeurs sur l'écran LCD
écran.d'impression ( " " ) ;
écran LCD.setCursor ( 0 ,1 ) ;
écran.d'impression ( " " ) ;
}
