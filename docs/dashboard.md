
# Projecten
## Projectstructuur

Trillingsmetingen via Frogwatch worden gedaan vanuit de context van een project.
Projecten zijn ingedeeld in *clusters* die elk hun eigen instellingen hebben.
Meetpunten bestaan binnen een cluster, vaak representeren die een fysieke locatie waar gemeten moet worden.

Vervolgens wordt er een Frogwatch Meter in het meetpunt geplaatst om de trillingsdata te verzamelen. Dit is heel flexibel, want de data hoort bij het meetpunt, niet bij de meter. Dus je kunt de meter in een meetpunt zonder problemen vervangen.

![ProjectStructuur](img/ProjectStructuur.png).

## Clusters

Het aanmaken van clusters en meetpunten gebeurt in het `Clusters` menu binnen een project. Hier zijn meestal drie stappen nodig:

1. Maak een cluster aan. Geef het cluster eventueel een logische naam.
2. Voeg meetpunten toe aan het cluster. Aan de meetpunten kan opnieuw een naam gegeven worden.
3. Selecteer een Meter die de metingen van het meetpunt gaat uitvoeren.
4. Activeer de meter in het meetpunt door op het link-icoon te klikken. LET OP: als een meter op dat moment nog actief is in een ander project zal deze stoppen met meten en de meetinstellingen van het nieuwe cluster krijgen.

![sensorlink](img/link.png).

Zodra er meetdata is ontvangen voor 1 van de meetpunten binnen een cluster wordt de meetconfiguratie gelocked en kan het cluster niet meer verwijderd worden.

Een meter kan op elk moment verplaatst worden naar een ander meetpunt, maar kan op een later tijdstip ook weer teruggeplaatst worden in een vorig meetpunt door opnieuw op het link-icoon te klikken.

## Meetconfiguratie

Per cluster kan er een meetconfiguratie worden ingesteld. In de configuratie kan bepaald worden

* Wanneer er gemeten wordt: op welke dagen en tussen welke tijdstippen
* Hoe er gemeten wordt: Volgens welke methode, in welke eenheid
* Hoe wordt de meting getoetst: Wordt er een norm gebruikt, en welke?

### Meetperiode

Er kan ingesteld worden op welke dagen van de week er gemeten wordt en vervolgens tussen welke tijdstippen op die dagen de meetpunten actief zijn. Buiten deze periode zijn de meters in stroombesparingsstand. Dat wil zeggen dat ze alleen af en toe verbinding maken met het Frogwatch Dashboard om te checken of er nieuwe instellingen zijn.

### Meetmethode

Op dit moment worden er 4 meetmethodes ondersteund:

1. **Schademeting Snelheid (SBR A/C).** Meten met een schadenorm in het snelheidsdomein werkt volgens de SBR A richtlijn. Dat wil zeggen dat er elke seconde één datapunt per as (x, y en z richting) wordt uitgerekend. Een datapunt bestaat uit de dominante frequentie en de bijbehorende topwaarde (snelheid of acceleratie) over een window van 1 seconde. De dominante frequentie wordt bepaald aan de hand van de norm. De norm is gedefinieerd over het bereik van 1 - 100 Hz. De frequentie met de meeste energie ten op zichte van de norm is de dominante frequentie (dit wordt bepaald via een Fast Fourier Transformatie). De topwaarde wordt bepaald in het tijdsdomein. Voor meer informatie zie de officiële documentatie over de SBR A en C richtlijnen. Ook kan er zonder norm gemeten worden. Dat wil zeggen dat de frequenties niet gewogen worden en dat er geen normoverschrijdingen kunnen plaatsvinden.
2. **Schademeting Acceleratie (SBR C).** In deze methode wordt er gemeten in het acceleratiedomein, verder is deze methode equivalent aan *Schademeting Snelheid*. Deze mode is geschikt voor SBR C-acceleratie normen.
3. **Hindermeting (SBR B).** Deze methode gaat volgens de SBR B richtlijn. Dat wil zeggen dat er elke 30 seconde een datapunt wordt uitgerekend waar een Vmax uit volgt. Dit is een dimensieloos  getal gerelateerd aan de trillingssnelheid (Een effectieve waarde gemiddeld over 30 seconde). Deze methode is dus te gebruiken met SBR B normen en ook zonder norm.
4. **Kalibratie.** Kies deze meetmethode als uw meters gekalibreerd moeten worden. Dit zorgt automatisch voor de juiste meetinstellingen. Ook wordt in deze methode de trillingsdata niet verstuurd naar het Frogwatch Dashboard om de kalibratie sneller te laten verlopen.

### Opties

#### Dataregistratiedrempel

De registratiedrempel is een manier om hele lage trillingswaardes te negeren: waardes lager dan deze drempelwaarde komen niet beschikbaar in het Frogwatch Dashboard. Dit bespaart energie en zorgt ervoor dat je alleen data in de grafieken ziet die belangrijk is. Standaard staat dit op 0 (uit: alle waardes beschikbaar).

#### Meting hoger dan 1g

Verwacht je zeer hevige trillingen boven 1g (de zwaartekrachtsversnelling)? Dan kun je deze optie aanvinken. Dit staat standaard uit omdat de meeste normen veel lager dan 1g liggen.

#### Meting tot 300Hz

Standaard word er tot 100Hz gemeten. In sommige situaties kan het wenselijk zijn om ook hogere frequenties te kunnen detecteren. Als de dominante frequentie boven 100Hz ligt, wordt deze getoetst aan de 100Hz norm (indien van toepassing).

#### Real time frequentiedata (FFT) opslaan

**LET OP**: *Dit wordt alleen ondersteund door meters met firmware **2.9.0** of nieuwer*.

Dit is een geavanceerde optie die het mogelijk maakt het volledige FFT specrum op te slaan. Voor elke seconde dat er gemeten is, wordt dan het totale spectrum van 0-100Hz opgeslagen.

Omdat dit erg veel data is, stuurt de Frogwatch deze niet door naar het dashboard. De data wordt intern in de meter opgeslagen en kan [via Frogwatch USB worden uitgelezen](../usb/#uitlezen-eerder-opgeslagen-frequentiedata).

### Toetsing

Kies in deze sectie een van de normen (of geen) die beschikbaar zijn voor de gekozen meetmethode. Voor SBR A en C normen kunnen tevens veiligheidsfactoren worden ingesteld. Dit zorgt er effectief voor dat de norm verschoven wordt.

### Bewaking

Als er een norm is ingesteld wordt de `Bewaking`-sectie zichtbaar.  In deze sectie kan ingesteld worden wie er een melding krijgt als het trillingsniveau een bepaald percentage van de norm bereikt. En hoe vaak dit (maximaal) moet gebeuren.

#### Alarmeringsgevoeligheid

Het minimaal aantal overschrijdingen dat moet optreden binnen een minuut voordat er een alarm wordt uitgestuurd. Als er bijvoorbeeld `3` wordt ingesteld, dan wordt er alleen een overschrijdingsbericht gestuurd als er binnen 1 minuut 3x een bepaald trillingsniveau is gemeten. Na elke verstuurd bericht is er een back-off periode waarin geen meldingen worden gestuurd naar contactpersonen, ook al zijn er wél overschrijdingen. Overschrijdingen worden echter nog wel doorgegeven aan Frogwatch Dashboard.

#### Alarminterval

De minimale tijd in minuten die tussen elk opvolgend bericht(sms of email) moet zitten. Deze instelling werkt als een rate-limit en zorgt er voor dat contactpersonen niet te vaak meldingen krijgen.

#### Contactpersonen

In de contactpersonen tabel kan ingesteld worden wie er een bericht krijgt bij een bepaald trillingsniveau ten opzichte van de norm. De norm is hierbij 100%. Dus om meldingen te ontvangen als het trillingsniveau bijna een normoverschrijding veroorzaakt vul je bijvoorbeeld `95%` in voor een bepaald contactpersoon.

Contactpersonen worden per project toegevoegd. Dit gebeurt in het `Toegang` menu binnen het project. Daar kan ook ingesteld worden of contactpersonen via sms of email gealarmeerd worden, en op welke dagen dit wel of niet mag gebeuren.


## ProjectInfo

Op de `Info` pagina binnen een project kunnen projectdetails en metadata worden ingevuld. Ook kunnen er documenten worden geüpload, bijvoorbeeld meetplannen. Deze pagina kan worden ingezien door opdrachtgevers of andere mensen met een account die toegang hebben gekregen tot dit project.

Als het project afgelopen is kan het project hier gearchiveerd worden. Daardoor verdwijnt het project uit de lijst met actieve projecten. Het project kan op elk moment weer actief gemaakt worden mocht dat nodig zijn.

## Meldingen

De `Meldingen` pagina toont berichten afkomstig van de Meters die actief zijn in het project.

Er zijn 4 soorten meldingen:

1. **Info** Dit zijn service berichten die laten weten wanneer een systeem opgestart is, gestart en gestopt met metingen etc.
2. **SBR** meldingen worden verstuurd als er een normoverschrijding is geweest. Deze melding laat het exacte tijdstip en trillingsniveau ten opzichte van de norm zien.
3. **Error** Als er iets fout gaat tijdens het meten of met de normale operationele activiteiten van een Frogwatch Meter wordt dat gemeld doormiddel van *Error* meldingen. De meters proberen te detecteren wat er precies fout gaat of wat er kapot is en melden dat op deze manier. Denk aan een kapotte sensor of een corrupte SD kaart. Voor deze meldingen zijn meestal acties vereist van de gebruiker. En eventueel moet de meter een servicebeurt krijgen.
4. **Warning** berichten zijn verwachte uitzonderingen die af en toe kunnen gebeuren maar waar de meter zelf van kan herstellen. Deze kunnen vaak beschouwd worden als diagnostische meldingen.

## Rapportage

Frogwatch Dashboard kan automatische rapportages genereren en versturen. Rapportages zijn per dag of per week. Wat er in het rapport staat is flexibel. Er kan per rapport gekozen worden welke meetpunten meegenomen worden voor die rapportage en welke contactpersonen de rapportage moeten ontvangen. Notificaties voor nieuwe beschikbare rapporten wordt 's ochtends rond 8 uur verstuurd per email.

Een rapport kan voorzien worden van beschrijvingen en foto's en afbeeldingen.

Naast automatische rapportage kun je ook een rapport handmatig genereren voor willekeurige tijdsbereiken. Bijvoorbeeld over het laatste uur, of over een hele maand. Handmatige rapportages worden niet automatisch verstuurd maar deze zijn wel te downloaden zodat je ze eventueel zelf kunt versturen.

# Toegang

## Gebruikers
Als organisatie binnen Frogwatch kun je zelf toegang en rechten verlenen aan andere gebruikers. Er zijn 3 type `rollen` beschikbaar:

1. `Manager` Een manager heeft alle rechten binnen een groep: lezen, schrijven, verwijderen, nieuwe gebruikers aanmaken.
2. `Spectator` is een gebruiker die wél leesrechten heeft voor alle projecten en instellingen binnen de groep, maar kan zelf niets aanpassen.
3. `User` heeft standaard geen rechten. Deze gebruiker kan niets zien/lezen en ook niet aanpassen. Een manager kan gebruikers toegang geven tot bepaalde projecten via het `Toegang` menu binnen een project. Dit is vooral bedoeld voor opdrachtgevers die graag mee willen kijken bij bepaalde projecten. Een `user` kan nooit instellingen aanpassen.

Een nieuwe gebruiker krijgt automatisch een email met een link om een wachtwoord aan te maken.

![Nieuwe Gebruiker](img/newuser.png).

## Contacten

Naast gebruikers zijn er ook contacten. Een contact kan niet inloggen op het Dashboard, maar kan informatie ontvangen in de vorm van emails en sms-berichten. Er kunnen berichten verstuurd worden als er normoverschrijdingen gemeten worden, of als er een nieuwe rapport met trillingsdata beschikbaar is.

Contacten worden in tegenstelling tot een `user` ingesteld per project. Als je een nieuw `contact` definieert moet je een emailadres en/of een telefoonnummer invullen. Daarnaast moet je expliciet aangeven of er emails en/of sms berichten verstuurd moeten worden. Hiermee kun je meldingen dus aan of uit zetten voor dit contact. Daarnaast is er een meldingmasker waarmee aangegeven wordt op welke dagen er berichten verstuurd mogen worden. Dus bijvoorbeeld alleen op de dagen dat een contact persoon aan het werk is.


![Nieuwe Gebruiker](img/authorisatie.png).

# Trillingsnormen

Via Frogwatch Dashboard kunnen trillingsnormen flexibel gedefinieerd worden.  Op deze manier kun je heel gemakkelijk meten via verschillende normen. Op dit moment wordt er onderscheid gemaakt tussen Schadenormen en Hindernormen.  Schadenormen kunnen zowel in het acceleratiedomein als in het snelheidsdomein gedefinieerd worden zodat SBR A en SBR C beide zijn ondersteund.
Met hindernormen wordt volgens de SBR B richtlijn gemeten.

## Schadenormen (SBR A en SBR C)

Ga naar het hoofdmenu item `Normen` en klik op `Schadenormen`. Hier wordt een lijst getoond van alle normen die aangemaakt zijn.

### Norm Definiëren

Klik boven aan de lijst op `+ NIEUWE NORM`.
Doormiddel van Frequentie - Acceleratie/snelheids paren wordt de norm gedefinieerd.
Kies het type norm: `snelheid` of `acceleratie`. Voeg vervolgens een aantal datapunten toe die samen door middel van interpolatie een norm vormen.
Kies een unieke naam voor de norm en klik op `OPSLAAN`. De Norm verschijnt nu in de lijst.

![SBR A Norm](img/norm.png).


## Hindernormen (SBR B)

Een Hindernorm bestaat, zoals de SBR B richtlijn omschrijft uit 6 waardes. Drie waardes voor de *dag* en *avond* periode. En 3 waardes voor de *nacht* periode.

### Norm Definiëren
Ga naar het hoofdmenu item `Normen` en klik op `Hindernormen`. Hier wordt een lijst getoond van alle normen die aangemaakt zijn. Klik boven aan de lijst op `+ NIEUWE NORM`.
Er verschijnt een popup met 7 invoer velden. De waardes A1, A2 en A3 voor zowel de dag/avond en de nacht periode, en 1 veld voor de unieke naam van de norm.

 ![SBR B Norm](img/norm_b.png)


# Statusoverzicht Meters

In het hoofdmenu item `Meters` zie je een overzicht van alle meters binnen de organisatie. Een vergelijkbaar overzicht is ook beschikbaar voor de status binnen één project: ga naar `Projecten`, en kies een project, of klik op `status` in het linker menu.

In het overzicht zie je een kolom `Status`. Deze kolom geeft een samenvatting van wat de sensor precies aan het doen is. In de tabel hieronder wordt toegelicht wat elke status betekent.
 ![Betekenis status](img/statushelp.png)

 Verder vind je in de statustabel nog een aantal kolommen die informatie geven over de meter:

 * `Meetperiode`: sinds wanneer de meting actief is en wanneer deze het laatst gestopt is.
 * `Signaal`: een indicatie van het netwerkbereik (100% is perfect, 0% is zeer slecht)
 * `Batterij`: hoe vol de batterij is en of de voeding aangesloten is.
 * `Temp`: de temperatuur binnen in de meter. Hieraan kun je aardig inschatten of de meter binnen of buiten staat.
 * `Laatste Update`: het tijdstip dat de meter voor het laatst contact heeft gemaakt met het Dashboard. Informatie van na dit tijdstip is nog niet beschikbaar.
 * `Commando`: hiermee kun je een `START` of `STOP` klaarzetten voor de meter. Zodra de meter online komt zal de meting gestart of gestopt worden.

# Trillingsdata inzien

Via het menu `Trillingen` binnen een project kan de gemeten trillingsdata interactief bekeken worden.

De eerste stap is om de gewenste meetpunten te selecteren in de linker kolom.

 ![data selectie](img/data-selectie.png)

Vervolgens selecteer je aan de rechter kant in de filtersectie het gewenste tijdbereik en resolutie.

De resolutie bepaalt hoeveel datapunten er geladen worden. Hoe meer datapunten hoe meer detail, maar bij te veel datapunten kan het lang duren voordat de plot geladen is. Om overbelasting van de browser te voorkomen wordt de data automatisch gedownsampled. Dit kan uitgezet of aangepast worden door op `Downsampling` te klikken.

Klik op `LAAD` om de data te laden en te plotten.

 ![data selectie](img/plot.png)

 De juiste plots worden automatisch getoond voor de data die beschikbaar is. Dit kan verschillen per cluster en is afhankelijk van de meetconfiguratie.

 Verschillende meetpunten die tegelijk zijn geselecteerd worden over elkaar heen geplot om ze eenvoudig te kunnen vergelijken.

 De plots zijn interactief. Als je in 1 plot zoomt, dan zoomen de ander plots mee als dat mogelijk is. Na het zoomen kan het soms handig zijn om opnieuw op `LADEN` te klikken (of op `Enter` te drukken) zodat de data in een hogere resolutie geladen wordt. Dit zorgt ervoor dat je meer details kunt zien.