# Strategie für das Softwareökosystem der Open Telekom Cloud

Die Open Telekom Cloud bietet Anwendern durch ihre umfangreichen Services viele Möglichkeiten, um Applikationssetups flexibel und skalierbar zu gestalten und zu betreiben. Zur Verwaltung und Orchestrierung dieser Setups gibt es mehrere Optionen:

1. **Browserbasierte Benutzeroberfläche (UI):** Die UI ermöglicht den Zugriff auf die verwalteten Ressourcen der Services über einen Browser. Dies ist anfangs intuitiv, skaliert jedoch nur bedingt, da langfristig viele Ressourcen häufig wiederkehrend verwaltet werden müssen. Eine umfassende Automatisierung der Verwaltungsaufgaben ist auf diese Weise nur eingeschränkt möglich.

2. **API-Steuerung:** Jeder Service der Open Telekom Cloud ist über eine API zugänglich und steuerbar, was es den Anwendern ermöglicht, die Steuerung dieser Schnittstellen zu automatisieren. Die Schnittstellen sind meist als REST-Endpoints realisiert, über die JSON-codierte Datenobjekte ausgetauscht werden. Dieser Weg ist sehr generisch, jedoch auch kleinschrittig und umfangreich.

3. **Verwaltungswerkzeuge und Orchestrierungsplattformen:** Diese Tools kombinieren beide Ansätze und bieten aufgabenbezogene Verwaltungsfunktionen, um Setups in der Cloud aufzusetzen, zu verwalten, zu betreiben und zu überwachen. Beispiele sind Infrastructure-as-Code-Plattformen wie Terraform, Ansible oder Chef sowie Orchestrierungswerkzeuge für Cluster wie Kubernetes, Rancher oder Nomad. Diese Werkzeuge bestehen aus einem allgemeinen Teil und einem Connector, der die Software über das API an die spezifische Cloud anbindet.

Das gesamte Set an Tools und Werkzeugen, die zur Verwaltung einer Cloud verwendet werden können, bezeichnen wir als das Ökosystem der Open Telekom Cloud. Die **Ecosystem Squad** der Open Telekom Cloud ist das Team, das sich um die Anbindung dieser Werkzeuge kümmert, das Angebot sichtet, testet und sich in ausgewählten Fällen um die Anbindung und Pflege dieser Werkzeuge bemüht.

Angesichts der Vielzahl an Verwaltungswerkzeugen und Plattformen sowie des Anpassungsbedarfs an alle oder möglichst viele Dienste ist eine Priorisierung der unterstützten Angebote notwendig. Diese Priorisierung basiert auf zwei Kernaspekten:

1. **Markt- und betriebswirtschaftliche Überlegungen:** Sie untersuchen, ob Bedarf für ein bestimmtes Werkzeug besteht, wie hoch die Nachfrage danach ist und wie es zum restlichen Angebotsportfolio der Open Telekom Cloud passt.

2. **Architektonische Analysen:** Diese beurteilen die technische Belastbarkeit, Kompatibilität und Eignung einer Lösung für typische Produkte. Dabei wird auch die verwendete Programmiersprache, die benötigte Ablaufumgebung und weitere technische Überlegungen betrachtet.

Dieses Strategiedokument enthält zwei zentrale Untersuchungen: Zunächst werden die typischen und verbreiteten Infrastrukturmanagementlösungen zusammengetragen, die unter einer Open-Source-Lizenz stehen und plattformunabhängig sind, sodass sie mit der Open Telekom Cloud genutzt werden können. Anschließend bewertet Dokument den Nutzen dieser Lösungen aus der Kunden- udn Anwenderperspektive in technischer Hinsicht. Diese Bewertungen dienen dem Produktmanagement als Entscheidungsgrundlage, um die Produkte auszuwählen, die im Rahmen des Implementierungsprozesses durch die Ecosystem Squad an die Open Telekom Cloud angepasst werden sollen.

## Kategorien von Cloud-Plattform-Management-Lösungen

Diese Tools sind plattformunabhängig, stehen unter einer Open-Source-Lizenz und bieten die Möglichkeit, Cloud-Umgebungen zu managen und zu orchestrieren.

### Infrastructure as Code und Configuration Management

Diese Werkzeuge verwalten die von der Cloud bereitgestellten Ressourcen direkt. Das Erzeugen, Anordnen und In-Beziehung-Setzen dieser Ressourcen wird als Provisionierung bezeichnet. Eine wesentliche Aufgabe dieser Tools ist der Abgleich zwischen Soll- und Ist-Zustand. Anwender beschreiben den gewünschten Zielzustand, und das Werkzeug kümmert sich um die Identifikation und die korrekte Reihenfolge oder Parallelität der dazu auszuführenden Schritte. Eine weitere wichtige Funktion ist das Zustandsmanagement der betreuten Clouds. Sollte die Vorstellung des Werkzeugs vom Vorhandensein bestimmter Ressourcen von der Realität abweichen, spricht man von Config-Drift. Diese Drift sollte ein Werkzeug erkennen, melden und auflösen können.

#### Infrastructure as Code

IaC-Werkzeuge (Infrastructure as Code) provisionieren Cloud-Ressourcen. Dabei abstrahieren sie meistens die konkreten APIs der jeweiligen Cloud-Plattformen, um so mehr oder weniger einfach mit einer Beschreibung Setups auf mehreren Plattformen  provisionieren zu können, um damit einen gewissen Grad an Multi-Cloud-Fähigkeit herzustellen. Die wichtigsten vertreter dieser Kategorie sind:

**Terraform:** Automatisierung und Orchestrierung von Multi-Cloud-Infrastrukturen.

Terraform gilt als technologische Markt- und Innovationsführer im Bereich IaC. Die Unterstützung von Terraform ist für jede Cloud heute praktisch obligatorisch, insbesondere für Anwender, die Cloud-native Umgebungen schnell und einfach provisionieren möchten. Terraform verwendet eine leicht zu erlernende, domänenspezifische Sprache, die einzelne Ressourcen definiert und ihre Beziehungen beschreibt. Die tatsächliche Reihenfolge berechnet Terraform in einer Planungsphase und führt alle erforderlichen Schritte aus. Die Bedeutung von Terraform zeigt sich auch darin, dass mittlerweile viele Planungstools Terraform-Templates als Ausgabeformat nutzen.

Terraform selbst stand lange unter der Mozilla-Lizenz MPL 2.0, bis HashiCorp im August 2023 die Lizenz auf die nicht OSI-konforme BSL 1.1 abänderte. Dies hat für Anwender zwar zunächst nur geringe Auswirkungen, doch könnte dieser Schritt langfristige rechtliche Auswirkungen haben.

Um auf den Lizenzwechsel zu reagieren, wurde das **OpenTOFU-Projekt** gegründet, das sich zum Ziel gesetzt hat, Terraform-kompatibel zu bleiben und bestehende Terraform-Provider weiter zu nutzen. Inwiefern dies dauerhaft gewährleistet bleibt, muss abgewartet werden. Solange die Auswirkungen des Lizenzwechsels gering bleiben, bleibt der Fokus der Ecosystem Squad zunächst beim klassischen Terraform.

Die Ecosystem Squad hat den Terraform-Provider für die Open Telekom Cloud entwickelt und betreut dessen Weiterentwicklung gemeinsam mit der Terraform-Community auf GitHub. Er steht unter der Mozilla MPL-Lizenz.

Terraform hat gegenwärtig die höchste Priorität unter den unterstützten Plattformen der Ecosystem Squad, um die Nachfrage der Anwender zu erfüllen und das Cloud-Enablement der Kunden zu unterstützen. Mittelfristig ist ein Ausbau der Ressourcen nötig, um den Support zu gewährleisten und das Angebot zu erweitern, etwa durch zusätzliche Dokumentation und Trainingsmaterialien.

**Ansible:** Automatisierung und Konfigurationsmanagement für Infrastruktur und Applikationen.

Ansible ist ursprünglich als Konfigurationsmanagement-Tool entstanden und gehört zu den bekanntesten und verbreitetsten Werkzeugen seiner Art. Es wird in Python entwickelt, was es flexibel, aber teils auch weniger performant macht.

Ansible ermöglicht die Definition von Ressourcen über YAML-Dateien, abgelegt in einer komplexen Verzeichnisstruktur, und führt anschließend die erforderlichen Schritte zur Zielerreichung aus. Ansible-Playbooks bieten eine Versionierbarkeit, die es ermöglicht, komplexe Infrastruktur- und Applikationslösungen zu definieren.

Durch die Integration von Ansible in Apimon und den CI-Server Zuul hat die Unterstützung von Ansible Collections großen Einfluss auf die internen Abläufe der Ecosystem Squad.

Ansible ist unter der GPL lizenziert, die Collections sind unter der Apache License 2.0 veröffentlicht.

**Pulumi:** Programmiersprachenbasierte IaC-Lösung für Multi-Cloud-Infrastrukturen.

Pulumi hat strukturell einen sehr ähnlichen Ansatz wie Terraform und kann fast als eine Greenfield-Reimplemtierung angesehen werden. Der wichtigste Unterschied besteht in der Sprache der Domain Specific Language, denn hier haben Anwender deutlich mehr Auswahl, da bekannte Programmiersprachen wie Typescript, Python, Go oder sogar Java möglich sind. Sie haben dann auch die Möglichkeiten, auch imperative Konstrukte wie Schleifen oder Bedingungen in ihren IaC-COde zu integrieren. Dadurch ist Pulumi ein sehr modernes und mächtiges Werkzeug. Dennoch hat es nur eine eingeschränkte Verbreitung gefunden.

Die Ecosystem Squad hat sich bislang nicht dazu entschlossen Pulumi aktiv zu unterstützen.

**Juju:** Tool von Canonical zur Verwaltung von Anwendungen in der Cloud.

Zusammen mit dem Infrastrukturmanager MaaS ist Juju ein komplexes, integriertes Werkzeug die Bereitstellung, Verwaltung und Orchestrierung von Anwendungen. Es ist hat Stärken in der Verwaltung von komplexen, verteilten Applikationen, die aus mehreren Komponenten bestehen und über verschiedene Cloud- oder Serverumgebungen verteilt sind. Die Kombination von Juju/MaaS wird vom Ubuntu-Hersteller Canonical entwickelt, hat aber außerhalb dieses Ökosystems nur eine sehr geringe Verbreitung, da es viele Ansätze des sonst üblichen Stacks von Terraform und Ansible sozusagen "proprietär" und inkompatibel realisiert.

Die Ecosystem Squad hat sich bislang nicht dazu entschlossen, Juju aktiv zu unterstützen.

**TOSCA:** Offenes Standardformat für die Modellierung von Infrastruktur.

Die Topology and Orchestration Specification for Cloud Applications (TOSCA) ist ein offener OASIS-Standard, der eine einheitliche Sprache zur Modellierung und Verwaltung komplexer Cloud-Anwendungen und -Infrastrukturen definiert. Ziel von TOSCA ist es, die Portabilität und Interoperabilität zwischen verschiedenen Cloud-Umgebungen zu ermöglichen, indem Anwendungen und deren Abhängigkeiten standardisiert und als wiederverwendbare Module definiert werden. TOSCA kann als eine Meta-Abstraktion der in diesem Abschnitt beschriebenen Prinzipien verstanden werden. Es ist auch kein Werkzeug an sich, sondern nur eine Sprachdefinition, die dann mit weiteren Werkzeugen zusammenarbeitet, etwa einem Modeller, der mit einem UI die Komponenten zusammenstellt und einem Orchestrator, der die Beschreibungen in unmittelbar prozessierbare Templates übersetzt, zum Beispiel nach Terraform oder Ansible.

Durch den hohen Abstraktionsgrad von TOSCA ist es notwendig, eine komplette Anwendungsumgebung vorzuhalten, die die einzelnen Komponenten integriert. Dies geht über die Ressourcen der Ecosystem Squad hinaus, wird aber von der Cloud Create Squad als Komplettanwendung angeboten.

### Cluster- und Containerorchestrierung

Neben virtuellen Maschinen spielen insbesondere Container als eine leichtgewichtigere Alternative bei modernen Cloud-Native-Setups eine immer stärkere Rolle, oft im Zusamenhang komplexerer Anwendungssetup die nach dem Konzept der Microservices zusammengestellt sind. Nachdem VM und COntainer an sich nur einen einzelnen Host als Ablaufplattform betrachten, adressieren Orchestrieungslösungen die Rollen und Beziehungen solcher VMs und Conatiner untereinander, etwa in Form von Clustern aus redundanten Knoten, die im laufenden Betrieb horizontale Skalierung ermöglichen: 

**Kubernetes:** Containerorchestrierung und Cluster-Management.

Zwischenzeitlich gilt Kubernetes als unbestrittener Kernbaustein der meisten Orchestrierungsplattformen (Plattform as a Servce, PaaS). Das ursprunglich von Google entworfene Framework realisiet eine Zahl von abstrakten Plattform-Ressourcen und verteilt sie transparent auf vorhandene Infrastrukturkomponenten (Nodes). Dabei bringt Kubernetes die Datenstruktur der Custom Resource Definitions (CRDs) mit, einen universellen Weg, um gewisse Grundbausteine von Clustern zu definieren und zu verwalten. Damit lasssen sich beispielsweise skalierbare Redundanzcluster realisieren, Scale-in und Scale-out realisieren oder Failover-Szenarien aufbauen. Für Anwendungsentwickler gilt Kubernetes heute als generalisierte Ablaufumgebung für verteilte, cloud-native-Anwendungen.

Dabei kann Kubernetes mit mehreren IaaS-Implementationen von Netzwerken (CNI), Storage-Providern (CSI) und weiteren Realisationene umgehen. Kubernetes lässt sich entweder unmittelbar auf IaaS-Ressourcen der Open Telekom CLoud aufsetzen (zum Beispiel auch mit Hilfe der beschriebenen Provisinierungswerkzeuge) oder in Form des vorhandenen Dienstes Cloud Container Engine (CCE) der Open Telekom Cloud nutzen. Insofern ist Kubernetes auch ohne konkrete Supportkomponente durch die Ecosystem Squad nutzbar.

**Rancher:** Plattform für Multi-Cluster-Kubernetes-Umgebungen.

Da die auf Kubernetes laufenden Setups meistens eine gewisse Komplexität besitzen und aus mehreren Komponenten bestehen, aber Kubernetes selbst nur ein begrenztes UI mitbringt, füllen Verwaltungswerkzeuge wie Rancher diese Lücke. Sie sind in der Lage Kubernetes-Cluster auf Grundlage eines CLusterdienstes wie dem CCE der open Telekom CLoud oder auf basis einer abstrahierten Virtuelen-NMaschine (Node-Driver) aufzusetzen, zu verwalten, und zu monitoren.

Die Ecosystem Squad bietet beide Formen von Drivern für die Open Telekom CLoud als Proof-of-Concept an. Rancher hat dabei eine gewisse Marktdurchdringung erreicht und wird vor allem aus pragmatischen Gründen eingesetzt, da es funktional keinen sehr großen Mehrwert zum verwalteten, eigentlichen Cluster bietet.

**OpenShift:** Umfangreiche Plattform für Cluster-Umgebungen

Der architektonische Ansatz von OpenShift ist vergleichbar zu dem von Rancher, wenn auch die von Red Hat als Plattform angebotene Software noch umfangreicher ist, weil neben deriegentlichen Ablaufplattform noch viele weitere Zusatzkomponenten mitbringt, darunter eine Bibliothek und Verwaltrung von eigenen Custom Resources, etwa für Datenbanken und andere Microservices.

Das Provisionieren von OpenShift-Clustern ist eine komplexe Aufgabe und erfordert die die Verfügbarkeit von einer ganzen Reihe von Custom Resources. Gegenwärtig gibt es für die Open Telekom CLoud nur eine Provisionierung auf der Grundlage von einer vorgegebenen Topologie, die für das Werkzeugt von Cloud Create der gleichnammigen Squad vorliegt.

**Nomad:** Orchestrierung für Container, VMs und mehr.

Nomad ist eine CLusterplattform von Hashicorp und kann als eine Kernfunktion und alternative Implementation von Kubernetes angesehen werden, die aber nur eine geringe Verbreitung gefunden hat.

Nomad wird aktuell nicht von der Ecosystem Squad angeboten.

### Ressource-Management-Plattformen

Vielen Anwendungsentwicklern für Cloud-Setups kämpfen mit den Abstraktionsebenen IaaS und PaaS für ihre Anwendungen. Wer auf der IaaS-Ebene über ihre APIs auf die Services der Open Telekom Cloud zugreift, muss ein anderes Zugriffsparadigma verwenden als er es auf der PaaS-eben von Kubernetes gewohnt ist. Daher geht ein Trend dahin die Ressourcen der IaaS-ebene quasi in Objekte der PaaS-ebene einzupacken. Diese Hüllen sind die Custom Resource Definitions (CRDs). Von ihnen gibt es unterschiedliche Sätze, die verschiedene Schwerpunkte adressieren:

**Cloud COntainer Management (CCM):** Anbindung der Ressourcen von Core-Services

Eine CCM-Implementation gibt ein Set von häufig benötigten CRDs vor, das IaaS-Clouds meist in der einen ooder anderen Form bereits bereitstellen.
xxx wo sind wir hier mit der OTC? xxx

**Crossplane:** xxx wie oben, aber für mehr servcies
xx öhnlcihes prinzip wie CCM, aber weiter gefasst und über die core services hinausgehend. würde umfangreiche realisierung benötigen.
