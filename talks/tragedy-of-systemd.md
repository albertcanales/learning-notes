[The Tragedy of Systemd](https://youtu.be/o_AIw9bGogo)

Xerrada divulgativa/històrica per explicar el backgound de Systemd. No és molt potent a nivell conceptual però he pres quatre apunts dels punts claus en la història de systemd igualment.

## Apunts

UNIX trenca amb el paradigma del moment en els SO essent extremadament simple, molts SO surten amb aquesta filosofia.

Un daemon es un procés que corre de fons. Amb l'arribada d'internet, els processos es van complicant i neix el concepte de servei, un superset més complexe dels daemons.

Com que init (el procés pare que gestiona la resta de processos del moment) no pot gestionar el concepte de servei, llavors neix systemd

Actualment els ordenadors són tan complexos que s'ahn allunyat de la distinció kernel vs usuari inicial. El "System" corre entremitges, només parcialment dins del Kernel però indispensable pel funcionament del SO. Algunes d'aquestes funcionalitats són el network per exemple. Separar-ho del Kernel permet cert dinamisme.

Tot i que diverses distribucions utilitzin els guidelines de UNIX, actualment Linux té una porció tant gran que pot establir unilateralment la forma d'evolucionar sobre la resta (FreeBSD, Solaris maybe)
