# Practica-7A
## Part A: "Reproducció des de memòria interna"
### Codi Complet
```cpp
#include <Arduino.h>
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup()
{
  Serial.begin(115200);

  audioLogger = &Serial;
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(10,9,1);
  aac->begin(in, out);
}


void loop()
{
  
  if (aac->isRunning()) {
    aac->loop();
  } else {
    Serial.printf("AAC done\n");
    delay(1000);
  }
}
```
### Funcionament per parts del codi
__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>
#include <AudioGeneratorAAC.h>
#include <AudioOutputI2S.h>
#include <AudioFileSourcePROGMEM.h>
#include <sampleaac.h>
```
Aquestes línies inclouen les biblioteques necessàries per treballar amb la generació d'àudio AAC, la sortida d'àudio a través del protocol I2S (Inter-IC Sound), i la font de fitxer d'àudio emmagatzemada a la memòria PROGMEM. A més, s'inclou l'arxiu de capçalera sampleaac.h, que conté el fitxer d'àudio en format AAC.


- Arduino.h
- AudioGeneratorAAC.h
- AudioOutputI2S.h
- AudioFileSourcePROGMEM.h
- sampleaac.h
  
__2. Declaració de Pointers a Objectes__
```cpp
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```
Aquí es declaren pointers a objectes de les classes AudioFileSourcePROGMEM, AudioGeneratorAAC i AudioOutputI2S. Aquests objectes s'utilitzaran per gestionar la font d'àudio, la generació d'àudio AAC i la sortida d'àudio, respectivament.

__3. Setup__
```cpp
void setup()
{
  Serial.begin(115200);

  audioLogger = &Serial;
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(10,9,1);
  aac->begin(in, out);
}
```
- Serial.begin(115200);: S'inicia la comunicació serial a una velocitat de 115200 bauds per permetre la depuració.
- audioLogger = &Serial;: S'assigna l'objecte Serial com a logger d'àudio per registrar missatges de depuració.
-  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac)): Es crea un nou objecte AudioFileSourcePROGMEM que utilitza el fitxer d'àudio sampleaac emmagatzemat a PROGMEM.
- aac = new AudioGeneratorAAC(): Inicialitza el generador d'Àudio.
- out = new AudioOutputI2S(): Inicialització de la sortida d'Àudio I2S.
- out -> SetGain(0.125): Estableix el guany de l'àudio a 0.125.
- out -> SetPinout(10,9,1): es configuren els pins per la sortida I2S (pin 10 per WS, pin 9 per BCK i pin 1 per DATA).
- aac->begin(in, out): S'inicia el generador d'àudio AAC amb la font d'àudio in i la sortida d'àudio out.
  
__4. Loop__
```cpp
void loop()
{
  
  if (aac->isRunning()) {
    aac->loop();
  } else {
    Serial.printf("AAC done\n");
    delay(1000);
  }
}

```
- if (aac->isRunning()) {aac->loop();}: Es verifica si el generador d'àudio AAC està en funcionament. Si és així, es crida a la funció loop() de l'objecte aac per processar l'àudio.
- Serial.printf("AAC done\n"): Imprimeix per pantalla el missatge "AAC done" .
- delay(1000);: 'espera un segon abans de la següent iteració del bucle.


Resum del funcionament: 
Configura un sistema en Arduino per reproduir un fitxer d'àudio AAC emmagatzemat a PROGMEM a través d'una sortida d'àudio I2S, amb missatges de depuració impresos al monitor serial.
