# Moodbot – Schritt-für-Schritt Anleitung

Diese Anleitung begleitet euch durch den Kurs „KI praktisch anwenden – Step by Step zu deinem Chatbot!" (KI-Campus). Folgt den Schritten genau in dieser Reihenfolge – dann funktioniert euer Chatbot am Ende garantiert!

💡 **Kursbezug:** Die Nummern (z. B. „2.2", „3.2") verweisen auf die passenden Kapitel im KI-Campus-Kurs. Wenn etwas unklar ist, schaut dort noch einmal nach.

## Teil 2: Dein erster Chatbot

### 📌 Kapitel 2.2 – Was macht ihr hier?

Ihr richtet eine Programmierumgebung in der Cloud ein (GitHub Codespace) und lasst dort einen fertig vorgebauten Chatbot, den „Moodbot", zum ersten Mal laufen und testet ihn im Terminal.

### Schritt 1 – GitHub-Account erstellen (Kap. 2.2)

- Geh zu github.com/signup
- Registriere dich kostenlos mit E-Mail, Passwort und Benutzername

### Schritt 2 – Repository forken (Kap. 2.2)

- Öffne github.com/weberi/ChatbotMakerspace
- Klicke oben rechts auf „Fork" → „Create a new fork"
- GitHub erstellt eine eigene Kopie in deinem Account

### Schritt 3 – Codespace starten (Kap. 2.2)

- Auf deiner geforkten Repo-Seite oben auf „Code" klicken
- Reiter „Codespaces" wählen
- „Create codespace on main" klicken
- Warten (ca. 10 Minuten beim ersten Mal)

⚠️ Nutze Chrome, nicht Firefox – Firefox macht hier manchmal Probleme.

### Schritt 4 – Ersten Chatbot anlegen (Kap. 2.2)

Im Terminal „Codespaces" (unten, nicht eines der anderen Terminals!) eingeben:

```
rasa init
```

Rasa fragt dich nacheinander:

| Frage | Deine Antwort |
|---|---|
| Please enter a path... | moodbot |
| Path 'moodbot' does not exist. Create path? | Y |
| Do you want to train an initial model? | Y |
| Do you want to speak to the trained assistant? | Y |

Teste kurz: tippe `Hi`, dann z. B. `I am fine!`. Zum Beenden: `/stop`

## Teil 2.3: Der Chatroom

### 📌 Kapitel 2.3 – Was macht ihr hier?

Ihr startet den Chatbot als Server und baut eine Weboberfläche (den „Chatroom"), über die ihr im Browser ganz normal mit ihm chatten könnt – so wie bei einem echten Chat-Fenster.

### Schritt 5 – Chatbot als Server starten

```
rasa run --port 5005 --enable-api --cors "*"
```

### Schritt 6 – Port veröffentlichen

- Klicke auf den Reiter „PORTS" (neben „TERMINAL")
- Rechtsklick auf Port 5005
- „Port Visibility" → „Public"
- Rechtsklick auf Port 5005 → „Copy Local Address" — merke dir diese Adresse!

Sie sieht ungefähr so aus:

```
https://DEIN-CODESPACE-NAME-5005.app.github.dev
```

### Schritt 7 – chatroom.html erstellen

Im Explorer, Rechtsklick auf den moodbot-Ordner → „New File" → Name: `chatroom.html`

Füge diesen Code ein (host-Adresse durch deine eigene ersetzen!):

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Chatroom</title>
   <link rel="stylesheet" href="Chatroom.css" />
   <link rel="stylesheet" type="text/css" href="index.css">
   <link rel="stylesheet" type="text/css" href="theme2.css" />
</head>
<body>
   <div class="chat-container"></div>
   <script src="Chatroom.js"></script>
   <script type="text/javascript">
      var chatroom = new window.Chatroom({
          host: "https://DEIN-CODESPACE-NAME-5005.app.github.dev",
          title: "Chat with a bot",
          container: document.querySelector(".chat-container"),
          welcomeMessage: "Nice to meet you.",
          speechRecognition: "en-US",
          voiceLang: "en-US"
      });
      chatroom.openChat();
   </script>
</body>
</html>
```

⚠️ **Wichtiger Unterschied zur Original-Kursanleitung:** Wir laden die Chatroom-Dateien (Chatroom.css, index.css, theme2.css, Chatroom.js) lokal herunter, statt sie vom CDN zu laden. Grund: Manche Schulnetzwerke/Browser blockieren das externe CDN wegen „Mixed Content"-Sicherheitsregeln (HTTPS-Seite lädt HTTP-Ressource). Mit lokalen Dateien passiert das nicht.

⚠️ **Achtung, sehr häufiger Stolperstein:** Achte genau darauf, dass die `host:`-Adresse **keinen Schrägstrich `/` am Ende** hat! Chatroom.js hängt selbst noch `/webhooks/rest/webhook` an – mit einem zusätzlichen Slash entsteht sonst ein doppelter Slash und der Bot antwortet nicht (siehe Problembehebung unten bei Schritt 10).

```js
// ❌ Falsch (Slash am Ende):
host: "https://DEIN-CODESPACE-NAME-5005.app.github.dev/",

// ✅ Richtig:
host: "https://DEIN-CODESPACE-NAME-5005.app.github.dev",
```

### Schritt 8 – Chatroom-Dateien lokal herunterladen

Neues Terminal öffnen (＋-Symbol bei den Terminal-Tabs), dann:

```
cd moodbot
curl -o Chatroom.css https://cdn.jsdelivr.net/gh/weberi/chatroom@master/dist/Chatroom.css
curl -o index.css https://cdn.jsdelivr.net/gh/weberi/chatroom@master/index.css
curl -o theme2.css https://cdn.jsdelivr.net/gh/weberi/chatroom@master/themes/theme2.css
curl -o Chatroom.js https://cdn.jsdelivr.net/gh/weberi/chatroom@master/dist/Chatroom.js
```

### Schritt 9 – Lokalen Webserver starten (kein Download nötig!)

In einem weiteren neuen Terminal (nicht das mit `rasa run`!):

```
cd moodbot
python3 -m http.server 8000
```

Dann im „PORTS"-Tab auch Port 8000 auf „Public" stellen.

### Schritt 10 – Chatroom im Browser öffnen

Rufe im Browser auf (Portnummer beachten: 8000, nicht 5005):

```
https://DEIN-CODESPACE-NAME-8000.app.github.dev/chatroom.html
```

Du solltest jetzt eine Verzeichnisliste sehen (oder direkt den Chatroom) – klicke auf `chatroom.html` in der Liste, falls nötig.

✅ **Test:** Schreib „hi" in den Chat – der Moodbot sollte antworten!

---

### 🛠️ Problembehebung: „Ich kann schreiben, aber der Bot antwortet nicht"

Das ist ein sehr häufiges Problem an genau dieser Stelle. Hier die zwei wahrscheinlichsten Ursachen und wie ihr sie behebt.

**Erstmal diagnostizieren: Browser-Konsole öffnen**

Drückt **F12** im Browser → Tab **„Network"** (Netzwerk) → schreibt nochmal eine Nachricht in den Chat → schaut euch die Zeile `webhook` an.

**Ursache 1: Fehler wie „CORS error" oder Status „404" beim `webhook`-Request**

Das bedeutet meistens: In eurer `chatroom.html` steht in der `host:`-Zeile ein `/` zu viel am Ende (siehe Warnhinweis bei Schritt 7). Chatroom.js hängt selbst noch `/webhooks/rest/webhook` an – dadurch entsteht ein doppelter Slash (`...github.dev//webhooks/...`), was der Server mit 404 beantwortet.

❌ Falsch:
```js
host: "https://DEIN-CODESPACE-NAME-5005.app.github.dev/",
```
✅ Richtig (kein Slash am Ende!):
```js
host: "https://DEIN-CODESPACE-NAME-5005.app.github.dev",
```

Speichern, Browser-Tab neu laden (F5), nochmal testen.

**Ursache 2: Der `webhook`-Request zeigt Status 200, aber die Antwort ist ein leeres `[]`**

Dann kommt die Nachricht zwar bei Rasa an, aber es ist **kein trainiertes Modell geladen**. Prüft das im Terminal, in dem `rasa run` läuft – steht dort beim Start eine Zeile wie

```
UserWarning: No valid model found at models!
```

und/oder erscheint beim Senden einer Nachricht

```
INFO  rasa.core.agent  - Ignoring message as there is no agent to handle it.
```

→ dann fehlt das Modell. So behebt ihr es:

```
# Im rasa-run-Terminal zuerst stoppen: Strg+C
cd moodbot
rasa train
```

Wartet, bis „Your Rasa model is trained and saved at ..." erscheint, prüft mit `ls -la models/`, ob jetzt eine `.tar.gz`-Datei da ist, und startet den Server neu:

```
rasa run --port 5005 --enable-api --cors "*"
```

⚠️ Denkt daran, danach im „PORTS"-Tab nochmal zu prüfen, ob Port 5005 noch auf „Public" steht – nach einem Neustart von `rasa run` wird das manchmal zurückgesetzt.

**Zum Eingrenzen hilfreich: `rasa shell`**

Testet die gleiche Nachricht direkt im Terminal mit `rasa shell` (ganz ohne Browser/Chatroom):

- Antwortet der Bot dort normal? → Problem liegt an der Verbindung (Ursache 1) oder am laufenden Server-Prozess.
- Antwortet er auch dort nicht? → Problem liegt am Modell (Ursache 2).

Noch genauer lässt sich das mit einem direkten Terminal-Test eingrenzen:

```
curl -X POST http://localhost:5005/webhooks/rest/webhook \
  -H "Content-Type: application/json" \
  -d '{"sender": "test", "message": "hi"}'
```

Das zeigt die rohe Rasa-Antwort ganz ohne Browser/Chatroom dazwischen.

---

## Teil 3: Die Sprache des Chatbots

### 📌 Kapitel 3.1 – Was macht ihr hier?

Ihr lernt die Grundlagen kennen: Ein Rasa-Chatbot wird nicht "programmiert" wie eine normale App, sondern über vier Dateien inhaltlich gesteuert – `nlu.yml`, `domain.yml`, `stories.yml` und `rules.yml`. Diese liegen im Ordner `moodbot/data/` (bzw. `moodbot/`).

Jetzt geht es an die Dateien, die den Chatbot inhaltlich steuern.

### 📌 Kapitel 3.2 – Was macht ihr hier?

Ihr bringt dem Chatbot bei, was Nutzer:innen sagen könnten (das nennt man NLU – Natural Language Understanding). Dazu ergänzt ihr Beispielsätze zu bestehenden Themen (Intents) und fügt einen komplett neuen Intent hinzu: `tell_name`.

### Schritt 11 – nlu.yml erweitern (Kap. 3.2)

Öffne `moodbot/data/nlu.yml`. Ergänze bei den bestehenden Intents mehr Beispiele (10–15 pro Intent) und füge den neuen Intent `tell_name` hinzu:

```yaml
- intent: tell_name
  examples: |
    - I am [Marco](name)
    - Call me [Patrick](name)
    - It's [Aline](name)
    - I'm [Irene](name)
    - My name is [Marco](name)
    - People call me [Marco](name)
    - [Jennifer](name)
    - Call me [Max](name)
    - [Hanna](name)
    - [Sarah](name)
```

💡 **Lernpunkt:** Je mehr unterschiedliche Beispiele (verschiedene Namen, verschiedene Satzmuster) ihr eintragt, desto besser erkennt der Chatbot auch neue, unbekannte Namen. Mit nur wenigen Beispielen erkennt er oft nur exakt die Namen, die er im Training gesehen hat!

### 📌 Kapitel 3.3 – Was macht ihr hier?

Ihr legt fest, was der Chatbot antwortet (das nennt man NLG – Natural Language Generation) und richtet einen „Slot" ein – das ist eine Art Gedächtnis, damit sich der Bot den Namen der/des Nutzer:in merken kann.

### Schritt 12 – domain.yml anpassen (Kap. 3.3)

Öffne `moodbot/domain.yml`. Ergänze/prüfe folgende Abschnitte:

```yaml
intents:
  - greet
  - goodbye
  - affirm
  - deny
  - mood_great
  - mood_unhappy
  - bot_challenge
  - tell_name

entities:
  - name

slots:
  name:
    type: text
    initial_value: "my friend"
    influence_conversation: false
    mappings:
      - type: from_entity
        entity: name

responses:
  utter_greet:
    - text: "Hey {name}! How are you?"
  utter_cheer_up:
    - text: "I'm a chatbot that cheers people up {name}. Here is something to cheer you up. Keep your head up!"
      image: "https://i.imgur.com/iPa8HCj.jpeg"
  utter_go_walk:
    - text: "So, {name}, why don't you just go out for a walk?"
    - text: "A little fresh air might do you some good, right? How about a walk, {name}?"
  utter_did_that_help:
    - text: "Did that help you a little bit?"
    - text: "Might this cheer you up {name}?"
  utter_goodbye:
    - text: "Goodbye {name}. I'm here for you! See you soon!"
    - text: "Goodbye {name}. Come back whenever you need a chat. See you!"
  utter_iamabot:
    - text: "I am a bot, powered by Rasa."
  utter_happy:
    - text: "Great {name}! Share your happiness with the world!"
  utter_ask_name:
    - text: "Hi, what's your name?"
  utter_ask_sports:
    - text: "{name} do you like to do sports?"
  utter_default:
    - text: "Sorry, I didn't get that, can you rephrase?"
```

⚠️ **Bekanntes Problem: Bild bei `utter_cheer_up` lädt nicht** ("The image you are requesting does not exist or is no longer available"). Der verlinkte Imgur-Link ist manchmal tot – Imgur löscht alte, ungenutzte Bilder automatisch. Lösung: `image:`-URL durch einen anderen funktionierenden Bild-Link ersetzen, z. B. diesen Hunde-Link:

```yaml
      image: "https://placedog.net/400/300"
```

Falls dieser Link bei euch auch mal nicht lädt, einfach einen der folgenden Alternativen eintragen:

- `https://placekitten.com/400/300` (Kätzchen)
- `https://picsum.photos/400/300` (zufälliges Foto)

Danach unbedingt neu trainieren (`rasa train`) und den Server neu starten – Responses inkl. Bild-URLs werden beim Training mit ins Modell übernommen, eine reine Änderung in `domain.yml` reicht nicht.

### 📌 Kapitel 3.4 – Was macht ihr hier?

Ihr legt fest, wie das Gespräch abläuft (Dialogmanagement): mit `stories.yml` beschreibt ihr typische Gesprächsverläufe als Beispiele, mit `rules.yml` legt ihr feste Regeln fest, die immer gelten (z. B. „bei Verabschiedung immer Goodbye sagen").

### Schritt 13 – stories.yml (Kap. 3.4)

Öffne `moodbot/data/stories.yml`. Ersetze den kompletten Inhalt durch:

```yaml
version: "3.1"

stories:

- story: "intro"
  steps:
    - intent: greet
    - action: utter_ask_name
    - intent: tell_name
    - action: utter_greet

- story: "user is happy"
  steps:
    - intent: mood_great
    - action: utter_happy
    - action: utter_goodbye

- story: "sad sporty guy, cheered up by sports"
  steps:
    - intent: mood_unhappy
    - action: utter_ask_sports
    - intent: affirm
    - action: utter_go_walk
    - action: utter_did_that_help
    - intent: affirm
    - action: utter_happy
    - action: utter_goodbye

- story: "sad sporty guy, not cheered up by sports, finally happy"
  steps:
    - intent: mood_unhappy
    - action: utter_ask_sports
    - intent: affirm
    - action: utter_go_walk
    - action: utter_did_that_help
    - intent: deny
    - action: utter_cheer_up
    - action: utter_did_that_help
    - intent: affirm
    - action: utter_happy
    - action: utter_goodbye

- story: "sad unsporty guy is happy"
  steps:
    - intent: mood_unhappy
    - action: utter_ask_sports
    - intent: deny
    - action: utter_cheer_up
    - action: utter_did_that_help
    - intent: affirm
    - action: utter_happy
    - action: utter_goodbye

- story: "sad, hopeless"
  steps:
    - intent: mood_unhappy
    - action: utter_ask_sports
    - intent: deny
    - action: utter_cheer_up
    - action: utter_did_that_help
    - intent: deny
    - action: utter_goodbye
```

### Schritt 14 – rules.yml (Kap. 3.4)

Öffne `moodbot/data/rules.yml`. Ersetze den kompletten Inhalt durch:

```yaml
version: "3.1"

rules:
  - rule: "Say goodbye anytime the user says goodbye"
    steps:
      - intent: goodbye
      - action: utter_goodbye

  - rule: "Say 'I am a bot' anytime the user challenges"
    steps:
      - intent: bot_challenge
      - action: utter_iamabot

  - rule: "fallback"
    steps:
      - intent: nlu_fallback
      - action: utter_default
```

### Schritt 15 – Letzter Check vor dem Training (Kap. 3.4)

Prüfe:

- `nlu.yml` enthält `goodbye` und `bot_challenge` mit Beispielsätzen ✓
- `domain.yml` enthält `goodbye`, `bot_challenge`, `utter_iamabot`, `utter_goodbye`, `utter_default` ✓

## Teil 3.5: Trainieren, Testen, Verbessern

### 📌 Kapitel 3.5 – Was macht ihr hier?

Ihr trainiert das Sprachmodell mit all euren neuen Daten aus nlu/domain/stories/rules und testet im Chatroom, ob der Bot jetzt Namen erkennt und auf Stimmungen richtig reagiert.

### Schritt 16 – Chatbot neu trainieren

Terminal öffnen (im moodbot-Ordner), Server ggf. stoppen (Strg+C), dann:

```
rasa train
```

⚠️ Falls der Bot nach dem Training nicht wie erwartet reagiert: Manchmal nutzt Rasa einen alten Zwischenspeicher (Cache). Löscht ihn und trainiert komplett neu:

```
rm -rf .rasa
rm -rf models/*
rasa train
```

Ihr müsst in der Ausgabe sehen: „Starting to train component 'DIETClassifier'" (nicht „Restored from cache") – das dauert dann ein paar Minuten.

### Schritt 17 – Server neu starten und testen

```
rasa run --port 5005 --enable-api --cors "*"
```

Chatroom im Browser neu laden (F5) und testen:

```
hi
My name is [euer Name]
great
```

🔍 **Bonus-Tool zum Debuggen:** `rasa shell nlu`

Wenn der Bot euren Namen nicht erkennt, könnt ihr direkt testen, was Rasa aus einem Satz herausliest:

```
rasa shell nlu
```

Dann einen Satz eingeben, z. B. `My name is Karlo`. Ihr seht dann genau:

- welchen Intent Rasa erkannt hat (und mit welcher Sicherheit)
- welche Entities (z. B. Namen) gefunden wurden

Wenn `"entities": []` leer bleibt, hat das Modell den Namen nicht erkannt – meist hilft es, mehr und vielfältigere Beispiele in der `nlu.yml` zu ergänzen und neu zu trainieren.

## ✅ Checkliste – ist alles fertig?

- [ ] GitHub-Account erstellt
- [ ] Repository geforkt
- [ ] Codespace erstellt und gestartet
- [ ] `rasa init` ausgeführt, Moodbot getestet
- [ ] Rasa-Server läuft (Port 5005, public)
- [ ] Chatroom-Dateien lokal heruntergeladen (Chatroom.css, index.css, theme2.css, Chatroom.js)
- [ ] `chatroom.html` erstellt mit eigener host-Adresse (ohne Slash am Ende!)
- [ ] Python-Webserver läuft (Port 8000, public)
- [ ] Chatroom im Browser erreichbar und Moodbot antwortet
- [ ] `nlu.yml` erweitert (mehr Beispiele + tell_name)
- [ ] `domain.yml` erweitert (intents, entities, slots, responses)
- [ ] `stories.yml` komplett ersetzt
- [ ] `rules.yml` komplett ersetzt
- [ ] Chatbot trainiert (`rasa train`)
- [ ] Im Chatroom getestet: Bot begrüßt, fragt nach Namen, spricht euch persönlich an

🎉 Geschafft! Euer Moodbot kennt jetzt eure Namen und reagiert auf eure Stimmung.

## 🚀 Wie geht's weiter? (für zuhause, wenn ihr mehr wollt)

Diese Anleitung endet bei Kapitel 3.5 – euer Chatbot funktioniert damit schon komplett! Wenn ihr aber Lust habt, zuhause weiterzumachen, geht der KI-Campus-Kurs noch deutlich weiter. Hier ein kurzer Ausblick, was euch dort erwartet:

### Teil 4: Der Chatbot lernt das Internet kennen

- **Kapitel 4.1 – Chatbot im Internet der Dinge und Dienste:** Ihr lernt ein neues Tool namens Node-RED kennen – damit könnt ihr Chatbots mit dem Internet verbinden. Außerdem: Grundlagen zu Client, Server und HTTP.
- **Kapitel 4.2 – Das Web erschließen:** Ihr sprecht echte Web-APIs an (z. B. eine Postleitzahlen-API und eine Wetter-API) und lernt das Datenformat JSON kennen, in dem solche Antworten aus dem Internet ankommen.
- **Kapitel 4.3 – Aktionen entwickeln:** Ihr baut die Brücke zwischen Rasa und Node-RED (den sogenannten Rasa ActionServer), damit der Chatbot nicht nur antworten, sondern echte Aktionen ausführen kann.

### Teil 5: Ein Chatbot mit echter Aktion

- **Kapitel 5.1 – Chatbot mit Aktion:** Ihr baut einen komplett neuen Bot, den „Weatherbot", von Grund auf (eigene nlu/domain/stories/rules/endpoints-Dateien) – er fragt bei einer echten Wetter-API nach und sagt euch das Wetter für einen Ort an.
- **Kapitel 5.2 – Mehr Skills für deinen Chatbot:** Ihr gebt eurem Weatherbot eine zweite Fähigkeit: Er kann jetzt auch Wikipedia durchsuchen und euch Infos dazu ausgeben.

### Teil 6: Chatbots und moderne KI (LLMs)

- **Kapitel 6.1 – Dein Chatbot und LLMs:** Ihr bindet ein echtes KI-Sprachmodell (LLM) über eine API (Groq) in euren Chatbot ein, damit er freier und natürlicher antworten kann – nicht mehr nur mit fest vorgeschriebenen Sätzen.
- **Kapitel 6.2 – Werden Chatbots bald Agenten?:** Kein Bauprojekt mehr, sondern ein spannender Theorie-Ausblick: Wie verändern LLMs Chatbots, und was bedeutet „Agentic AI" – also KI, die selbstständig Aufgaben plant und ausführt?

💡 **Tipp:** Kapitel 4–5 sind ein deutlicher Schwierigkeitssprung (neues Tool, echte APIs, mehr Dateien) – nehmt euch dafür Zeit und geht wieder Schritt für Schritt vor, genau wie in dieser Anleitung. Kapitel 6 könnt ihr auch einfach nur lesen, ganz ohne selbst zu programmieren.
