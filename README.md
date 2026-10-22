# Verlauf: PHQ-9, GAD-7 und WAI-SR (iPad, offline, verschlüsselt), Version 1.1.0

Eine kleine Web-App ohne Server. Sie läuft im Safari des iPads, speichert alles verschlüsselt im Browser-Speicher des Geräts und zeigt pro Kennung den Verlauf mit Hinweisen.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App (HTML, CSS, JavaScript) |
| `sw.js` | Offline-Cache, enthält nur Programmcode |
| `manifest.webmanifest`, `*.png` | Installation auf dem Home-Bildschirm |

## Einrichten

1. **Hosting über https.** Die Verschlüsselung (Web Crypto) funktioniert nur über https, nicht über eine lokal geöffnete Datei. Lege die vier Dateitypen auf einen statischen Webspace, z. B. GitHub Pages oder eigener Webspace. Dort liegt nur Programmcode, niemals Patientendaten. Der Anbieter sieht beim Laden und bei Updates deine IP-Adresse.
2. **Auf dem iPad in Safari öffnen**, dann Teilen, dann **Zum Home-Bildschirm**.
3. **Die App vom Home-Bildschirm starten und erst dort einrichten.** Auf dem iPad hat die Home-Bildschirm-App einen eigenen Speicher, getrennt von Safari. Was du vorher in Safari eingegeben hast, ist in der installierten App nicht sichtbar.
4. Passphrase wählen und sicher notieren. **Es gibt keine Wiederherstellung.**

## Ablauf in der Praxis

1. Übersicht, dann **Neue Messung**, dann Kennung wählen (nur Nummer, kein Name) und **Tablet übergeben**.
2. Patient:in beantwortet die Fragen (eine pro Bildschirm, Überspringen möglich). Danach sperrt die App sich selbst. Die Patientin oder der Patient sieht nie Verläufe oder Kennungen.
3. Du entsperrst mit der Passphrase und landest direkt im Verlauf dieser Kennung.
4. **Papierbogen eintragen** erfasst eine Messung nachträglich mit frei wählbarem Datum.

## WAI-SR (Arbeitsbeziehung)

- **Standardmäßig aus.** Einschalten unter Einstellungen, Fragebögen.
- **Intervall:** Einstellbar, bei jeder n-ten Messung einer Kennung (Voreinstellung 4, also bei der 4., 8., 12. Messung). Mit 1 wird bei jeder Messung gefragt.
- **Ablauf:** Zuerst die Anleitung im Originalwortlaut, dann die 12 Aussagen mit den Stufen 1 (selten) bis 5 (immer). Die Texte stammen aus der deutschen Kurzform (Patientenversion), die du mir geschickt hast.
- **Auswertung:** Gesamtwert 12 bis 60 und Teilskalen Bindung, Aufgaben, Ziele (je 4 bis 20), als Verlauf und in der Tabelle. Es gibt bewusst **keine automatischen Warnhinweise**, weil ich keine belegten Grenzwerte kenne.
- **Zuordnung der Teilskalen** (Bindung: 3, 5, 7, 9. Aufgaben: 1, 2, 10, 12. Ziele: 4, 6, 8, 11) habe ich aus dem Inhalt der Aussagen und meiner Erinnerung abgeleitet. Gleiche sie mit der Auswertungsanleitung des Instruments ab. Sie steht im Skript im Block `Q.wai.subs`.
- **Rechte:** Der Wortlaut der Aussagen liegt dann im Programmcode. Bei einer öffentlichen Seite (z. B. GitHub Pages) kann ihn jeder lesen, der die Adresse kennt. Kläre mit den Rechteinhabern, ob deine Erlaubnis das abdeckt.

## Datenschutz, kurz

- Pseudonymisiert ist nicht anonym: Die Daten bleiben personenbezogene Gesundheitsdaten im Sinne der DSGVO (Art. 9). Die Zuordnung Kennung zu Person gehört ausschließlich in deine Patientenakte.
- Alles liegt lokal und ist mit AES-256-GCM verschlüsselt, der Schlüssel entsteht per PBKDF2 (600.000 Runden) aus deiner Passphrase und wird nie gespeichert. Auch die Kennung ist verschlüsselt.
- Die App sendet selbst keine Daten ins Netz (die Seitenvorgabe `connect-src 'self'` verhindert externe Verbindungen).
- Auto-Sperre nach Leerlauf (einstellbar), nach 60 Sekunden im Hintergrund und nach jeder Tablet-Messung.
- Ergänze dein Verarbeitungsverzeichnis, die Patienteninformation und die technischen Maßnahmen (Art. 32). Eine Datenschutz-Folgenabschätzung ist bei einer Einzelpraxis meist nicht nötig, prüfe das aber mit deiner Landesdatenschutzbehörde. Das ist keine Rechtsberatung.

## Backup (wichtig)

- **Einstellungen, Backup erstellen** erzeugt eine verschlüsselte Datei, die du über das Teilen-Menü in die Dateien-App oder auf einen Praxisrechner legst.
- Ohne Backup sind die Daten weg, wenn die App vom Home-Bildschirm gelöscht oder das iPad zurückgesetzt wird. Die App erinnert daran.
- Die Backup-Datei lässt sich nur mit der Passphrase öffnen, die beim Erstellen galt. Nach einem Passphrasenwechsel bitte ein neues Backup anlegen.

## Bitte vor dem Einsatz prüfen

- **Fragetexte** (PHQ-9, GAD-7) stammen aus der Erinnerung des Entwicklers. Gleiche sie mit der offiziellen deutschen Fassung ab, z. B. unter phqscreeners.com. Die Texte stehen oben im Skript im Block `Q`.
- **Schwellenwerte** für die Hinweise (Voreinstellung: 5 Punkte PHQ-9, 4 Punkte GAD-7, Ausgangswert ab 10, nach 5 Messungen oder 8 Wochen) sind Orientierungswerte aus der Erinnerung. Prüfe sie anhand der Originalliteratur und passe sie in den Einstellungen an.
- **Sitzungsfragen** (optional, standardmäßig aus) sind drei eigene, nicht validierte Kurzfragen, gedacht als Gesprächsanlass zwischen den WAI-SR-Messungen.
- Die Hinweise sind Entscheidungshilfen, kein Befund und kein Medizinprodukt.

## Grenzen

- Getestet in einem automatisierten Chromium (Einrichten, Messen, Sperren, Verschlüsselung, Backup und Wiederherstellung, Passphrasenwechsel, Offline-Start, Hell- und Dunkelmodus, Quer- und Hochformat). **Nicht auf einem echten iPad in Safari getestet.** Prüfe dort besonders: Installation auf dem Home-Bildschirm, Offline-Start, Backup über das Teilen-Menü und die Wiederherstellung.
- Kein Mehrbenutzerbetrieb und keine Synchronisation zwischen Geräten.
- Eine vergessene Passphrase lässt sich nicht zurücksetzen.
