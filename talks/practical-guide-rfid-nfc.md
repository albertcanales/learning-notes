[CONFidence 2018: Practical Guide to Hacking RFID/NFC](https://youtu.be/7GFhgv5jfZk)

Les targetes inalàmbriques són classificades respecte a la seva frequència de funcionament:
- **Low frequency (RFID)**: 125 kHz
- **High frequency (NFC)**: 13.56 MHz
- **Altres**: Per exemple cotxes (**UHF** 868 Mhz)

L'identificació amb el mòbil treballa amb NFC, i per tant a High Frequency.

Sovint s'utilitza una targeta o un mòbil com a identificació, a través d'un UID. Depenent de com es passi el UID al lector, hi ha mètodes d'atac molt diferents.

## Unprotected UID
És el tipus més simple, normalment es composa per:
- Pocs bytes
- Sols lectura
Per tant, és fàcil de llegir i posteriorment imitar (spoofing).

Per a fer spoofing s'utilitzen diferents eines:
- **T5575** per low freq
- **Magic UIDs** per high freq
- **RFID cloner** molt menys segur però més versàtil
- **NFC Card Emulator** app de mòbil, requereix root

Per a NFC, on la lectura és una mica més complexa:
- **PN532** (bastant barat) juntament amb **libnfc** per explotar la lectura
- Apps de Smartphone que permeten llegir un UID
- smartlockpicking.com/nfc-toolkit

Les magic cards són targetes en les que es pot escriure el valor que es desitgi a dins, permetent així l'spoofing. 

Per evitar això, alguns lectors abans de llegir envien els codis de escritura de les magic cards, per esborrar-ne el contingut i denegar l'atac. Hi ha disponibles magic cards que només permetes una escriptura que eviten aquesta defensa.

Altres tècniques més simples però efectives:
- **Visual UIDs**: Algunes targetes venen amb el UID escrit, no cal complicar-se la vida per llegir el valor
- **Enginyeria social**: Tenir la targeta en blanc, ja t'obrirà el següent que passi.

## Protected UID (ICLASS)
- Només el lector vàlid (amb una clau privada) farà que la targeta emeteixi el valor
- Per atacar el sistema, buscar aconseguir els valors directes de memòria

### Atac Post-Lector
**Targeta** -> **Lector** -> **Control** (per exemple raspi) -> **Activador** (electroimán)

Sovint per la connexió *Lector -> Control* s'envia la lectura desencriptada de la targeta. Si el lector és fàcilment desmuntable, es podria fer un MITM.
- **BLE Key**: Sniffer entre el lector i el control (~20€)
- **ESP-RFID Tool / ESP Key**

## Targetes amb dades (MIFare)

Depenent de la versió, les dades s'organitzen de formes diferents. El lector pot llegir i escriure aquestes dades i permet o no l'accés en funció del valor d'aquestes.

- Podem llegir les dades amb **MiFare++ Ultralight** (app de Android)
- Normalment protegit contra escritura, però amb les dades anteriors de vegades es pot fer spoofing amb una simple Magic Card

El model de targeta més utilitzat és **MiFare Classic**:
- 2 sectors, protegits amb claus d'accés. Sovint senzilles, i es poden crackejar amb brute-force, per exemple amb **MIFare Classic Tool**

> Això del plan no entenc molt bé el que vaig apuntar en el seu moment, probablement no tingui molt sentit

Amb una targeta MIFare Classic, el esquema d'atac és el següent:
- S'utilitzen keys per defecte o estan leakejades?
	- *SI*. Fet!
	- *NO*. Es té almenys una clau?
		- *SI*. **Nested Attack**: Amb un *PN532*, pot tardar uns minuts
		- *NO*. **Darkside Attack**: Amb un *PN532*, pot tardar una hora

Les versions més actuals de MIFare ja han parchejat errors com els utilitzats en la Classic. L'atac al model més modern a data de la conferència és diu *MyLazyCracker*.

### Decoding de les dades

Moltes vegades les dades de la targeta no es troben amb text pla (ja que seria trivial fer reverse engineer per treure el patró), sinó que s'utilitzen diversos encodings com **XOR Encoding**.

- **Metrodroid**: Llegeix i decodifica targetes de transport públic