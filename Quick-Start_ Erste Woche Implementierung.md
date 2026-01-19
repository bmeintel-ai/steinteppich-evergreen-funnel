# Quick-Start: Erste Woche Implementierung

## 🎯 Ziel dieser Woche
Landingpage live, erste Leads erfassen, Automations-Workflow konfigurieren

---

## Tag 1-2: Landingpage hosten

### Schritt 1: Hosting-Provider wählen
**Empfehlung: Netlify (kostenlos, einfach, schnell)**

1. Gehen Sie zu https://netlify.com
2. Klicken Sie auf "Sign up"
3. Registrieren Sie sich mit GitHub oder E-Mail
4. Klicken Sie auf "New site from Git" oder "Drag and drop"

### Schritt 2: HTML-Datei hochladen
**Option A: Drag & Drop (Einfachste Methode)**
1. Laden Sie `steinteppich_landingpage.html` herunter
2. Ziehen Sie die Datei auf die Netlify-Website
3. Netlify generiert automatisch eine URL (z.B. `https://xyz123.netlify.app`)

**Option B: Mit Git (Für Fortgeschrittene)**
1. Laden Sie die HTML-Datei in ein GitHub-Repository
2. Verbinden Sie das Repository mit Netlify
3. Netlify deployed automatisch bei jedem Update

### Schritt 3: URL testen
1. Öffnen Sie die generierte URL im Browser
2. Überprüfen Sie:
   - [ ] Seite lädt korrekt
   - [ ] Alle Texte sind sichtbar
   - [ ] Formular ist funktionsfähig
   - [ ] Buttons sind klickbar
   - [ ] Mobile-Ansicht funktioniert (im Browser F12 drücken)

### Schritt 4: Domain konfigurieren (Optional)
1. Gehen Sie zu Netlify Settings
2. Klicken Sie auf "Domain settings"
3. Verbinden Sie Ihre eigene Domain (z.B. steinteppich-ratgeber.bitschus.de)
4. Folgen Sie den Anweisungen für DNS-Einstellungen

---

## Tag 3: CRM auswählen und einrichten

### Option A: HubSpot (Empfohlen für Anfänger)

**Schritt 1: Account erstellen**
1. Gehen Sie zu https://hubspot.com
2. Klicken Sie auf "Get started free"
3. Registrieren Sie sich mit E-Mail
4. Bestätigen Sie Ihre E-Mail-Adresse

**Schritt 2: Basis-Setup**
1. Wählen Sie "CRM" als Hauptprodukt
2. Beantworten Sie die Setup-Fragen
3. Erstellen Sie Ihr erstes Team

**Schritt 3: CSV importieren**
1. Gehen Sie zu **Contacts** → **Import**
2. Klicken Sie auf "Import contacts"
3. Wählen Sie `email_automation.csv`
4. Mappen Sie die Felder:
   - `email` → Email Address
   - `name` → First Name
   - `segment` → Custom Property "Projektart"
   - `plz` → Postal Code

### Option B: Brevo (Empfohlen für E-Mail-Marketing)

**Schritt 1: Account erstellen**
1. Gehen Sie zu https://brevo.com
2. Klicken Sie auf "Sign up"
3. Registrieren Sie sich mit E-Mail
4. Bestätigen Sie Ihre E-Mail-Adresse

**Schritt 2: CSV importieren**
1. Gehen Sie zu **Contacts** → **Import contacts**
2. Wählen Sie `email_automation.csv`
3. Mappen Sie die Felder (wie oben)

---

## Tag 4-5: Formular-Integration

### Schritt 1: Formular-Submission tracken

**Option A: Mit Formspree (Kostenlos, Einfach)**

1. Gehen Sie zu https://formspree.io
2. Klicken Sie auf "Sign up"
3. Erstellen Sie ein neues Formular
4. Erhalten Sie eine Formular-ID (z.B. `f/abc123def456`)
5. Bearbeiten Sie Ihre HTML-Datei:

```html
<!-- Ersetzen Sie diese Zeile: -->
<form style="max-width: 600px; ...">

<!-- Mit dieser Zeile: -->
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" style="max-width: 600px; ...">
```

6. Laden Sie die aktualisierte Datei zu Netlify hoch

**Option B: Mit Zapier (Empfohlen für CRM-Integration)**

1. Gehen Sie zu https://zapier.com
2. Klicken Sie auf "Create Zap"
3. Wählen Sie **Trigger: Webhooks by Zapier** → **Catch Raw Hook**
4. Kopieren Sie die Webhook-URL
5. Bearbeiten Sie Ihre HTML-Datei:

```html
<!-- Ersetzen Sie diese Zeile: -->
<form style="max-width: 600px; ...">

<!-- Mit dieser Zeile: -->
<form action="YOUR_WEBHOOK_URL" method="POST" style="max-width: 600px; ...">
```

6. Laden Sie die aktualisierte Datei zu Netlify hoch

### Schritt 2: Test-Submission durchführen

1. Öffnen Sie Ihre Landingpage
2. Füllen Sie das Formular mit Test-Daten aus
3. Klicken Sie auf "Ratgeber kostenlos erhalten"
4. Überprüfen Sie:
   - [ ] Bestätigungsmeldung erscheint
   - [ ] Daten erscheinen in Formspree/Zapier
   - [ ] Daten erscheinen in Ihrem CRM

---

## Tag 5-6: E-Mail-Templates erstellen

### In HubSpot:

1. Gehen Sie zu **Marketing** → **Email**
2. Klicken Sie auf "Create email"
3. Wählen Sie "Regular email"
4. Erstellen Sie 8 Templates mit den Inhalten aus `email_automation.json`:

**Template 1: Tag 0 – Ratgeber**
- Betreff: "Ihr Gratis-Ratgeber: Alles über Steinteppiche!"
- Inhalt: (aus JSON)
- CTA: "Ratgeber jetzt lesen"

**Template 2: Tag 2 – Vorteile**
- Betreff: "Fugenlos, pflegeleicht, sicher: Die 3 Top-Vorteile Ihres neuen Bodens"
- Inhalt: (aus JSON)
- CTA: "Mehr über die Vorteile erfahren"

... (wiederholen für alle 8 E-Mails)

### In Brevo:

1. Gehen Sie zu **Email** → **Templates**
2. Klicken Sie auf "Create template"
3. Wählen Sie "Blank template"
4. Erstellen Sie die Templates wie oben

---

## Tag 7: Automations-Workflow konfigurieren

### In HubSpot:

1. Gehen Sie zu **Automation** → **Workflows**
2. Klicken Sie auf "Create workflow"
3. Wählen Sie **Trigger: Contact submitted a form**
4. Konfigurieren Sie folgende Schritte:

```
Step 1: Send email "Tag 0 - Ratgeber"
Step 2: Set property "automation_started" = today
Step 3: Set property "segment" = [form field value]

Delay: 2 days

Step 4: Send email "Tag 2 - Vorteile"

Delay: 3 days

Step 5: Send email "Tag 5 - Haltbarkeit"

Delay: 1 day

Step 6: Branch by property "segment"
  - Branch "Balkon": Send email "Tag 6a - Balkon"
  - Branch "Treppe": Send email "Tag 6b - Treppe"
  - Branch "Innenraum": Send email "Tag 6c - Innenraum"

Delay: 2 days

Step 7: Send email "Tag 8 - Angebot"

Delay: 2 days

Step 8: Send email "Tag 10 - Letzte Chance"
```

5. Klicken Sie auf "Publish"

### In Brevo:

1. Gehen Sie zu **Automation** → **Automations**
2. Klicken Sie auf "Create automation"
3. Wählen Sie **Trigger: New contact added**
4. Konfigurieren Sie die gleichen Schritte wie oben

---

## 🧪 Test-Submission durchführen

### Schritt 1: Test-Lead erstellen

1. Öffnen Sie Ihre Landingpage
2. Füllen Sie das Formular mit Test-Daten aus:
   - Name: "Test Lead"
   - E-Mail: Ihre E-Mail-Adresse
   - PLZ: "72250"
   - Projektart: "Balkon"
   - Nachricht: "Test"

3. Klicken Sie auf "Ratgeber kostenlos erhalten"

### Schritt 2: Automations-Workflow überprüfen

1. Gehen Sie zu Ihrem CRM
2. Suchen Sie den Test-Lead
3. Überprüfen Sie:
   - [ ] Lead wurde erstellt
   - [ ] Segment wurde gesetzt
   - [ ] automation_started wurde gesetzt
   - [ ] Tag 0 E-Mail wurde versendet

### Schritt 3: E-Mail-Empfang überprüfen

1. Überprüfen Sie Ihren E-Mail-Posteingang
2. Suchen Sie nach der Tag 0 E-Mail
3. Überprüfen Sie:
   - [ ] E-Mail wurde empfangen
   - [ ] Betreff ist korrekt
   - [ ] Inhalt ist korrekt
   - [ ] CTA-Button funktioniert

---

## 📊 Erste Metriken überprüfen

### Nach 24 Stunden:

1. Gehen Sie zu Ihrem CRM
2. Überprüfen Sie:
   - [ ] Wie viele Leads wurden erfasst?
   - [ ] Wie viele E-Mails wurden versendet?
   - [ ] Wie viele E-Mails wurden geöffnet?
   - [ ] Wie viele Links wurden geklickt?

### Zielwerte für die erste Woche:

- **Lead-Erfassung:** 5-10 Leads (abhängig von Traffic)
- **E-Mail-Open-Rate:** 30%+
- **Click-Through-Rate:** 5%+

---

## ⚠️ Häufige Probleme und Lösungen

| Problem | Lösung |
|---------|--------|
| Formular wird nicht versendet | Überprüfen Sie die Webhook-URL / Formspree-ID |
| E-Mails landen im Spam | Überprüfen Sie SPF/DKIM-Einstellungen in Ihrem CRM |
| Automations-Workflow startet nicht | Überprüfen Sie, ob der Trigger korrekt konfiguriert ist |
| Segmentierung funktioniert nicht | Überprüfen Sie, ob das Segment-Feld korrekt gemappt ist |
| Tracking funktioniert nicht | Überprüfen Sie, ob Tracking in den E-Mail-Einstellungen aktiviert ist |

---

## ✅ Checkliste für Ende der Woche

- [ ] Landingpage ist live und funktioniert
- [ ] Formular-Submission funktioniert
- [ ] CRM ist eingerichtet und konfiguriert
- [ ] CSV-Datei wurde importiert
- [ ] 8 E-Mail-Templates wurden erstellt
- [ ] Automations-Workflow wurde konfiguriert
- [ ] Test-Submission wurde durchgeführt
- [ ] Tag 0 E-Mail wurde empfangen
- [ ] Erste Metriken wurden überprüft

---

## 🚀 Nächste Schritte (Woche 2)

- [ ] Erste echte Leads erfassen
- [ ] Engagement-Metriken analysieren
- [ ] A/B-Tests durchführen
- [ ] Betreff-Zeilen optimieren
- [ ] CTA-Texte testen
- [ ] Segment-Performance überprüfen

**Viel Erfolg! 🎉**
