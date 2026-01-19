# 🚀 Netlify Deployment Anleitung

## Schritt-für-Schritt: Evergreen-Funnel auf Netlify deployen

### 📋 Voraussetzungen
- GitHub Account (bereits vorhanden ✅)
- Netlify Account (kostenlos)
- Dieses Repository: `bmeintel-ai/steinteppich-evergreen-funnel`

---

## 1️⃣ Netlify Account erstellen

1. Gehen Sie zu **https://app.netlify.com/signup**
2. Klicken Sie auf **"Sign up with GitHub"**
3. Autorisieren Sie Netlify für Ihren GitHub Account
4. Wählen Sie einen Team-Namen (z.B. "Bitschus")

✅ **Fertig!** Ihr Netlify Account ist eingerichtet.

---

## 2️⃣ Landingpage deployen

### Option A: Deploy über GitHub (Empfohlen)

1. **In Netlify:**
   - Klicken Sie auf **"Add new site"** → **"Import an existing project"**
   - Wählen Sie **"Deploy with GitHub"**
   
2. **Repository auswählen:**
   - Suchen Sie nach `steinteppich-evergreen-funnel`
   - Klicken Sie darauf

3. **Build-Einstellungen:**
   - **Branch to deploy:** `main`
   - **Build command:** *leer lassen*
   - **Publish directory:** `.` (Punkt)
   - **Build path:** `steinteppich_landingpage.html`

4. **Deploy:**
   - Klicken Sie auf **"Deploy site"**
   - ⏳ Warten Sie 1-2 Minuten

5. **Site-Einstellungen anpassen:**
   - Gehen Sie zu **"Site settings"** → **"General"** → **"Site details"**
   - Klicken Sie auf **"Change site name"**
   - Wählen Sie einen Namen: z.B. `bitschus-steinteppich` oder `steinteppich-ratgeber`
   
✅ **Ihre Landingpage ist jetzt live!**  
URL: `https://ihr-gewählter-name.netlify.app`

---

### Option B: Manuelles Deployment (Schneller Testzweck)

1. **In Netlify:**
   - Gehen Sie zu **"Sites"**
   - Ziehen Sie die Datei `steinteppich_landingpage.html` per Drag & Drop in den Upload-Bereich

2. **Netlify benennt die Seite automatisch:**
   - Sie erhalten eine zufällige URL (z.B. `https://xyz123.netlify.app`)
   
3. **Site umbenennen:**
   - Gehen Sie zu **"Site settings"** → **"Change site name"**

✅ **Fertig!** Das ging schnell, aber bei zukünftigen Änderungen müssen Sie die Datei manuell erneut hochladen.

---

## 3️⃣ Custom Domain einrichten (Optional)

Wenn Sie eine eigene Domain haben (z.B. `steinteppich-ratgeber.bitschus.de`):

1. **In Netlify:**
   - Gehen Sie zu **"Site settings"** → **"Domain management"**
   - Klicken Sie auf **"Add custom domain"**
   - Geben Sie Ihre Domain ein

2. **DNS-Einstellungen:**
   - Netlify zeigt Ihnen die DNS-Einträge an
   - Fügen Sie diese bei Ihrem Domain-Provider hinzu:
     ```
     Type: A
     Name: @ (oder subdomain)
     Value: 75.2.60.5
     ```

3. **SSL-Zertifikat:**
   - Netlify aktiviert automatisch HTTPS (Let's Encrypt)
   - ⏳ Warten Sie 5-10 Minuten

✅ **Ihre Custom Domain ist eingerichtet!**

---

## 4️⃣ PDF Lead Magnet hinzufügen

1. **PDF in Netlify hochladen:**
   - Gehen Sie zu **"Deploys"** → **"Deploy settings"**
   - Klicken Sie auf **"Trigger deploy"** → **"Upload files"**
   - Laden Sie `Landingpage_Evergreen-Leadmagnet_Steinteppich-Sanierung.pdf` hoch

2. **PDF-Link in der Landingpage:**
   - Bearbeiten Sie `steinteppich_landingpage.html`
   - Fügen Sie den Download-Link hinzu:
     ```html
     <a href="/Landingpage_Evergreen-Leadmagnet_Steinteppich-Sanierung.pdf" download>
       Ratgeber herunterladen
     </a>
     ```

✅ **PDF ist jetzt über Ihre Landingpage downloadbar!**

---

## 5️⃣ Formular-Integration

Um Leads zu erfassen, müssen Sie das Formular mit einem Service verbinden:

### Option A: Netlify Forms (Kostenlos, einfach)

1. **In `steinteppich_landingpage.html` ändern:**
   ```html
   <form netlify name=\"steinteppich-kontakt\" method=\"POST\">
     <!-- Formular-Felder bleiben gleich -->
   </form>
   ```

2. **Deploy:**
   - Commit & Push die Änderungen
   - Netlify deployed automatisch

3. **Leads ansehen:**
   - Gehen Sie zu **\"Forms\"** in Netlify
   - Alle Submissions sind dort sichtbar

✅ **Formular funktioniert!**

---

### Option B: Formspree (Empfohlen für E-Mail-Benachrichtigungen)

1. **Formspree Account:**
   - Gehen Sie zu **https://formspree.io/register**
   - Erstellen Sie ein kostenloses Konto

2. **Neues Formular erstellen:**
   - Klicken Sie auf **\"New Form\"**
   - Kopieren Sie die Formular-ID (z.B. `f/abc123def456`)

3. **In `steinteppich_landingpage.html` ändern:**
   ```html
   <form action=\"https://formspree.io/f/YOUR_FORM_ID\" method=\"POST\">
     <!-- Formular-Felder bleiben gleich -->
   </form>
   ```

4. **Deploy:**
   - Commit & Push
   - Netlify deployed automatisch

✅ **Sie erhalten jetzt E-Mails bei jedem Lead!**

---

## 6️⃣ Nächste Schritte

Nach dem Deployment:

- [ ] **CRM einrichten** (HubSpot/Brevo) → Siehe `Quick-Start_ Erste Woche Implementierung.md`
- [ ] **E-Mail-Automation konfigurieren** → Siehe `email_automation.json`
- [ ] **Analytics einrichten** (Google Analytics/Plausible)
- [ ] **Traffic auf die Landingpage leiten** (Social Media, Google Ads)

---

## 🆘 Troubleshooting

### Problem: "Seite lädt nicht"
**Lösung:** 
- Überprüfen Sie den Branch (muss `main` sein)
- Überprüfen Sie die Datei `steinteppich_landingpage.html` existiert im Root

### Problem: "Formular sendet nicht"
**Lösung:**
- Netlify Forms: Überprüfen Sie `netlify` Attribut im `<form>`-Tag
- Formspree: Überprüfen Sie die Formular-ID

### Problem: "PDF nicht downloadbar"
**Lösung:**
- Überprüfen Sie, ob die PDF im gleichen Verzeichnis wie die HTML liegt
- Link muss exakt dem Dateinamen entsprechen

---

## 📞 Support

Bei Fragen:
- Netlify Docs: https://docs.netlify.com
- Formspree Docs: https://help.formspree.io
- GitHub Issues: https://github.com/bmeintel-ai/steinteppich-evergreen-funnel/issues

---

**🎉 Viel Erfolg mit Ihrem Evergreen-Funnel!**
