# CRM-Integration: Schritt-für-Schritt-Anleitung

## 1. HubSpot Integration

### 1.1 CSV-Import

1. Gehen Sie zu **Contacts** → **Import**
2. Wählen Sie **CSV-Datei hochladen**
3. Laden Sie `email_automation_extended.csv` hoch
4. Mappen Sie die Felder:
   - `email` → Email Address
   - `name` → First Name / Last Name
   - `segment` → Custom Property "Projektart"
   - `plz` → Postal Code

### 1.2 Workflow-Setup

1. Gehen Sie zu **Automation** → **Workflows**
2. Klicken Sie auf **Create workflow**
3. Wählen Sie **Trigger: Contact submitted a form**
4. Konfigurieren Sie folgende Schritte:

```
Step 1: Send email (Template: Tag 0)
Step 2: Set property "automation_started" = today
Step 3: Set property "segment" = [form field value]

Delay: 2 days

Step 4: Send email (Template: Tag 2)

Delay: 3 days

Step 5: Send email (Template: Tag 5)

Delay: 1 day

Step 6: Branch by property "segment"
  - Branch "Balkon": Send email (Template: Tag 6a)
  - Branch "Treppe": Send email (Template: Tag 6b)
  - Branch "Innenraum": Send email (Template: Tag 6c)
  - Branch "Allgemein": Send email (Template: Tag 2)

Delay: 2 days

Step 7: Send email (Template: Tag 8)

Delay: 2 days

Step 8: Send email (Template: Tag 10)
```

### 1.3 Tracking-Setup

1. Gehen Sie zu **Settings** → **Tracking Code**
2. Kopieren Sie den Tracking-Code
3. Fügen Sie ihn auf Ihrer Landingpage ein
4. Aktivieren Sie **Email tracking** in den Workflow-Einstellungen

---

## 2. Brevo (Sendinblue) Integration

### 2.1 CSV-Import

1. Gehen Sie zu **Contacts** → **Import**
2. Wählen Sie **CSV-Datei hochladen**
3. Laden Sie `email_automation_extended.csv` hoch
4. Mappen Sie die Felder:
   - `email` → Email
   - `name` → First Name / Last Name
   - `segment` → Custom Attribute "Projektart"
   - `plz` → Postal Code

### 2.2 Automation-Setup

1. Gehen Sie zu **Automation** → **Automations**
2. Klicken Sie auf **Create automation**
3. Wählen Sie **Trigger: New contact added**
4. Konfigurieren Sie folgende Schritte:

```
Step 1: Send email (Template: Tag 0)
Step 2: Wait (2 days)
Step 3: Send email campaign (Tag 2)
Step 4: Wait (3 days)
Step 5: Send email campaign (Tag 5)
Step 6: Wait (1 day)
Step 7: Conditional branching based on "Projektart"
  - If "Balkon": Send email (Tag 6a)
  - If "Treppe": Send email (Tag 6b)
  - If "Innenraum": Send email (Tag 6c)
Step 8: Wait (2 days)
Step 9: Send email campaign (Tag 8)
Step 10: Wait (2 days)
Step 11: Send email campaign (Tag 10)
```

### 2.3 E-Mail-Template-Erstellung

1. Gehen Sie zu **Email** → **Templates**
2. Klicken Sie auf **Create template**
3. Geben Sie die E-Mail-Inhalte ein (aus `email_automation.json`)
4. Speichern Sie das Template
5. Wiederholen Sie für alle 8 E-Mails

---

## 3. Mailchimp Integration

### 3.1 CSV-Import

1. Gehen Sie zu **Audience** → **Import**
2. Wählen Sie **CSV-Datei hochladen**
3. Laden Sie `email_automation_extended.csv` hoch
4. Mappen Sie die Felder:
   - `email` → Email Address
   - `name` → First Name / Last Name
   - `segment` → Merge Field "PROJEKTART"
   - `plz` → Merge Field "PLZ"

### 3.2 Automation-Setup

1. Gehen Sie zu **Automations** → **Automation**
2. Klicken Sie auf **Create automation**
3. Wählen Sie **Trigger: Welcome new subscribers**
4. Konfigurieren Sie folgende Schritte:

```
Email 1: Tag 0 (Sofort)
Email 2: Tag 2 (2 Tage später)
Email 3: Tag 5 (5 Tage später)
Email 4: Tag 6 (6 Tage später, segmentiert)
Email 5: Tag 8 (8 Tage später)
Email 6: Tag 10 (10 Tage später)
```

---

## 4. ActiveCampaign Integration

### 4.1 CSV-Import

1. Gehen Sie zu **Contacts** → **Import**
2. Wählen Sie **CSV-Datei hochladen**
3. Laden Sie `email_automation_extended.csv` hoch
4. Mappen Sie die Felder:
   - `email` → Email
   - `name` → First Name / Last Name
   - `segment` → Custom Field "Projektart"
   - `plz` → Postal Code

### 4.2 Automation-Setup

1. Gehen Sie zu **Automations** → **Automations**
2. Klicken Sie auf **Create automation**
3. Wählen Sie **Trigger: Contact submitted form**
4. Konfigurieren Sie folgende Schritte:

```
Step 1: Send email (Template: Tag 0)
Step 2: Tag contact with "automation_started"
Step 3: Wait (2 days)
Step 4: Send email (Template: Tag 2)
Step 5: Wait (3 days)
Step 6: Send email (Template: Tag 5)
Step 7: Wait (1 day)
Step 8: If/else based on "Projektart"
  - If "Balkon": Send email (Tag 6a)
  - If "Treppe": Send email (Tag 6b)
  - If "Innenraum": Send email (Tag 6c)
  - Else: Send email (Tag 2)
Step 9: Wait (2 days)
Step 10: Send email (Template: Tag 8)
Step 11: Wait (2 days)
Step 12: Send email (Template: Tag 10)
```

---

## 5. Zapier Integration (für beliebige CRM-Systeme)

### 5.1 Zap-Setup

1. Gehen Sie zu **Zapier.com**
2. Klicken Sie auf **Create Zap**
3. Wählen Sie **Trigger App: Webhooks by Zapier**
4. Wählen Sie **Trigger Event: Catch Raw Hook**
5. Kopieren Sie die Webhook-URL
6. Integrieren Sie die URL in Ihre Landingpage

### 5.2 Aktion-Setup

```
Trigger: Webhook empfangen

Action 1: Create contact in [Ihr CRM]
  - email: [webhook data.email]
  - name: [webhook data.name]
  - segment: [webhook data.segment]
  - plz: [webhook data.plz]

Action 2: Send email via Brevo
  - Template: Tag 0
  - To: [webhook data.email]

Action 3: Create task in [Ihr CRM]
  - Title: "Follow-up: [webhook data.name]"
  - Due date: Today + 2 days
```

---

## 6. Google Sheets Integration (für Tracking)

### 6.1 Tracking-Sheet-Setup

1. Erstellen Sie ein Google Sheet mit folgenden Spalten:
   - Datum
   - E-Mail
   - Name
   - Segment
   - E-Mail-Typ
   - Status (Versendet/Geöffnet/Geklickt)
   - Link geklickt
   - Konvertiert

2. Verbinden Sie Ihr CRM mit Google Sheets via Zapier:

```
Trigger: Contact updated in [Ihr CRM]
Action: Create row in Google Sheet
  - Datum: today
  - E-Mail: [contact.email]
  - Name: [contact.name]
  - Segment: [contact.segment]
  - Status: [contact.status]
```

---

## 7. Fehlerbehandlung & Troubleshooting

### 7.1 Häufige Probleme

| Problem | Ursache | Lösung |
|---------|--------|--------|
| E-Mails werden nicht versendet | Automation nicht aktiviert | Überprüfen Sie, ob die Automation "Aktiv" ist |
| Falsche Segmentierung | Feldmapping fehlerhaft | Überprüfen Sie das Feldmapping im Import |
| Tracking funktioniert nicht | Tracking-Code nicht installiert | Installieren Sie den Tracking-Code auf der Landingpage |
| Bounces steigen | E-Mail-Liste veraltet | Bereinigen Sie die E-Mail-Liste vor dem Import |
| Unsubscribe-Rate zu hoch | Zu häufige E-Mails | Erhöhen Sie die Abstände zwischen E-Mails |

### 7.2 Debugging-Tipps

1. **Test-Lead erstellen:** Erstellen Sie einen Test-Lead mit Ihrer eigenen E-Mail-Adresse
2. **Logs überprüfen:** Überprüfen Sie die Automations-Logs auf Fehler
3. **E-Mail-Vorschau:** Überprüfen Sie die E-Mail-Vorschau vor dem Versand
4. **Bounce-Analyse:** Analysieren Sie Bounces und bereinigen Sie die Liste
5. **Engagement-Metriken:** Überprüfen Sie Open Rates und CTRs

---

## 8. Compliance & GDPR

### 8.1 Erforderliche Maßnahmen

- [ ] Double-Opt-In aktivieren (für neue Kontakte)
- [ ] Datenschutzerklärung auf Landingpage verlinken
- [ ] Unsubscribe-Link in jeder E-Mail
- [ ] Impressum auf Landingpage
- [ ] Datenverarbeitungsvereinbarung (DPA) mit CRM-Anbieter
- [ ] Aufbewahrungsrichtlinien definieren (z.B. Löschen nach 12 Monaten Inaktivität)

### 8.2 Unsubscribe-Handling

```
WENN: Lead klickt auf "Abmelden"
DANN:
  - Markiere Lead als "Unsubscribed"
  - Stoppe alle zukünftigen E-Mails
  - Speichere Unsubscribe-Datum
  - Beachte GDPR-Anforderungen (Recht auf Vergessenwerden)
```

---

## 9. Testing & Qualitätssicherung

### 9.1 Pre-Launch-Checkliste

- [ ] Alle E-Mail-Templates erstellt und getestet
- [ ] Automations-Sequenz in Staging-Umgebung getestet
- [ ] Test-Lead durch gesamte Sequenz verfolgt
- [ ] Alle Links funktionieren
- [ ] Tracking funktioniert
- [ ] Unsubscribe-Link funktioniert
- [ ] Compliance-Anforderungen erfüllt
- [ ] Landingpage-Formular funktioniert
- [ ] CRM-Feldmapping überprüft
- [ ] Backup der Konfiguration erstellt

### 9.2 Post-Launch-Monitoring

- [ ] Erste 24 Stunden: Stündliches Monitoring
- [ ] Erste Woche: Tägliches Monitoring
- [ ] Danach: Wöchentliches Monitoring
- [ ] Metriken tracken: Open Rate, CTR, Conversion Rate
- [ ] Bounce-Raten überwachen
- [ ] Unsubscribe-Raten überwachen

---

## 10. Support & Kontakt

Bei Fragen zur Integration kontaktieren Sie:

- **Bitschus Support:** info@bitschus.de
- **CRM-Support:** [CRM-Anbieter-Support]
- **Zapier Support:** support@zapier.com (für Zapier-Integrationen)
