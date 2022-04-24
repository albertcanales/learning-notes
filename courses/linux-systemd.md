[Linux systemd - tutoriaLinux](https://youtube.com/playlist?list=PLtK75qxsQaMKPbuVpGuqUQYRiTwTAmqeI)

## Units

Les units es guarden en:
- `/lib/systemd/system`: Les del sistema
- `/usr/lib/systemd/system`: Les dels package-manager
- `/etc/systemd/system`: Les del usuari

Per recarregar el contingut dels fitxers:
`systemd daemon-reload`


## Systemctl

- **reload vs restart**: Reload és opcional, si està definit permetrà recarregar la nova configuració del servei sense haver de fer restart. Si restart no està definit es fa stop+start.
- **enable / disable**: Afecten *només* al boot, no fa start i stop


## Targets

- **Wants / Wanted By**: *MAY* iniciar al mateix temps
- **Requires / Required By**: *MUST* iniciar al mateix temps
- **Before / After**: Molt restrictius, no abusar per fer molta feina
- També hi ha altres menors: Requisite, BindsTo, PartOf, Conflics
- **NO** tocar les units del sistema, sols amb un dels dos ja funciona, posar el adequat al user