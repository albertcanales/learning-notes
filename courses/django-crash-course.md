[Python Django Course for Beginners 2021 - Learn Django from Scratch in this 100% Free & Tutorial!](https://www.youtube.com/watch?v=t7DrJqcUviA&t=2081s)

Una WEBPAGE és un conjunt de APPS (similars a paquets).

Iniciar un projecte:
`python manage.py startapp APPNAME`

Obrir el servidor (a localhost:8000 per defecte):
`python manage.py runserver`

# Fitxers importants

## `views.py`
Una **view** és una funció que té retorna un HTML com a request de una petició de HTTP. Tenen `request` com a paràmetre, com una espècie de *context* gestionat per Django.

## `urls.py` (de fora de la APP)
Conté una variable `urlpatters` que indica quina view li correspon a cada path.
Es poden fer paths dinàmics amb slugs i coses d'aquetes, està al codi.

## `settings.py`
Com una espècie de manifest, bastant autoexplicatiu. S'han d'afegir les apps perquè Django les consideri, a l'estil de les Activities en Android Java


# Templating

Permet posar contingut dinàmic als HTML. Hi ha de dos tipus:

Template **interpolation**: Sintaxi `{{ VARIABLE }}`. Incrusta el contingut de variable. OJO!: ~~`{{ dict['name'] }}`~~ -> `{{ dict.name }}`. A més, per cridar mètodes s'ometen els parèntesis

La resta de templates: Sintaxi `{% COMMAND %}`. On command segueix una espècie de pseudollenguatge. Algunes comandes:
- `if, for, endif, endfor...` acompanyats de sintaxi de Python de tota la vida
- `static`: Per posar paths a fitxers (tipus CSS). S'ha de fer així pq Django mou els paths en el HTML final
- `block TITLE`, `endblock`: Entre ells es posa el valor per defecte. Un fill que inclogui les mateixes templates substituirà el contingut de dins del pare
- `extends PARE`: Per extendre del pare, PARE segueix la sintaxi que tindria si estigués amb un static
- `include PATH`: Inclou a dins el HTML dins de path. útil per fer elements modulars

# Models

Els models són les classes a les que ens interessa tenir una taula de la DB associada.

Cada model és una classe de `models.py` que hereda de `models.Model`. Els camps del model són a l'estil SQL (tipus complexos). Cada tipus té les seves propietats, interessa mantenir-se en l'estàndard per tenir més feina feta.

Per generar les taules corresponents tenim.

Crea el codi que genera les taules SQL, es veu a `APP/migrations` i és human-readable
`python manage.py makemigrations`

Aplica el codi de les migrations per crear les taules
`python manage.py migrate`

## Tipus

Entre els tipus de Django es destaca `SlugField` per generar fàcilment pseudoidentificadors i `ImageField`, que gestiona molt fàcilment el tractament de imatges en el servidor.

Pel segon cal afegir `MEDIA_ROOT` i `MEDIA_URL` a `settings.py` per indicar on es guarden i on s'accedeix la media de la pàgina web.

Cada tipus té els seus atributs estecífics, sent `blank : Boolean` atribut comú per permetre valors nuls.

## Relacions entre models

Es poden definir les mateixes relacions entre elemens que SQL:
- One To One: Propietat
- One To Many: ForeinKey (clau forània)
- Many To Many: ManyToManyKeys (internament clau forània a una taula separada)



# Admin

Per gestionar els models des de l'administració de la web hi ha el fitxer `admin.py`. Es crea un usuari de l'administració amb:
`python manage.py createsuperuser`

Per cada model que es crea, s'ha d'indicar en aquest fitxer (`admin.site.register`) que el volem fer accessible des de l'administració.

Per personalitzar cada model, es crea una classe `<MODEL>Admin (admin.ModelAdmin)` i diferents variables modifiquen el comportament de la pàgina de admin. A la documentació de Django hi ha les opcions disponibles


# Forms

Django pot gestionar els formularis a partir de `forms.py`, que es crea en cada app concreta. Cada classe del fitxer és un formulari, i té associat els models que "interactuen" amb el formulari. Per cada model es pot seleccionar els camps d'interès

A views, s'instancia el form i es passa al HTML. Allí es posa el contingut dins d'un form, i es fa el mètode de submit. Cal utilitzar `{% csrf_token %}` al form per seguretat. Es pot posar un enllaç ja existent discernint `request.method`.

En la view que rep el post, s'inicialitza un form amb la request (`MyForm(request.POST)`). Es comprova la validesa amb `is_valid` i s'afegeixen els models a la DB amb `save`. Els dos mètodes poden ser reescrits si es vol.

En cas de tractar amb ManyToMany, s'utilitza `add` per afegir el nou valor a la subtaula interna.

També es pot crear un form a partir de `forms.Form`, ja no va lligat a un model concret i ens hem de programar nosaltres mateixos funcions com `save`, però es útil per casos on el funcionament per defecte no és el que es voldria.