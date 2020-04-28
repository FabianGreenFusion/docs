atscm Tutorial / Atvise-Git-Workflow
========================
[atscm-cli](https://github.com/atSCM/atscm-cli) ("Atvise Source Code Management/Command Line Interface") ist ein Programm von [Bachmann electronics](https://www.bachmann.info/home/).
Es extrahiert die Daten ("displays, serverside scripts and quickdynamics will be split into their JavaScript and SVG sources") aus der binären SQLite-Datenbank von Atvise ("nodes.db").
Diese Quelldateien lassen sich mit git versionieren.
[atSCM bei Git](https://github.com/atSCM)
[Dokumentation](https://atscm.github.io/latest)

Installing node.js
-------------------------------
atscm basiert auf node.js. Es muss also zunächst ein node.js-Server installiert werden:
https://nodejs.org/dist/v12.16.1/node-v12.16.1-x64.msi

Installing atscm
-------------------------------
Bei node.js sollte der Paket-Manager "npm" (node.js packet manager) dabei gewesen sein.
Diesen nutzen wir zur Installation von atSCM:
`npm install --global atscm-cli`

Installing Green Fusion repository
-------------------------------
An einem beliebigen lokalen Ort wird nun das atscm-Projekt angelegt:
`C:\Workspace\Atvise-Source>atscm init`

Innerhalb dieses Projekts wird nun unser repository plaziert:
`C:\Workspace\Atvise-Source>git clone https://github.com/GreenFusion/nodes.git src`

Jetzt ist die Arbeitsumgebung bereit:
* atscm wird genutzt, um die Daten von der nodes.db in den src-Ordner zu schreiben (atscm pull) und umgekehrt (atscm push)
* git wird genutzt, um die Daten vom src-Ordner in unser repository zu schreiben (git commit, git push) und umgekehrt (git pull)

Workflow
-------------------------------
### Example 1: Updating the project
_Alle eigenen Änderungen sind eingecheckt_
* git pull wird ausgeführt
* atscm push wird ausgeführt
* Die neueste Version ist sofort im Browser sichtbar
* TODO: Muss jetzt im Builder ein Update angestossen werden?
### Example 2: Commiting a change
_Das repository ist auf dem neuesten Stand_
* Im Builder werden beliebige Änderungen vorgenommen
* atscm pull wird ausgeführt
* git commit wird ausgeführt
* git push wird ausgeführt
#### So sieht das dann aus
Auf dem branch [test](https://github.com/GreenFusion/nodes/commits/test) habe ich mal eine kleine Änderung hochgeladen:
https://github.com/GreenFusion/nodes/commit/4d42b95b7fa1a342538acf0e116335f5fb910f90

Automating the workflow
-------------------------------
atscm bringt ein "Überwachungs"-Tool mit, welches Änderungen in der Datenbank erkennt und diese exportiert.
Damit habe ich noch nicht herumexperimentiert.
Mit git ist es über "hooks" (Skripte die von git bei bestimmten Operationen gestartet werden) möglich nach einem Auschecken (updaten, pullen) des repositorys atscm anzuschmeissen.
Für den Anfang lässt sich das aber auch mit einer einfachen Batch-Datei lösen.
Iich würde sogar empfehlen, die Schritte zunächst immer händisch auszuführen um ein Gefühl für die Datenflüsse zu entwickeln. Denn: Ich weiss noch nicht, wie ausgereift die merge-Fähigkeiten von atscm sind. **Gleichzeitige Arbeit an der Datenbank sollten wir trotz git zunächst vermeiden!***

FAQ / Debugging
===============

## Error:  cannot locate certificate file C:\Workspace\Atvise-Source\node_modules\node-opcua\certificates\client_selfsigned_cert_1024.pem
Im Ordner `node_modules\node-opcua-crypto\test\fixtures\certs\` befinden sich Demo-Zertifikate. Mit diesen ist es mir möglich zu pullen. Ich bin aber unsicher, ob die Lösung gut war, denn ich kann derzeit nicht pushen.

## Error: Error creating node AGENT: Script SYSTEM.LIBRARY.ATVISE.SERVERSCRIPTS.atscm.CreateNode was not found.
Diesen Fehler kriege ich aktuell beim Versuch `atscm push`auszuführen.
https://github.com/atSCM/atscm/issues/143



