# Segmentierungs- und Automations-Logik für Bitschus Evergreen-Funnel

## 1. Segmentierungs-Matrix

| Segment | Trigger | Beschreibung | Zielgruppe | Besonderheiten |
|---------|---------|-------------|-----------|-----------------|
| **Balkon** | Projektart = "Balkon" | Leads mit Balkon-Sanierungsbedarf | Hausbesitzer mit Balkon-Problemen | Fokus auf Wasserdurchlässigkeit, UV-Beständigkeit, Witterungsschutz |
| **Treppe** | Projektart = "Treppe" | Leads mit Treppensanierungsbedarf | Hausbesitzer mit alten/unsicheren Treppen | Fokus auf Rutschfestigkeit, Sicherheit, Langlebigkeit |
| **Innenraum** | Projektart = "Innenbereich" | Leads mit Innenraum-Bedarf | Hausbesitzer/Mieter mit Renovierungswunsch | Fokus auf Ästhetik, Allergikerfreundlichkeit, Komfort |
| **Allgemein** | Projektart = "Sonstiges" oder leer | Leads ohne spezifische Auswahl | Interessierte ohne klare Vorstellung | Allgemeine Informationen, später Qualifizierung |

---

## 2. Automations-Regeln

### 2.1 Trigger-Regel: Lead-Generierung

```
WENN: Lead füllt Landingpage-Formular aus
DANN:
  - Speichere Lead-Daten in CRM
  - Setze Segment basierend auf Projektart
  - Starte Automations-Sequenz
  - Sende Tag 0 E-Mail sofort
  - Setze nächste E-Mail für Tag 2
```

**Implementierung in gängigen CRM-Systemen:**

- **HubSpot:** Workflow mit Trigger "Form Submission" → Action "Send Email"
- **Brevo (Sendinblue):** Automation mit Trigger "Lead erfasst" → Action "E-Mail senden"
- **Mailchimp:** Automation mit Trigger "Neue Kontakte" → Action "E-Mail-Sequenz starten"
- **ActiveCampaign:** Automation mit Trigger "Formular ausgefüllt" → Action "Tag hinzufügen"

---

### 2.2 Verzögerungs-Regel: E-Mail-Timing

```
WENN: Vorherige E-Mail versendet
DANN:
  - Berechne Versendungsdatum (Tag + Versendungszeit)
  - Warte bis zum berechneten Zeitpunkt
  - Sende nächste E-Mail
  - Protokolliere Versendungszeit
```

**Timing-Tabelle:**

| E-Mail | Tag | Versendungszeit | Verzögerung ab Lead-Erfassung |
|--------|-----|-----------------|-------------------------------|
| Tag 0 | 0 | Sofort | Sofort |
| Tag 2 | 2 | 09:00 Uhr | 2 Tage + 09:00 |
| Tag 5 | 5 | 14:00 Uhr | 5 Tage + 14:00 |
| Tag 6 | 6 | 09:00 Uhr | 6 Tage + 09:00 |
| Tag 8 | 8 | 10:00 Uhr | 8 Tage + 10:00 |
| Tag 10 | 10 | 15:00 Uhr | 10 Tage + 15:00 |

---

### 2.3 Segmentierungs-Regel: Dynamische E-Mail-Auswahl

```
WENN: Tag 6 erreicht
DANN:
  WENN Segment = "Balkon"
    DANN: Sende E-Mail "Balkon-Sanierung"
  SONST WENN Segment = "Treppe"
    DANN: Sende E-Mail "Treppensanierung"
  SONST WENN Segment = "Innenraum"
    DANN: Sende E-Mail "Innenräume"
  SONST
    DANN: Sende E-Mail "Allgemein"
```

**Implementierung:**

```json
{
  "automation_rule": "segment_based_email_tag_6",
  "trigger": "tag_6_reached",
  "conditions": [
    {
      "field": "segment",
      "operator": "equals",
      "value": "Balkon",
      "action": "send_email",
      "email_id": "template_004"
    },
    {
      "field": "segment",
      "operator": "equals",
      "value": "Treppe",
      "action": "send_email",
      "email_id": "template_005"
    },
    {
      "field": "segment",
      "operator": "equals",
      "value": "Innenraum",
      "action": "send_email",
      "email_id": "template_006"
    },
    {
      "field": "segment",
      "operator": "equals",
      "value": "Allgemein",
      "action": "send_email",
      "email_id": "template_002"
    }
  ]
}
```

---

### 2.4 Engagement-Tracking-Regel

```
WENN: E-Mail wird versendet
DANN:
  - Füge Tracking-Pixel hinzu
  - Tracke E-Mail-Öffnungen
  - Tracke Link-Klicks
  - Speichere Engagement-Daten

WENN: Lead öffnet E-Mail
DANN:
  - Inkrementiere "email_opens" Counter
  - Aktualisiere "last_engagement_date"
  - Markiere Lead als "Engaged"

WENN: Lead klickt auf Link
DANN:
  - Inkrementiere "link_clicks" Counter
  - Speichere geklickten Link
  - Markiere Lead als "Highly Engaged"
```

---

### 2.5 Fallback-Regel: Bounce-Handling

```
WENN: E-Mail bounced (Hard Bounce)
DANN:
  - Markiere E-Mail-Adresse als ungültig
  - Stoppe Automations-Sequenz
  - Benachrichtige Vertriebsteam
  - Speichere Bounce-Grund

WENN: E-Mail bounced (Soft Bounce)
DANN:
  - Versuche Zustellung später erneut
  - Maximal 3 Versuche
  - Nach 3 Versuchen: Markiere als ungültig
```

---

### 2.6 Unsubscribe-Regel

```
WENN: Lead klickt auf "Abmelden"
DANN:
  - Stoppe Automations-Sequenz sofort
  - Markiere Lead als "Unsubscribed"
  - Entferne Lead aus allen zukünftigen E-Mails
  - Speichere Unsubscribe-Datum
  - Beachte GDPR-Anforderungen
```

---

### 2.7 Konversions-Regel

```
WENN: Lead klickt auf CTA "Kostenlose Beratung anfragen"
DANN:
  - Markiere Lead als "Converted"
  - Setze "conversion_date" = heute
  - Berechne "days_to_conversion"
  - Stoppe Automations-Sequenz (optional)
  - Benachrichtige Vertriebsteam
  - Erstelle Aufgabe für Vertrieb
```

---

## 3. Lead-Scoring

Implementieren Sie ein Lead-Scoring-System, um die Qualität von Leads zu bewerten:

| Aktion | Punkte |
|--------|--------|
| Lead erfasst | +10 |
| E-Mail geöffnet | +5 |
| Link geklickt | +10 |
| CTA geklickt | +25 |
| Formular erneut ausgefüllt | +15 |
| Telefon angerufen | +50 |

**Scoring-Schwellen:**

- 0-20 Punkte: Cold Lead
- 21-50 Punkte: Warm Lead
- 51+ Punkte: Hot Lead (Vertrieb kontaktieren)

---

## 4. CRM-Feld-Mapping

Folgende Felder sollten in Ihrem CRM vorhanden sein:

| CRM-Feld | Datentyp | Beschreibung | Erforderlich |
|----------|----------|-------------|-------------|
| `email` | Text | E-Mail-Adresse | Ja |
| `name` | Text | Vollständiger Name | Ja |
| `plz` | Text | Postleitzahl | Ja |
| `segment` | Dropdown | Projektart (Balkon/Treppe/Innenraum/Allgemein) | Ja |
| `nachricht` | Text (lang) | Lead-Notizen | Nein |
| `automation_started` | Datum/Zeit | Wann Automation gestartet | Auto |
| `last_email_sent` | Datum/Zeit | Letzte E-Mail versendet | Auto |
| `email_opens` | Zahl | Anzahl E-Mail-Öffnungen | Auto |
| `link_clicks` | Zahl | Anzahl Link-Klicks | Auto |
| `last_engagement` | Datum/Zeit | Letztes Engagement | Auto |
| `lead_score` | Zahl | Berechneter Lead Score | Auto |
| `status` | Dropdown | Cold/Warm/Hot/Converted | Auto |
| `conversion_date` | Datum | Konversionsdatum | Auto |
| `days_to_conversion` | Zahl | Tage bis Konversion | Auto |

---

## 5. Implementierungs-Beispiele

### 5.1 HubSpot Workflow

```
Trigger: "Contact submitted a form"
Condition: "Form is Landing Page Form"

Action 1: "Send email" (Template: Tag 0)
Action 2: "Set property" (automation_started = today)
Action 3: "Set property" (segment = [form field value])

Delay: 2 days
Action 4: "Send email" (Template: Tag 2)

Delay: 3 days
Action 5: "Send email" (Template: Tag 5)

Delay: 1 day
Action 6: "Branch by property" (segment)
  - Branch "Balkon": Send email (Template: Tag 6a)
  - Branch "Treppe": Send email (Template: Tag 6b)
  - Branch "Innenraum": Send email (Template: Tag 6c)
  - Branch "Allgemein": Send email (Template: Tag 2)

Delay: 2 days
Action 7: "Send email" (Template: Tag 8)

Delay: 2 days
Action 8: "Send email" (Template: Tag 10)
```

### 5.2 Brevo (Sendinblue) Automation

```
Trigger: "New contact added"
Condition: "Contact email is valid"

Step 1: "Send transactional email" (Tag 0)
Step 2: "Wait" (2 days)
Step 3: "Send email campaign" (Tag 2)
Step 4: "Wait" (3 days)
Step 5: "Send email campaign" (Tag 5)
Step 6: "Wait" (1 day)
Step 7: "Conditional branching"
  - If segment = "Balkon": Send email (Tag 6a)
  - If segment = "Treppe": Send email (Tag 6b)
  - If segment = "Innenraum": Send email (Tag 6c)
Step 8: "Wait" (2 days)
Step 9: "Send email campaign" (Tag 8)
Step 10: "Wait" (2 days)
Step 11: "Send email campaign" (Tag 10)
```

### 5.3 Zapier Integration

```
Trigger: "New form submission" (Landingpage)

Action 1: "Create contact in CRM"
  - email: [form email]
  - name: [form name]
  - segment: [form projektart]

Action 2: "Send email via Brevo"
  - Template: Tag 0

Action 3: "Delay" (2 days)

Action 4: "Send email via Brevo"
  - Template: Tag 2

... (weitere Schritte)
```

---

## 6. Reporting & Analytics

### 6.1 Wichtige Metriken

| Metrik | Zielwert | Berechnung |
|--------|----------|-----------|
| **Open Rate** | 30%+ | (Öffnungen / Versendungen) × 100 |
| **Click-Through Rate** | 5%+ | (Klicks / Öffnungen) × 100 |
| **Conversion Rate** | 10%+ | (Konversionen / Leads) × 100 |
| **Unsubscribe Rate** | <0,5% | (Abmeldungen / Versendungen) × 100 |
| **Bounce Rate** | <2% | (Bounces / Versendungen) × 100 |
| **Days to Conversion** | <14 | Durchschnittliche Tage bis Konversion |

### 6.2 Dashboard-Elemente

- **Funnel-Visualisierung:** Leads → Öffnungen → Klicks → Konversionen
- **Segment-Performance:** Vergleich der Konversionsraten nach Segment
- **E-Mail-Performance:** Open Rate, CTR pro E-Mail
- **Lead-Scoring-Verteilung:** Anzahl Cold/Warm/Hot Leads
- **Zeitreihen-Analyse:** Trends über Zeit

---

## 7. Optimierungs-Roadmap

### Phase 1: Basis-Setup (Woche 1-2)
- [ ] Automations-Sequenz konfigurieren
- [ ] E-Mail-Templates erstellen
- [ ] Segmentierung einrichten
- [ ] Tracking aktivieren

### Phase 2: Monitoring (Woche 3-4)
- [ ] Erste Metriken sammeln
- [ ] Engagement-Raten überprüfen
- [ ] Bounce-Raten analysieren
- [ ] A/B-Tests vorbereiten

### Phase 3: Optimierung (Woche 5+)
- [ ] A/B-Tests durchführen
- [ ] Betreff-Zeilen optimieren
- [ ] CTA-Texte testen
- [ ] Versendungszeiten optimieren
- [ ] Segment-Inhalte anpassen
