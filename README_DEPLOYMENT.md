# Netlify Deployment - Anleitung

Die Steinteppich-Sanierung Landing Page ist jetzt vollständig für Netlify vorbereitet.

## ✅ Was bereits fertig ist:

1. **Netlify Forms Integration**: Das Kontaktformular in `steinteppich_landingpage.html` hat die Attribute `name="contact"` und `data-netlify="true"`

2. **Repository Update**: Alle Änderungen sind auf GitHub committed

## 📋 Deployment-Schritte:

### Option 1: Netlify Drop (Einfachste Methode)

1. Gehe zu: https://app.netlify.com/drop
2. Lade alle Dateien aus diesem Repository als ZIP herunter
3. Entpacke das ZIP
4. Ziehe den Ordner direkt auf die Netlify Drop-Seite
5. ✅ Fertig! Deine Seite ist live

### Option 2: Git-Integration (Empfohlen für Auto-Updates)

1. Gehe zu: https://app.netlify.com/start
2. Klicke auf "Import from Git"
3. Wähle GitHub
4. Wähle dieses Repository: `bitschus-steinteppich-sanierung`
5. Build-Einstellungen:
   - **Build command**: Leer lassen (statisches HTML)
   - **Publish directory**: `.` (Root-Verzeichnis)
6. Klicke auf "Deploy site"

## 🔧 Nach dem Deployment:

### E-Mail-Benachrichtigungen einrichten:

1. Gehe im Netlify Dashboard zu: Site settings → Forms
2. Klicke auf "Form notifications"
3. Wähle "Email notification"
4. Füge hinzu: `info@bitschus.de`
5. ✅ Alle Formular-Eingaben werden automatisch an diese E-Mail gesendet

### Eigene Domain verbinden:

1. Im Netlify Dashboard: Site settings → Domain management
2. Klicke auf "Add custom domain"
3. Gib deine Domain ein (z.B. `steinteppich-sanierung.bitschus.de`)
4. Folge den DNS-Konfigurations-Anweisungen:
   - **A-Record**: Zeigt auf Netlify Load Balancer
   - Oder **CNAME**: Zeigt auf deine Netlify-URL

### SSL/HTTPS aktivieren:

- Netlify aktiviert automatisch kostenloses SSL (Let's Encrypt)
- Nach DNS-Propagierung (kann bis zu 24h dauern) ist HTTPS aktiv

## 📧 Netlify Forms - Wie es funktioniert:

Netlify erkennt automatisch Formulare mit `data-netlify="true"` und:
- Sammelt alle Submissions
- Sendet E-Mail-Benachrichtigungen
- Speichert Submissions im Netlify Dashboard
- Bietet Spam-Schutz (optional: reCAPTCHA)

### Formular-Submissions anzeigen:

1. Netlify Dashboard → Forms
2. Dort siehst du alle eingehenden Anfragen
3. Du kannst Submissions auch als CSV exportieren

## 🚀 Fertig!

Deine Landing Page ist jetzt:
- ✅ Live auf Netlify
- ✅ Mit funktionierendem Kontaktformular
- ✅ E-Mail-Benachrichtigungen an info@bitschus.de
- ✅ Automatisches Deployment bei Git-Änderungen
- ✅ Kostenlos gehostet mit SSL

---

**Support**: Bei Fragen → https://docs.netlify.com/
