# LADV Stammdaten Datenformat

> Stand: 20.04.2021 | Version: 1.0 FINAL

Das **Stammdaten Datenformat** dient zum Austausch von Stammdaten. Stammdatenbeinhalten Vereine,
Start- und Leichtathletik-Gemeinschaften, die Zusammensetzung derGemeinschaften und Athleten.
Typische Nutzer sind Meldeprogramme (wie LADV) undWettkampfprogrammen (wie COSA WIN).
Das Stammdaten Datenformat nutzt [XML](https://de.wikipedia.org/wiki/Extensible_Markup_Language)
und verwendet [XML-Schema](https://de.wikipedia.org/wiki/XML_Schema) zur Beschreibung der
Grundstruktur. Dieses Dokument beschreibt den Aufbau des Formats und Festlegungendie nicht in
XML-Schema abgebildet werden können.

Dieses Datenformat knüpft an ATL.TXT und VER.TXT aus den [DLV Austauschformaten](https://html.ladv.de/Leichtathletik-DLV-Austauschformate.pdf)
an und bietet einen XML basierten Ersatz für die nicht mehr Zeitgemäßen MS-DOS basiertenDatenformate.
Zudem sind im LADV Stammdaten Format neue Entwicklungen undAnforderungen der Leichtathletik
abgebildet: Gemeinschaften und ihre Zusammenstellungen werden mit bereitgestellt.

Das Datenformat verwendet englische Begriffe in Element und Attribut Namen um einen Einsatz
außerhalb des Deutschen Sprachraums zu ermöglichen.

## Änderungen

| Datum | # | Änderungen |
|---|---|---|
| 20.04.2021 | 1.0 | - GUID für Athleten ergänzt (optional)<br>- GUID für Vereine ergänzt (optional) |
| 24.01.2016 | 0.4 | - Dokumentation in dieses Dokument übertragen<br>- erste öffentliche Version |

## XML Schema

Zur Beschreibung des LADV Stammdaten Datenformates wird [XML Schema](https://www.w3.org/TR/xmlschema-0/) verwendet.

Die Aktuelle Schema Datei:

- https://html.ladv.de/format/structuredata/1.0/structuredata.xsd


Frühere Schema Datei(en):

- https://html.ladv.de/format/structuredata/0.4/structuredata.xsd

In diesem Dokument sind weitere, über das Schema hinausgehende Festlegungen für das Datenformat ausgeführt.

## Copyright

Das LADV Stammdatenformat ist Copyright Marc Schunk und LADV-Team.

## Struktur

```
<structuredata>     - Hauptelement
-<clubs>            - Gruppen Element für Vereine und Gemeinschaften
--<club>            - Vereine und Gemeinschaften
-<alliances>        - Gruppen Element für Gemeinschaften
--<alliance>        - Gemeinschaften und Ihre Zusammensetzung
---<alliancemember> - Mitglied einer Gemeinschaft
-<athletes>         - Gruppen Element für Athleten
--<athlete>         - Athleten
```

## Zeichenkodierung / Encoding

Alle Dateien im LADV Meldungen Format **müssen** [UTF-8](https://tools.ietf.org/html/rfc3629) kodiert sein. Alle
XML-Dokumente **müssen** die Kodierung deklarieren. Wie die Zeichenkodierung spezifiziert wird
ist teil desXML Standards. Beispiel:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<structuredata>
...
</structuredata>
```

## Status dieses Dokuments

Das LADV Stammdatenformat wird seit 2016 produktiv eingesetzt.

## Beschreibung der Elemente

Detailbeschreibung der Elemente und Attribute inkl. Beispiele.

### Element structuredata (Stammdaten/Strukturdaten)

#### Attribute

keine

#### Kindelemente

| Name | Beschreibung |
|------|--------------|
| clubs (club) | Vereine und Gemeinschaften |
| athletes (athlete) | Athleten |
| alliances (alliance) | Aufbau von Gemeinschaften |

### Element club (Vereine und Gemeinschaften)

#### Attribute

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| **id** | ID | Eindeutige ID des Elements |
| guid | string | GUID des Vereins |
| **name** | string | Name des Vereins |
| **shortname** | string | Kurname des Vereins. Maximal 20 Zeichen. |
| **number** | int | Vereinsnummer |
| **country** | string | Landeskennzeichen in [IOC Ländercodes](https://en.wikipedia.org/wiki/List_of_IOC_country_codes) |
| region | string | Landesverbandskennzeichen. Beispiele:<br>BA (für Baden)<br>BY (für Bayern) |
| **type** | string | Gibt an ob es sich bei dem club um einenVerein, eine Startgemeinschaft oderLeichtathletikgemeinschaft handelt. Gültige Werte: CLUB / STG / LG |

#### Kindelemente

keine

#### Beispiele

```xml
<clubs>
    <club id="C1" name="1. LAV Rostock" shortname="1. LAV Rostock" number="98"
      country="GER" region="MV" type="CLUB"/>
    <club id="C2" name="StG Hamburg Nord" shortname="StG Hamburg Nord" number="1234"
      country="GER" region="HH" type="STG"/>
    <club id="C3" name="LG Alb-Donau" shortname="LG Alb-Donau" number="3702" country="GER"
      region="WÜ" type="LG"/>
</clubs>
```

### Element athlete (Athleten)

#### Attribute

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| **id** | ID | Eindeutige ID des Elements |
| number | int | Athletennummer / Startpassnummer / Startrechtnummr |
| guid | string | GUID des Athleten |
| **lastname** | string | Nachname |
| **forename** | string | Vorname |
| **yearofbirth** | int | Geburtsjahr. Vierstellig |
| **sex** | string | Geschlecht. Mögliche Werte: M / W |
| country | string | Nationalität des Athleten in [IOC Ländercodes](https://en.wikipedia.org/wiki/List_of_IOC_country_codes) |

#### Kindelemente

keine

#### Beispiele

```xml
<athletes>
    <athlete id="A1" club="C1" country="GER" forename="Philipp" lastname="Steinberg"
       number="1234" sex="M" yearofbirth="2001"/>
    <athlete id="A2" club="C1" country="GER" forename="Andrea" lastname="Gabler"
       number="456789" sex="W" yearofbirth="1997"/>
</athletes>
```

### Element alliance (Gemeinschaft)

#### Attribute

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| **id** | ID | Eindeutige ID des Elements |
| **alliance** | IDRef | Referenz auf ID des Elements Club. Es sind nur Referenzen auf Clubs vom Type STG und LG zulässig. |

#### Kindelemente

| Name | Beschreibung |
|------|--------------|
| alliancemember | Mitglied einer Start- oder Leichtathletik-Gemeinschaft |

#### Beispiele

```xml
<alliances>
    <alliance id="AL4" alliance="C159">
        <alliancemember id="AM13" member="C143" membershiplist="AKTIVEU20U18U16U14"/>
        <alliancemember id="AM14" member="C147" membershiplist="AKTIVEU20U18U16U14"/>
    </alliance>
    <alliance id="AL26" alliance="C486">
        <alliancemember id="AM95" member="C51" membershiplist="U16U14M"/>
        <alliancemember id="AM96" member="C97" membershiplist="U16U14M"/>
    </alliance>
    <alliance id="AL27" alliance="C488">
        <alliancemember id="AM97" member="C130" membershiplist="SENIORENW"/>
        <alliancemember id="AM98" member="C453" membershiplist="SENIORENW"/>
    </alliance>
</alliances>
```

### Element alliancemember (Mitglied einer Gemeinschaft)

#### Attribute

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| **id** | ID | Eindeutige ID des Elements |
| **member** | IDRef | Referent auf ID aus dem Element Club. Es sind nur Clubs vom Type VEREIN zulässig. |
| **membershiplist** | Liste von strings | Liste aus Mitgliedschafts konstanten. Siehe Referenz Mitgliedschaftskonstanten. |

#### Kindelemente

kiene

#### Beispiele

siehe Element alliance (Gemeinschaft)

### Referenz membership (Mitgliedschaftskonstanten)

Die folgende Liste an Konstanten bildet die Regelungen aus §2 der Deutschen Leichtathletik Ordnung
"Leichtathletik-Gemeinschaften (LG) und Startgemeinschaften (StG)" ab.

#### Konstanten für Leichtathletik-Gemeinschaften (LG)

| Konstante | Beschreibung |
|-----------|--------------|
| AKTIVEU20U18 | Männer, Frauen und Jugend U20, U18 |
| U16U14 | Jugend U16 und U14 |
| U12 | Kinder U12 (nicht in DLO enthalten, Regelungen diverserLandesverbände) |
| U10 | Kinder U10 (nicht in DLO enthalten, Regelungen diverserLandesverbände) |
| U8 | Kinder U8 (nicht in DLO enthalten, Regelungen diverserLandesverbände) |

#### Konstanten für Startgemeinschaften (StG)

| Konstante | Beschreibung |
|-----------|--------------|
| U16U14W | weibliche U16/U14 |
| U16U14M | männliche U16/U14 |
| U20U18W | weibliche U20/U18 |
| U20U18M | männliche U20/U18 |
| AKTIVEW | Frauen/Juniorinnen |
| AKTIVEM | Männer/Junioren |
| SENIORENW | Seniorinnen |
| SENIORENM | Senioren |
