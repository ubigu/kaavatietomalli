---
layout: "default"
description: ""
id: "dokumentaatio"
status: "Keskeneräinen"
---
{% include common/important.html content="Sisältö ei vielä ajantasalla UML-kaavion kanssa" %}

# Loogisen tason kaavatietomalli
{:.no_toc}

1. 
{:toc}

## Yleistä
Loogisen tason Kaavatietomalli määrittelee kaikille kaavalajeille yhteiset tietorakenteet, joita sovelletaan kaavatiedon ilmaisemiseen kullekin kaavalajille laadittujen soveltamisohjeiden ([asemakaava](../../soveltamisohjeet/asemakaava/), [yleiskaava](../../soveltamisohjeet/yleiskaava/)) ja niissä kiinnitettyjen koodistojen sekä [elinkaari](../elinkaarisaannot.html)- ja [laatusääntöjen](../laatusaannot.html) mukaisesti. Looginen tietomalli pyrkii olemaan mahdollisimman riippumaton tietystä toteutusteknologiasta tai tiedon fyysisestä esitystavasta (esim. relaatiotietokanta, tietyn ohjelmointikielen tietorakenteet, XML, JSON).

## Normatiiviset viittaukset
Seuraavat dokumentit ovat välttämättömiä tämän dokumentin täysipainoisessa soveltamisessa:

* [ISO 639-2:1998 Codes for the representation of names of languages — Part 2: Alpha-3 code][ISO-639-2]
* [ISO 8601-1:2019 Date and time — Representations for information interchange — Part 1: Basic rules][ISO-8601-1]
* [ISO 19103:2015 Geographic information — Conceptual schema language][ISO-19103]
* [ISO 19107:2019 Geographic information — Spatial schema][ISO-19107]
* [ISO 19108:2002 Geographic information — Temporal schema][ISO-19108]
* [ISO 19109:2015 Geographic information — Rules for application schema][ISO-19109]
* [ISO 19505-2:ISO/IEC 19505-2:2012, Information technology — Object Management Group Unified Modeling Language (OMG UML) — Part 2: Superstructure][ISO-19505-2]

## Standardienmukaisuus
Looginen kaavatietomalli perustuu [ISO 19109][ISO-19109]-standardin yleinen kohdetietomalliin (General Feature Model, GFM), joka määrittelee rakennuspalikat paikkatiedon ISO-standardiperheen mukaisten sovellusskeemojen määrittelyyn. GFM kuvaa muun muassa metaluokat ```FeatureType```, ```AttributeType``` ja ```FeatureAssociationType```. Kaavatietomallissa kaikki tietokohteet, joilla on tunnus ja jota voivat esiintyä erillään toisista kohteista on määritelty kohdetyypeinä (stereotyyppi ```FeatureType```. Sellaiset tietokohteet, joilla ei ole omaa tunnusta ja jotka voivat esiintyä vain kohdetyyppien attribuuttien arvoina on määritelty [ISO 19103][ISO-19103]-standardin ```DataType```-stereotyypin avulla. Lisäksi [HallinnollinenAlue](#hallinnollinenalue) ja [Organisaatio](#organisaatio) on mallinnettu vain rajapintojen (```Interface```) avulla, koska on niitä ei ole tarpeen kuvata kaavatietomallissa yksityiskohtaisesti, ja on todennäköistä, että kaavatietovarastoja ylläpitävät tietojärjestelmät tarjovat niille konkreettiset toteuttavat luokat.

[ISO 19109][ISO-19109] -standardin lisäksi Kaavatietomalli perustuu muihin paikkatiedon ISO-standardeihin, joista keskeisimpiä ovat [ISO 19103][ISO-19103] (UML-kielen käyttö paikkatietojen mallinnuksessa), [ISO 19107][ISO-19107] (sijaintitiedon mallintaminen) ja [ISO 19108][ISO-19108] (aikaan sidotun tiedon mallintaminen).

### Muulla määritellyt luokat ja tietotyypit

#### CharacterString

Kuvaa yleisen merkkijonon, joka koostuu 0..* merkistä, merkkijonon pituudesta, merkistökoodista ja maksimipituudesta. Määritelty rajapinta-tyyppisenä [ISO 19103][ISO-19103]-standardissa.

#### LanguageString

Kuvaa kielikohtaisen merkkijonon. Laajentaa [CharacterString](#characterstring)-rajapintaa lisäämällä siihen ```language```-attribuutin, jonka arvo on ```LanguageCode```-koodiston arvo. Kielikoodi voi [ISO 19103][ISO-19103]-standardin määritelmän mukaan olla mikä tahansa ISO 639 -standardin osa.

#### Number

Kuvaa yleisen numeroarvon, joka voi olla kokonaisluku, desimaaliluku tai liukuluku. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa.

#### Integer

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on kokonaisluku ilman murto- tai desimaaliosaa. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa.

#### Decimal

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on desimaaliluku. Decimal-rajapinnan toteuttava numero voidaan ilmaista virheettä yhden kymmenysosan tarkkuudella. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa. Decimal-numeroita käytetään, kun desimaalien käsittelyn tulee olla tarkkaa, esim. rahaan liityvissä tehtävissä.

#### Real

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on tarkkudeltaan rajoitettu liukuluku. Real-rajapinnan numero voi ilmaista tarkasti vain luvut, jotka ovat 1/2:n (puolen) potensseja. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa. Käytännössä esitystarkkuus riippuu numeron tallentamiseen varattujen bittien määrästä, esim. ```float (32-bittinen)``` (tarkkuus 7 desimaalia) ja ```double (64-bittinen)``` (tarkkuus 15 desimaalia).

#### TM_Object

Aikamääreiden yhteinen yläluokka, käytetään, mikäli arvo voi olla joko yksittäinen ajanhetki tai aikaväli. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa. 

#### TM_Instant

Kuvaa yksittäisen ajanhetken 0-ulotteisena ajan geometriana, joka vastaa pistettä avaruudessa. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa. Aikapisteen arvo on määritelty ```TM_Position```-luokalla yhdistelmänä [ISO 8601][ISO-8601-1]-standin mukaisia päivämäärä- tai kellonaika-kenttiä tai näiden yhdistelmää, tai muuta ```TM_TemporalPosition```-luokan avulla kuvattua aikapistettä. Viimeksi mainitun luokan attribuutti ```indeterminatePosition``` mahdollistaa ei-täsmällisen ajanhetken ilmaisemisen liittämällä mahdolliseen arvoon luokittelun tuntematon, nyt, ennen, jälkeen tai nimi.

#### TM_Period

Kuvaa aikavälin [TM_Instant](#tm_instant)-tyyppisten ```begin```- ja ```end```-attribuuttien avulla. Molemmat attribuutit ovat pakollisia, mutta voivat sisältää tuntemattoman arvon  ```indeterminatePosition = unknown``` -attribuutin arvon avulla annettuna. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa.

#### URI

Määrittää merkkijonomuotoisen Uniform Resource Identifier (URI) -tunnuksen [ISO 19103][ISO-19103]-standardissa. URIa voi käyttää joko pelkkänä tunnuksena tai resurssin paikantimena (Uniform Resource Locator, URL).

#### Geometry

Kaikkien geometria-tyyppien yhteinen rajapinta [ISO 19107][ISO-19107]-standardissa. Tyypillisimpiä [ISO 19107][ISO-19107]-standardin Geometry-rajapintaa laajentavia rajapintoja ovat ```Point```, ```Curve```, ```Surface``` ja ```Solid``` sekä ```Collection```, jota käyttämällä voidaan kuvata geometriakokoelmia (multipoint, multicurve, multisurface, multisolid).

#### Point
Täsmälleen yhdestä pisteestä koostuva geometriatyyppi. Määritelty rajapintana [ISO 19107][ISO-19107]-standardissa.

## Kaavatietomallin yleispiirteet

Tietomalli on jaettu kahteen UML-pakettiin: [MKP-ydin](#mkp-ydin) kuvaa maankäyttöpäätösten tietomallintamisessa yleiskäyttöisiksi suunnitellut luokat ja niihin liittyvät koodistot, ja [Kaavatiedot](#kaavatiedot) kuvaa kaavojen mallinukseen tarkoitetut, ydinpakettia hyödyntävät luokat ja niiden koodistot. Myös muita maankäyttöpäätöksiä kuin kaavoja voidaan tulevaisuudessa mallintaa laajentamalla MKP-ydin -paketin luokkia.

UML-mallin suunnittelussa on kiinnitetty erityistä huomiota siihen, että malli tarjoaa hyvän yhteentoimivuuskehikon tulevaisuuden kaavatietojen tietomallipohjaiseen kuvaamiseen erilaisissa tietojärjestelmissä. Malliin on tarkoituksellisesti jätetty useita laajennusmahdollisuuksia käyttäen abstrakteja luokkia ja koodistoja. Käyttämällä yhteistä luokkarakennetta ja erikoistamala koodistoja kaavalajikohtaisesti on saatu aikaan malli, joka mahdollistaa eri kaavalajien käsittelyn, tiedonsiirron ja tallentamisen samojen tietojärjestelmien ja -rakenteiden avulla, mutta tarjoaa kuitenkin riittävät mahdollisuudet eri kaavalajien erityispiirteiden huomioimiseen niiden tietosisällön ja merkityksen osalta.

Kaavatietomallin UML-luokkakaaviot ovat saatavilla erillisellä [UML-kaaviot](../uml/)-sivulla.

### VersioituObjekti
Englanninkielinen nimi: **AbstractVersionedObject**

Stereotyyppi: FeatureType (kohdetyyppi)

Yhteinen yläluokka kaikille kaavatietomallin versiohallituille luokille. Kuvaa kaikkien kohdetyyppien yhteiset ominaisuudet ja assosiaatiot.

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
paikallinenTunnus| localId            | [CharacterString](#characterstring)     | 0..1            | kohteen pääavain (id)
nimiavaruus      | namespace          | [URI](#uri)                 | 0..1            | tunnusten nimiavaruus
viittausTunnus   | referenceId        | [URI](#uri)                 | 0..1            | johdettu nimiavaruudesta, luokan englanninkielisestä nimestä ja paikallisesta tunnuksesta
identiteettiTunnus | identityId       | [CharacterString](#characterstring)     | 0..1            | kohteen versioriippumaton tunnus
tuottajakohtainenTunnus | producerSpecificId | [CharacterString](#characterstring) | 0..1         | kohteen tunnus tuottajatietojärjestelmässä
viimeisinMuutos  | latestChange       | [TM_Instant](#tm_instant)          | 0..1            | ajanhetki, jolloin kohteen tietoja on viimeksi muutettu tuottajatietojärjestelmässä
tallennusAika    | storageTime        | [TM_Instant](#tm_instant)          | 0..1            | ajanhetki, jolloin kohde on tietojärjestelmään

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
korvaaObjektin   | replacesObject     | [VersioituObjekti](#versioituobjekti) | 0..* | kohteen versio, jonka tämä versio korvaa. Voi olla saman kohteen edellinen versio tai poistuva kohde, jonka tämä kohde korvaa. Oltava saman luokan instanssi.
korvattuObjektilla | replacedByObject | [VersioituObjekti](#versioituobjekti) | 0..* | kohteen versio, jolla tämä versio on korvattu. Voi olla saman kohteen seuraava versio tai uusi kohde, jolla tämä kohde on korvattu. Oltava saman luokan instanssi.

### Alueidenkayttoasia
Englanninkielinen nimi: **LandUseCase**

Erikoistaa luokkaa [VersioituObjekti](#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..* | asian nimi
kuvaus           | description        | [LanguageString](#languagestring) | 0..* | asian kuvausteksti
asianhallintaTunnus | Tunnusarvo          | [Tunnusarvo](#tunnusarvo)         | 0..* | asian yksilöivä ja pysyvä, keskitetysti myönnetty tunnus.
diaarinumero           | recordNumber        | [CharacterString](#characterstring) | 0..1 | kirjattavalle asialle, asiakirjalle tai välitoimenpiteelle diaarikaavan mukaan annettava numerotunnus
vireilletulopaiva | timeOfInitiation        | Date   | 0..1 | päivämäärä, jona asia on tullut vireille
elinkaaritila | lifeCycleStatus        | [KaavanElinkaaritila](#kaavanelinkaaritila)   | 0..1 | asian elinkaaren tila
aluerajaus       | geographicalArea           | [Geometry](#geometry) | 0..1            | maantieteellinen alue, jota maankäyttöasia koskee.
asianLiite  | caseAnnex        | [Liiteasiakirja](#liiteasiakirja) | 0..*        | asian kuvaamiseen tai käsittelyyn olennaisesti kuuluva liitetty asiakirja.
metatatietokuvaus | metadata          | [URI](#uri)         | 0..1            | viittaus ulkoiseen metatietokuvaukseen
kuntanumero | MunicipalNumber          | [Number](#number)         | 1            | kuntan, jota asia koskee
digitaalinenAlkuperä | digitalOrigin          | [Number](#number)         | 0..1            | tieto kaavan digitaalisen muodon tuotantotavasta ja siihen liittyvästä juridisuuden asteesta 

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
vastuutaho | responsibleOrganisation | [Organisaatio](#organisaatio) | 0..1 | organisaatio, joka on vastuussa asian käsittelystä
liittyvaAineisto | usedInputDataset | [Lahtotietoaineisto](#lahtotietoaineisto) | 0..* | lähtötietoaineisto, jota asian valmistelussa ja käsittelyssä on hyödynnetty
liittyvaAsia     | relatedMatter      | [Alueidenkäyttoasia](#alueidenkayttoasia) | 0..* | toinen, tähän asiaan liittyä asia. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring),joka kuvaa miten asia liittyy tähän asiaan


### Liiteasiakirja
Englanninkielinen nimi: **AttachmentDocument**

Kuvaa käsitteen [Liiteasiakirja](../../kasitemalli/#liiteasiakirja), stereotyyppi: DataType (tietotyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
laji             | documentType | [AsiakirjanLaji](#asiakirjanlaji) | 1 | tieto, joka kuvaa tuotettavan asiakirjan lajia. Asiakirjoja tuotetaan yleensä asiankäsittelyyn liittyvissä yksittäisissä toimissa tai tapahtumissa (toimenpide). Asiakirjatyyppi noudattaa yhteistä luokitusta/koodistoa, esimerkkejä arvoille ovat raportti, päätös, ilmoitus, muistio, tiedote...
otsikko          | documentIdentifier | [LanguageString](#languagestring) | 0..*            | asiakirjan pysyvä tunnus, esim. diaarinumero tai muu dokumentinhallinnan tunnus
rooli            | role               | [LanguageString](#languagestring) | 0..* | asiakirjan nimi
tiedosto         | file               | [CharacterString](#characterstring)  | 1            | viittaus ulkoiseen lisätietokuvaukseen asiakirjasta


**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asiakirja | Document      | [Asiakirja](#asiakirja) | 1 | Kuvaa liiteasiakirjan metatiedot

{% include common/note.html content="Liiteasiakirja-luokka ei kuvaa dokumentin sisältöä, eikä ota kantaa tapaan, jolla sisältö noudetaan kaavatietovarastosta tai muusta dokumentinhallintajärjestelmästä. Nämä tiedot voidaan kuvata Asiakirja-luokassa." %}

### Asiakirja
Englanninkielinen nimi: **Document**

Stereotyyppi: Interface (rajapinta)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asiakirjanTunnus | documentIdentifier | [CharacterString](#characterstring) | 1 | asiakirjan pysyvä tunnus, esim. diaarinumero tai muu dokumentinhallinnan tunnus
avainsanat       | keywords           | [Avainsanat](#avainsanat) | 1 | tiettyyn aihepiiriin koottu asiasanojen arvojoukko. Asiasana ja asiasanasto tulee esittää parina, jotta aina tiedetään mistä asiasanastosta kyseinen asiasana on poimittu. Huom! Asiasanasto on tarpeellinen metatieto vain tilanteissa, joissa EI käytetä Fintoon URI:lla viittaavaa "asiasana" -ominaisuutta.
nimi             | name               | [LanguageString](#languagestring) | 1 | asiakirjan nimi
henkilötietosisältö | personalDataContent | [HenkilotietosisallonLaji](#hankilotietosisallonlaji) | 1 | kuvaa asiakirjan henkilötietosisällön
julkisuusluokka | categoryOfPublicity | [AsiakirjanJulkisuusluokka](#asiakirjanjulkisuusluokka) | 1 | kuvaa asiakirjan julkisuusluokan
säilytysaika  | retentionTime | [Int](#integer)   | 1  | asiakirjan säilytysaika vuosina ennen sen hävittämistä
saavutettavuus  | accessibility | boolean  | 1 | asiakirjan saavutettavuustieto
tallennusaika  | recordingTime | [URI](#uri)  | 1   | asiakirjan tallennusaika
vahvistuspäivämäärä  | confirmationDate | [URI](#uri)  | 1  | asiakirjan vahvistuspäivämäärä
kieli  | language | [URI](#uri)  | 1 | asiakirjan kieli
metatietokuvaus  | metadataDescription | [URI](#uri) | 1  | viittaus ulkoiseen metatietokuvaukseen
viimeinenMuutosaika  | mostRecentChangeTime  | [TM_Instant](#tm_instant)  | 1 | asiakirjan viimeisin muutosaika

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
vahvistaja | author  | [Toimija](#toimija) | 0..1 | asiakirjan vahvistaja


### Lahtotietoaineisto
Englanninkielinen nimi: **InputDataset**

Kuvaa käsitteen [Lahtotietoaineisto](../../kasitemalli/#lahtotietoaineisto), erikoistaa luokkaa [VersioituObjekti](#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
aineistoTunnus   | datasetIdentifier  | [URI](#uri)         | 0..*            | lähtötietoaineiston tunnus
nimi             | name               | [LanguageString](#languagestring) | 0..* | aineiston nimi
laji             | type               | [LahtotietoaineistonLaji](#lahtotietoaineistonlaji) | 1 | aineiston tyyppi
aluerajaus       | boundary           | [Geometry](#geometry) | 0..*            | maantieteellinen alue, jota ainesto koskee
lisatietolinkki  | additionalInformationLink | [URI](#uri)  | 0..1            | viittaus ulkoiseen lisätietokuvaukseen asiakirjasta
metatietokuvaus  | metadata           | [URI](#uri)         | 0..1            | viittaus ulkoiseen metatietokuvaukseen

{% include common/note.html content="Lahtotietoaineisto-luokka ei kuvaa aineiston sisältöä, eikä ota kantaa tapaan, jolla sisältö noudetaan kaavatietovarastosta tai muusta tietojärjestelmästä. Nämä tiedot voidaan kuvata lähtötietoaineiston metatietokuvauksessa." %}


### Tapahtuma
Englanninkielinen nimi: **Event**

Erikoistaa luokkaa [VersioituObjekti](#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..* | tapahtuman nimi
tapahtumaAika    | eventTime          | [TM_Object](#tm_object) | 0..1        | tapahtuman aika (hetki tai aikaväli)
kuvaus           | description        | [LanguageString](#languagestring) | 0..* | tapahtuman tekstimuotoinen kuvaus
sijainti         | location           | [Geometry](#geometry) | 0..1          | tapahtumapaikka
lisatietolinkki  | additionalInformationLink | [URI](#uri)  | 0..1            | viittaus ulkoiseen lisätietokuvaukseen tapahtumasta
peruttu          | cancelled          | boolean = false     | 1               | onko tapahtuma peruttu (oletus: false)
liittyvaAsiakirja | relatedDocument      | [Liiteasiakirja](#liiteasiakirja) | 0..* | tapahtumaan liittyä asiakirja. 


**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsia     | relatedMatter      | [Alueidenkayttoasia](#alueidenkayttoasia) | 0..* | asia(n versio)t, joihin tapahtuma liittyy.



### Kasittelytapahtuma
Englanninkielinen nimi: **HandlingEvent**

Kuvaa käsitteen [Käsittelytapahtuma](../../kasitemalli/#käsittelytapahtuma), erikoistaa luokkaa [Tapahtuma](#tapahtuma), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiKasittelytapahtumanLaji](#abstraktikasittelytapahtumanlaji) | 1 | käsittelytapahtuman tyyppi

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
kasittelija      | handler            | [Organisaatio](#organisaatio) | 0..1  | tapahtuman vastuullinen toimija


### Vuorovaikutustapahtuma
Englanninkielinen nimi: **InteractionEvent**

Kuvaa käsitteen [Vuorovaikutustapahtuma](../../kasitemalli/#vuorovaikutustapahtuma), erikoistaa luokkaa [Tapahtuma](#tapahtuma), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiVuorovaikutustapahtumanLaji](#abstraktivuorovaikutustapahtumanlaji) | 1 | vuorovaikutustapahtuman tyyppi


## Kaavatiedot

### Kaava
Englanninkielinen nimi: **SpatialPlan**

Kuvaa käsitteen [Kaava](../../kasitemalli/#kaava), erikoistaa luokkaa [VersioituObjekti](https://tietomallit.ymparisto.fi/ry-yhteiset/v1.0/looginenmalli/dokumentaatio/#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
kaavalaji | type | [Kaavalaji](#kaavalaji)      | 1  | kaavan tyyppi
pysyväKaavaTunnus | spatialPlanId  | [URI](#uri) | 1               | kaavan yksilöivä ja pysyvä tunnus
kunnanKaavaTunnus | municipalSpatialPlanId | [URI](#uri) | 0..1 | kunnan antama kaavatunnus
elinkaaritila | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavan elinkaaren tila
kaavaKuvaus | planDescription    | [CharacterString](#characterstring) |0..1| sanallinen kuvaus kaavasta
maanalaisuus | groundRelativePosition | [MaanalaisuudenLaji](#maanalaisuudenlaji) | 0..1 | luokittelu maanalaista ja maanpäällistä maankäyttöä koskeviin kaavoihin
mittakaava | scale | [Int](#integer) | 0..1 | kertoo kaavan mittakaavan
kaavakartta | planMap | [Kaavakartta](#kaavakartta) | 0..* | viittaus kaavatietomallin mukaiseen kaavatiedostoon (geotiff)
aluerajaus | boundary | [Geometry](#geometry) | 0..* | kuvaa kaavan suunnittelualueen
yleiskaavanOikeusvaikutus    | legalEffectOfLocalMasterPlan |   | [YleiskaavanOikeusvaikutukset](#yleiskaavanoikeusvaikutukset) | 0..* | luokittelu yleiskaavan oikeusvaikutuksista
kaavanLiite    | spatialPlanAnnex   | [Liiteasiakirja](#liiteasiakirja) | 0..* | kaavaan olennaisesti kuuluva liitetty asiakirja
kumoamistieto    | cancellationInfo   | [KaavanKumoamistieto](#kaavanakumoamistieto) | 0..* | kaava tai sen osa, jonka tämä kaava kumoaa


**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
osallistumisJaArvointisuunnitelma | participationAndEvaluationPlan | [OsallistumisJaArviontiSuunnitelma](#osallistumisjaarviointisuunnitelma) | 0..1 | kaavan osallistumis- ja arviointisuunnitelma
selostus         | spatialPlanCommentary | [Kaavaselostus](#kaavaselostus) | 0..1          | kaavaselostus
laatija          | planner            | [KaavanLaatija]() | 0..* | kaavan laatimiseen tietyssä roolissa osallistunut suunnittelija
muuKaavaAineisto | otherPlanMaterial  | [MuuKaavaAineisto](#muukaavaaineisto) | 0..* | ei-asiakirjaksi luokiteltavat muut kaava-aineistot mm. videot, vaikutusten arvioinnit jne..
kaavakohde       | planObject         | [Kaavakohde](#kaavakohde) | 0..*       | paikkatietokohde, johon kohdistuu kaavamääräyksiä tai -suosituksia
kaavamääräys     | generalRegulation  | [Kaavamaarays](#kaavamaarays) | 0..*   | kaavamääräys, joka koskee koko kaavan aluetta
yleisMääräysJaSuositus    | generalRegulationAndGuidance    | [Kaavamääräysryhmä](#kaavamääräysryhmä) | 0..* | kaavan yleismääräys, yleissuositus tai niiden muodostama ryhmä, joka koskee koko kaavaan aluetta

### Alueidenkayttopaatos

Englanninkielinen nimi: **PlanUseDecision**

Erikoistaa luokkaa [VersioituObjekti](https://tietomallit.ymparisto.fi/ry-yhteiset/v1.0/looginenmalli/dokumentaatio/#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
paatospaivamaara | decisionDate | Date  | 0..1 | päätöksen päivämäärä
päätöksenantopäivämäärä | dateOfDecision | Date  | 0..1 | päätöksen antopäivämäärä, joka määrittää valitusajan sekä päätöksen lainvoimaisuuden sekä sitä kautta aloittamisoikeuden alkamisen.
lainvoimaisuuspäivämäärä | dateOfValidity | Date  | 0..* | päivämäärä, jolloin päätös tulee tai on tullut lainvoimaiseksi. Päivämäärä erityisesti tarvitaan, mikäli päätöstieto julkistetaan Ryhtiin ennen päätöksen lainvoimaisuutta.
päätösasiakirja | decisionDocument  | [Liiteasiakirja](#liiteasiakirja) | 0..* | päätökseen liittyvät asiakirjat
julkisuuspäivämäärä | dateOfPublicity |  Date | 1 | päätöksen julkisuuspäivämäärä
päätöspykälä  | decisionArticle | [LanguageString](#languagestring) | 1 | päätöksen pykälä kuntajärjestelmässä
julkaisemispäivämäärä  | dateOfPublication |  Date | 1 |päivämäärä, jolloin päätös on julkaistu. Päivämäärällä on merkitystä lainvoimaisuuden määrittämiseen. Päivämäärä erityisesti tarvitaan, mikäli päätöstieto Ryhtiin ennen päätöksen lainvoimaisuutta.
päätösteksti   | decisionText | [LanguageString](#languagestring) | 1 | päätöksen teksti sisältää varsinaisen päätöstekstin, perustelut, sovelletut oikeusohjeet.
nimi | name | [Paatoksennimi](#paatoksennimi) | 1 | päätöksen nimitys, joka kertoo mitä päätös koskee
päättäjänLaji | typeOfDecisionMaker | [Päätöksentekija](#paatoksentekija) | 1 | päätöksen tekijä, monijäseninen toimielin tai viranhaltija
päätösTunnus | decisionIdentifier |  [URI](#uri)  | 1 | päätöksen yksilöivä ja pysyvä, keskitetysti myönnetty tunnus.
ohjaavasäädös | guidingStatute | [Saadosviite](#saadosviite) | 0..1 | säädös, johon päätös perustuu.
voimassaoloaika | periodOfValidity | [TM_Period](#tm_period) | 0..1 | päätöksen voimassaoloaika

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asia        |   case   | [Alueidenkayttoasia](#alueidenkayttoasia)  | 1        | asia, johon päätös

### Saadosviite
Englanninkielinen nimi: **GuidingStatute**

Stereotyyppi: DataType (tietotyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
säädöksenNimi | nameOfStatute  |  [LanguageString](#languagestring) | 1 | säädöksen nimi
säädöksenKokoelmanNumero | documentIdentifier | [Int](#integer) | 1 | säädöskokoelman numero
säädöksenKokoelmanVuosi  | role | [Int](#integer) | 1 | säädöskokoelman vuosi
luku         | file  | [Int](#integer)  | 0..1 | säädöksen luku
pykälä         | file | [Int](#integer)  | 0..1| säädöksen pykälä
momentti         | file | [Int](#integer)  | 0..1 | säädöksen momentti
kohta         | file | [Int](#integer)  | 0..1 | säädöksen kohta
alakohta         | file | [CharacterString](#characterstring)  | 0..1 | säädöksen alakohta

### Kaavapäätös

Englanninkielinen nimi: **SpatialPlanDecision**

Kuvaa käsitteen [Kaavapäätös](../../kasitemalli/#kaavapäätös), erikoistaa luokkaa [Alueidenkayttopaatos](https://tietomallit.ymparisto.fi/ry-yhteiset/dev/looginenmalli/dokumentaatio/#alueidenk%C3%A4ytt%C3%B6p%C3%A4%C3%A4t%C3%B6s), stereotyyppi: FeatureType (kohdetyyppi)

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
kaava        |   spatialPlan   | [Kaava](#kaava)  | 1..*        | kaava, johon kohde kuuluu


### Kaavaselostus

Englanninkielinen nimi: **SpatialPlanCommentary**

Kuvaa käsitteen [Kaavaselostus](../../kasitemalli/#kaavaselostus), erikoistaa luokkaa [VersioituObjekti](https://tietomallit.ymparisto.fi/ry-yhteiset/v1.0/looginenmalli/dokumentaatio/#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Kaavaselostus on on Kaavatietomallin tässä versiossa kuvattu vain viittauksena kaavaselostuksen muodostaviin asiakirjoihin. Tulevissa tietomallin kehitysversiossa kaavaselostusta voidaan rakenteistaa pidemmälle tämän luokan avulla.

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
tiedosto        | file           | [Liiteasiakirja](https://tietomallit.ymparisto.fi/ry-yhteiset/dev/looginenmalli/dokumentaatio/#liiteasiakirja) | 0..*        | liiteasiakirja, joka on osa kaavaselostusta.


### OsallistumisJaArviointisuunnitelma

Englanninkielinen nimi: **ParticipationAndEvalutionPlan**

Kuvaa käsitteen [Osallistumis- ja arviointisuunnitelma](../../kasitemalli/#osallistumis--ja-arviointisuunnitelma), erikoistaa luokkaa [VersioituObjekti](https://tietomallit.ymparisto.fi/ry-yhteiset/v1.0/looginenmalli/dokumentaatio/#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Osallistumis- ja arviointisuunnitelma on on Kaavatietomallin tässä versiossa kuvattu vain viittauksena suunnitelman muodostaviin asiakirjoihin. Tulevissa tietomallin kehitysversiossa osallistumis- ja arviointisuunnitelmaa voidaan rakenteistaa pidemmälle tämän luokan avulla.

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
tiedosto        | file           | [Liiteasiakirja](https://tietomallit.ymparisto.fi/ry-yhteiset/dev/looginenmalli/dokumentaatio/#liiteasiakirja) | 0..*        | asiakirja, joka on osa kaavaselostusta. 


### Kaavakohde
Englanninkielinen nimi: **SpatialPlanObject**

Kuvaa käsitteen [Kaavakohde](../../kasitemalli/#kaavakohde), erikoistaa luokkaa [RakennetunYmpäristönKohde](#rakennetunympäristönkohde), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [RakennetunYmpäristönKohdelaji](#rakennetunympäristönkohdelaji) | 0..1 | varattu tulevaisuuden käyttöön
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavakohteen elinkaaren tila
liittyvanLahtotietokohteenTunnus | relatedInputDatasetObjectId | [URI](#uri) | 0..*    | viittaus kaavan lähtötietoaineistoon sisältyvään tietokohteeseen, joka liittyy kaavakohteeseen. Esim. pohjavesialue
voimassaoloaika  | validityTime       | [TM_Period](#tm_period) | 0..1        | maankäyttöasiasssa tehdyn päätöksen voimassaoloaika
maanalaisuus     | groundRelativePosition | [MaanalaisuudenLaji](#maanalaisuudenlaji) | 0..1 | luokittelu maanalaista ja maanpäällistä maankäyttöä koskeviin kaavakohteisiin
värikoodi        | colorCode | [CharacterString](#characterstring) | 0..1 | kunnan antama kaavakohteen värikoodi

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
kaava        |   spatialPlan   | [Kaava](#kaava)  | 1..*        | kaava, johon kohde kuuluu
määräysJaSuositus       | regulationAndGuidance         | [Kaavamaaraysryhmä](#kaavamaarays) | 0..*  | kaavamääräys, suositus tai niiden muodostama ryhmä, joka kohdistuu tämän kaavakohteen alueelle


### Kaavamaaraysryhmä

Englanninkielinen nimi: **SpatialPlanRegulationGroup**

Kuvaa käsitteen [Kaavamääräysryhmä](../../kasitemalli/#kaavamääräysryhmä), erikoistaa luokkaa [VersioituObjekti](https://tietomallit.ymparisto.fi/ry-yhteiset/v1.0/looginenmalli/dokumentaatio/#versioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
kaavamääräyksenOtsikko  | titleOfPlanRegulation | [CharacterString](#characterstring) | 0..1 | kaavamääräyksen otsikko
kirjainTunnus  | letterIdentifier | [CharacterString](#characterstring) | 0..1 | kaavamääräyksen kirjaintunnus


### Kaavamaarays

Englanninkielinen nimi: **SpatialPlanRegulation**

Kuvaa käsitteen [Kaavamääräys](../../kasitemalli/#kaavamääräys), erikoistaa luokkaa [Tietoyksikko](https://tietomallit.ymparisto.fi/ry-yhteiset/dev/looginenmalli/dokumentaatio/#tietoyksikk%C3%B6), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiKaavamaaraysLaji](#abstraktikaavamaarayslaji) | 1 | kaavamääräyksen luokittelu
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavamääräyksen elinkaaren tila
teema            | theme              | [AbstraktiKaavoitusteema](#abstraktikaaavoitusteema) | 0..* | kaavamääräyksen teemoittelu
lisatieto        | additionalInformation | [Lisatieto](#lisatieto)   | 0..*            | tarkentaa tai rajaa kaavamääräystä
voimassaoloAika  | validityTime       | [TM_Period](#tm_period)      | 0..1            | kaavamääräyksen lainvoimaisuusaika
aihetunniste  | subjectIdentifier | [CharacterString](#characterstring) | 0..* | kaavamääräyksen aihetunniste
sanallinenMääräys  | verbalRegulation              | [SanallisenMääräyksenLaji](#sanallisenmääräyksenlaji) | 0..* | kaavamääräyksen teemoittelu

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsiakirja | relatedDocument   | [Asiakirja](#asiakirja) | 0..*        | asiakirja, joka on perustelee kaavamääräystä tai liittyy siihen muulla tavalla. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa liittymistavan.

### Kaavasuositus

Englanninkielinen nimi: **SpatialPlanGuidance**

Kuvaa käsitteen [Kaavasuositus](../../kasitemalli/#kaavasuositus), erikoistaa luokkaa [AbstraktiTietoyksikko](#abstraktitietoyksikko), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
kuvaus             | name               | [LanguageString](#languagestring) | 0..*       | kaavasuosituksen sanallinen kuvaus
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavasuosituksen elinkaaren tila
teema            | theme              | [AbstraktiKaavoitusteema](#abstraktikaaavoitusteema) | 0..* | kaavasuosituksen teemoittelu
voimassaoloAika  | validityTime       | [TM_Period](#tm_period)      | 0..1            | kaavasuosituksen lainvoimaisuusaika
liittyväAsiakirja        | relatedDocument   | [Liiteasiakirja](https://tietomallit.ymparisto.fi/ry-yhteiset/dev/looginenmalli/dokumentaatio/#liiteasiakirja) | 0..*        | liiteasiakirja, joka on perustelee kaavasuositusta tai liittyy siihen muulla tavalla.

### Lisatieto

Englanninkielinen nimi: **SupplementaryInformation**

Kuvaa käsitteen [Lisätieto](../../kasitemalli/#lisätieto), stereotyyppi: DataType (tietotyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [LisatiedonLaji](#abstraktilisatiedonlaji) | 1 | lisätiedon luokittelu
nimi             | name               | [LanguageString](#languagestring) | 0..*       | lisätiedon tunnistamiseen käytettävä nimi. Huom: kaavan oikeusvaikutteiset nimeämiset (mm. katujen, teiden ja yleisten alueiden nimet ja korttelinumerot) kuvataan kaavamääräysten arvojen avulla.
arvo             | value              | [OminaisuudenArvo](#ominaisuudenarvo) | 0..*         | lisätiedon lajia tarkentava arvo


### KaavanKumoamistieto
Englanninkielinen nimi: **CancellationInfo**

Stereotyyppi: DataType (tietotyyppi)

Kumoamistieto yksilöi mitä kaavoja tai niiden osia kaava kumoaa lainvoimaiseksi tullessaan.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
kumottavanKaavanTunnus | cancelledPlanId | [URI](#uri)               | 1               | kaava, johon kumoaminen kohdistuu
kumoaaKaavanKokonaan | cancelsEntirePlan | boolean                   | 1               | jos arvo on ```true```, kumoaa kaavan kokonaisuudessaan, muuten muiden ominaisuuksien yksilöimällä tavalla
kumottavaKaavanAlue | areaToCancel    | [Geometry](#geometry)        | 0..1            | alue, jonka sisällä kokonaan olevia kaavamääräyksiä ja -suosituksia kumoaminen koskee
kumottavanKohteenTunnus | cancelledPlanObjectId | [URI](#uri)        | 0..*            | kaavakohteen ```viittausTunnus```, jota kumoaminen koskee
kumottavanMaarayksenTunnus | cancelledRegulationId | [URI](#uri)     | 0..*            | kaavamääräyksen ```viittausTunnus```, jota kumoaminen koskee
kumottavanSuosituksenTunnus | cancelledGuidanceId | [URI](#uri)      | 0..*            | kaavasuosituksen ```viittausTunnus```, jota kumoaminen koskee

### Koodistot

#### AsiakirjanLaji
Englanninkielinen nimi: **DocumentType**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_AsiakirjanLaji_YKAK" name="Asiakirjan laji (yleis- ja asemakaava)" %}

#### HenkilotietosisallonLaji
Englanninkielinen nimi: **personalDataContent**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="henkilotietosisalto" name="Henkilötietosisältö" %}

#### AsiakirjanJulkisuusluokka
Englanninkielinen nimi: **personalDataContent**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="julkisuus" name="Asiakirjan julkisuusluokka" %}

#### Paatoksennimi
Englanninkielinen nimi: **NameOfDecision**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="paatoksennimi" name="Päätöksen nimi" %}

#### Päätöksentekija
Englanninkielinen nimi: **DecisionMaker**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="PaatoksenTekija" name="Päätöksentekijä" %}


#### Kaavalaji
Englanninkielinen nimi: **SpatialPlanKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_Kaavalaji" name="Kaavalajit (maakunta-, yleis- ja asemakaava)" %}


#### KaavanElinkaaritila
Englanninkielinen nimi: **SpatialPlanLifecycleStatus**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavanElinkaaritila" name="Elinkaaren tila (yleis- ja asemakaava)" %}

#### Sitovuuslaji
Englanninkielinen nimi: **BindingnessKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_Sitovuuslaji" name="Sijainnin sitovuuden laji" %}

#### MaanalaisuudenLaji
Englanninkielinen nimi: **GroundRelativenessKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_MaanalaisuudenLaji" name="Maanalaisuuden laji" %}

#### AbstraktiKaavoitusteema
Englanninkielinen nimi: **AbstractSpatialPlanTheme**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

#### KaavoitusteemaAsemakaava
Englanninkielinen nimi: **DetailPlanTheme**

Erikoistaa luokkaa [AbstraktiKaavoitusteema](#abstraktikaavoitusteema), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

{% include common/codelistref.html registry="rytj" id="RY_Kaavoitusteema_AK" name="Kaavoitusteema (asemakaava)" %}

#### KaavoitusteemaYleiskaava
Englanninkielinen nimi: **MasterPlanTheme**

Erikoistaa luokkaa [AbstraktiKaavoitusteema](#abstraktikaavoitusteema), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

{% include common/codelistref.html registry="rytj" id="RY_Kaavoitusteema_YK" name="Kaavoitusteema (yleiskaava)" %}

#### Kaavamaarayslaji
Englanninkielinen nimi: **PlanRegulationKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

#### LisatiedonLaji
Englanninkielinen nimi: **AdditionInformationKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

#### SanallisenMääräyksenLaji
Englanninkielinen nimi: **typeOfVerbalRegulation**

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="" name="Sanallisen määräyksen lji (yleis- ja asemakaava)" %}

#### YleiskaavanOikeusvaikutukset
Englanninkielinen nimi: **legalEffectOfLocalMasterPlan**

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="oikeusvaik_YK" name="Elinkaaren tila (yleis- ja asemakaava)" %}

[ISO-8601-1]: https://www.iso.org/standard/70907.html "ISO 8601-1:2019 Date and time — Representations for information interchange — Part 1: Basic rules"
[ISO-639-2]: https://www.iso.org/standard/4767.html "ISO 639-2:1998 Codes for the representation of names of languages — Part 2: Alpha-3 code"
[ISO-19103]: https://www.iso.org/standard/56734.html "ISO 19103:2015 Geographic information — Conceptual schema language"
[ISO-19107]: https://www.iso.org/standard/66175.html "ISO 19107:2019 Geographic information — Spatial schema"
[ISO-19108]: https://www.iso.org/standard/26013.html "ISO 19108:2002 Geographic information — Temporal schema"
[ISO-19109]: https://www.iso.org/standard/59193.html "ISO 19109:2015 Geographic information — Rules for application schema"
[ISO-19505-2]: https://www.iso.org/standard/52854.html "ISO/IEC 19505-2:2012, Information technology — Object Management Group Unified Modeling Language (OMG UML) — Part 2: Superstructure"