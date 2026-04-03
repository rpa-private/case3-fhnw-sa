## Problemfall 3

### Enterprise Messaging

### FHNW, Marc Schaaf - VL Software Architecture

##### FS


### 1 Motivation und Szenario

Die Accolaia AG möchte gerne ein neues Konzept für die Disposition von Aussendienstaufträgen realisie-
ren, welches statt der bisher statischen Planung des Aussendiensteinsatzes durch die Dispositionsab-
teilung eine zumindest teilweise dynamische Auftragsannahme durch die Aussendienstmitarbeitenden
vorsieht. Hierfür soll zwischen Routinewarungsarbeiten und dringenden Reparatureinsätzen unter-
schieden werden. Dringende Einsätze werden weiterhin von der Dispositionsabteilung koordiniert, die
zeitlich unkritischen Routinearbeiten sollen durch die Aussendienstmitarbeitenden selbst koordiniert
werden. Hierzu werden Aussendienstmitarbietende zeitnah über nötige Routineeinsätze informiert. Die
Aussendienstmitarbeitenden können diese dann selbstständig je nach ihrer aktuellen zeitlichen Ver-
fügbarkeit annehmen, beispielsweise wenn ein vorheriger Auftrag unerwartet schnell abgeschlossen
werden konnte.

```
Im Detail ist der Prozess wie folgt vorgesehen:
```
1. Die Dispositionsabteilung erhält einen neuen Aussendienstauftrag von durch das Callcenter.
    - Der Auftrag beinhaltet bereits alle relevanten Informationen inkl. der Klassifikation, ob es
       sich um einen dringenden Einsatz oder einen Routineauftrag handelt.
2. Je nach Typ des Auftrags:
    - Dringender Reparaturauftrag: Die Dispositionsabteilung kooriniert den Einsatz direkt.De-
       tails zur Koordination dieser Aufträge sind nicht Teil der Problemstellung.
    - Routineauftrag: Die Dispositionsabteilung publiziert den Auftrag an die Aussendienstmitar-
       beitenden.
3. Aussendienstmitarbietende werden zeitnah über den neuen Auftrag informiert.
    - Damit die Mitarbeitenden nicht zu viele derartige Benachrichtigungen erhalten, können
       Sie Filter definieren (z. B. für die aktuelle Region).
4.Wenn sich eine(r) der Aussendienstmitarbeitenden für den Auftrag entscheidet, informiert diese(r)
    die Dispositionsabteilung, welche die (nicht) erfolgreiche Zuteilung des Auftrags rückbestätigt.
       - Wenn mehrere Mitarbeitende den gleichen Auftrag anfordern, entscheidet die Dispospositi-
          onsabteiligung.
       - Bei erfolgreicher Vergabe soll zudem der Auftrag bei den anderen Aussendienstmitarbei-
          tenden nicht mehr angezeigt werden.
Hierbei enthält ein Auftrag vereinfacht die folgenden Informationen:
- Auftragsnummer
- Kundennummer


- Kurzbeschreibung
- Typ (Wartung, Reparatur)
- Datum / Urzeit des geplanten Einsatzes
- Region
- Ausführede(r) Aussendienstmitarbeite(r)

#### 1.1 Rahmenbedingungen:

```
1.Die IT-Abteilung hat bereits Apache ActiveMQ^1 evaluiert und als Unternehmensstandard bereit-
gestellt (Zugangsdaten auf Moodle).
2.Die Mitarbeitenden im Aussendienst haben spezielle Tablet-Computer, auf welche vom Unter-
nehmen verwaltet werden und entsprechend Java-Applikationen deployed werden können.
Zudem verfügen diese mobilen Geräte über Internetzugang und sind mittels VPN an das interne
Unternehmensnetz angebunden.
```
3. Um die Problemstellung zu vereinfachen:
    - Kunden-, Auftrags- und Mitarbeitendennummern müssennichtmit anderen Systemen
       abgeglichen werden.
    - Details zur Koordination der dringenden Reparatureinsätze sind nicht Teil der Problemstel-
       lung.
    - Eine Anbindung einer Android/iOS App istnichtangedacht.

#### 1.2 Ergänzende Hinweise

Da sie bei ihrer bisherigen Tätigkeit noch keine Erfahrung in diesem Bereich der Anwendungsinte-
gration haben, haben sie sich in ihrem beruflichen Bekanntenkreis ein paar erste Hinweise für die
Umsetzung geholt. Aus diesen Gesprächen haben sie erfahren, das es Sinn machen könnte, in diesem
Fall nicht auf eine RPC orientierte Middleware zu setzen, sondern stattdessen eine Message Oriented
Middleware einzusetzen. Speziell soll die passende Nutzung von “Topics” und “Queues” eine Lösung
ermöglichen.

(^1) https://activemq.apache.org/


# 

```
Bitte nutzen Sie zum Entwurf und zur Beschreibung des Lösungskonzeptes die Enterprise
Integration Patterns und die dort vorgestellte Notation. Als Startpunkte können die
folgenden Patterns & Konzepte aus dem de facto Standardwerk “Enterprise Integration
Patterns: Designing, Building, and Deploying Messaging Solutions” von Gregor Hohpe
und Bobby Woolf dienen:
```
1. Message
2. Message Channel
3. Point-to-Point Channel
4. Publish-Subscribe Channel
5. Message Endpoint
6. Message Router / Content-Based Router
7. Competing Consumers
8. Durable Subscriber

```
Die EIP-Notation wird auch durch das Zeichentool “Draw.io” unterstützt.
```
Für diesen Problemfall klammern wir das Thema Enterprise Service Bus noch aus, da wir dies in einem
separaten Problemfall behandeln.

### 2 Zielsetzung

Entwerfen Sie ein Lösungskonzept für das oben beschriebene Szenario und realisieren Sie das Konzept
als einen technischen Durchstich, welcher die zentralen Funktionen demonstriert.


# 

```
Zentrale Lernziele für diesen Fall:
```
- Grundlegendes Verständnis sowie Anwendung von Enterprise Messaging bzw.
    Asynchronous-Messaging, Message Oriented Middleware und ausgewählten Enter-
       prise Integration Patterns.
- Verstehen des Begriffs Middleware im Zusammenhang zu Enterprise Messaging
    sowie des Konzeptes des Asynchronous-Messaging.

```
Sowie das Trainieren von Selbststudienfähigkeiten und Lösungsorientiertem Denken.
```
### 3 PBL Prozess

Bitte folgen Sie der in der Vorlesung vorgestellten Methodik zum Design-oriented Problem Based
Learning.

#### 3.1 Phase 1: Definition der eigenen Lernziele

1. Lesen Sie die Problemfallbeschreibung gründlich.
2.Definieren Sie, wer innerhalb der Gruppe als Moderator/Schriftführer für diesen Problemfall
    agiert.
3. Definieren Sie die Lernfragen für das Selbststudium.

**Abgabe: Protokoll der Sitzung inkl. der erarbeiteten Lernfragen** (Vorlage auf Moodle).

#### 3.2 Phase 2: Selbststudium

1. Basierend auf den Lernfragen recherchieren und bereitgestelltes Material durcharbeiten.
    - Startpunke für die Recherche finden Sie in Form von Videos und verlinkten Dokumenten auf
       Moodle. Neben diesen sollten Sie auch nach weiteren vertrauenswürdigen Quellen suchen.
2. Zusammenstellen der Resultate für die kommende Besprechung mit der Gruppe
    - Zwischenresultate in eigenen Worten im persönlichen Learning Report dokumentieren
       (unter Angabe der zugehörigen Quellen).

#### 3.3 Phase 3: Zusammenführen der Selbststudienergebnisse und Design/Umsetzung der

#### Problemlösung

1. In der Gruppe die Resultate des individuellen Selbststudiums zusammentragen.


- Unterschiedliche Funde/Auffassungen besprechen und einordnen.
- Einheitliches Verständnis schaffen.
- Lernfragen beantworten und Antworten dokumentieren.
2. Lösungsstrategie definieren.
3. Coaching-Besprechung bezüglich der zentralen Konzepte und des weiteren Vorgehens.
4. Lösung entwerfen.
5. Finalisieren des Lösungskonzeptes und Umsetzung der Lösung sowie dessen Dokumentation.
6.Finalisieren des Artefakts/der Dokumentation und Abgabe auf Moodle(durch Moderator).

**Abgaben auf Moodle zum Abschluss der Phase 3:**

- Antworten auf die Lernfragen.
- Lösungskonzeptmit nachvollziehbarer Begründung.
- Lösungsumsetzung.

#### 3.4 Phase 4: Reflektion

1. Bewertung des Problemfalls und des PBL-Prozesses auf Moodle
2. Reflektieren des Problemfalls und dessen Bearbeitung.
3.Eigenen Lernbericht um eine kurze und prägnante Zusammenfassung der zentralen Erkenntnisse
    / des gelernten aus dem Problemfall bzw. dessen Bearbeitung ergänzen


