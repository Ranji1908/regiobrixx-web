# Regiobrixx — Landing Page

Statische Landingpage für **Regiobrixx by Hermibricks** (individuelle Klemmbaustein-Sets
für touristische Destinationen). Inhalte stammen aus dem Konzeptdokument „Regiobrixx".

## Dateien

| Datei | Inhalt |
|---|---|
| `index.html` | Startseite (Hero, Modelle, Referenz, Ablauf, Pakete, Vorteile, Vertrieb, Über uns, Kontakt) |
| `impressum.html` | Anbieterkennzeichnung nach § 5 DDG |
| `datenschutz.html` | Datenschutzerklärung |
| `fonts.css`, `fonts/` | Lokal ausgelieferte Schriften, erzeugt aus Google Fonts |
| `styles.css` | Design-Tokens, Layout, Komponenten, Responsive- und Print-Regeln |
| `main.js` | Mobile-Navigation, Parallax, Scroll-Reveal, Kontaktformular |
| `images/` | Konzeptbilder der Modelle und Fotos des Altmühltal-Sets |
| `.github/workflows/deploy.yml` | Veröffentlicht die Seite bei jedem Push auf `main` via GitHub Pages |

## Lokal ansehen

Einfach `index.html` im Browser öffnen — kein Build, keine Abhängigkeiten,
keine Internetverbindung nötig. Die Seite lädt nichts von fremden Servern nach.

## Schriften

Instrument Serif und Inter liegen als woff2 unter `fonts/` und werden über
`fonts.css` eingebunden. Bewusst **nicht** über Googles CDN: sonst ginge bei
jedem Seitenaufruf die IP-Adresse der Besucher an Google in die USA — in
Deutschland ein bekanntes Abmahnrisiko und ein Punkt, der sonst in die
Datenschutzerklärung müsste.

Enthalten sind nur die tatsächlich benutzten Schnitte (Instrument Serif 400,
Inter 400/500) in den Subsets `latin` und `latin-ext`, zusammen rund 162 KB.
Inter kommt als Variable Font, beide Gewichte teilen sich dieselbe Datei.
Wer einen weiteren Schnitt braucht, muss ihn in `fonts/` und `fonts.css`
ergänzen — ein `font-weight`, das dort fehlt, wird vom Browser nur synthetisch
nachgebildet.

## Gestaltung

Ruhiges, redaktionelles Layout: warmes Off-White (`#FBFAF8`), Serif-Headlines
(Instrument Serif) über Inter-Fließtext, Haarlinien statt Rahmen und Schatten,
keine Flächenfarben. Die Farbe kommt ausschließlich aus den Produktfotos.
Alle Maße liegen als Custom Properties am `:root` in `styles.css`.

### Parallax

Jedes Bild mit `data-parallax="<faktor>"` sitzt in einem `.parallax`-Rahmen mit
fester Seitenratio. Der Rahmen schneidet das Bild oben und unten um `--range`
zu kurz ab; `main.js` verschiebt es beim Scrollen innerhalb dieses Spielraums
über `--shift`. Der Faktor steuert die Stärke (0.35 dezent bis 0.7 kräftig).
Ohne JavaScript oder bei `prefers-reduced-motion: reduce` bleiben die Bilder
mittig stehen — das Layout ändert sich dadurch nicht.

## Inhaltliche Festlegungen

- Die Galerie zeigt **Konzeptbilder**, keine lieferbaren Produkte — die Bildunterschriften
  sind entsprechend mit „Konzept" ausgezeichnet.
- Paketpreise stehen bewusst **nicht** auf der Seite („Preis auf Anfrage"), damit kleinere
  Destinationen nicht vorab abspringen. Der Leistungsumfang bleibt vollständig sichtbar.
- Die Argumente sind auf die Abschnitte verteilt, ohne sich zu wiederholen: „Warum
  Klemmbausteine" trägt die Produktargumente, „Ablauf" den Prozess, „Ihre Vorteile" die
  kaufmännische Sicht, „Vertrieb" die Rechte.

## Deployment

Die Seite läuft auf **GitHub Pages**. Jeder Push auf `main` stößt den Workflow
`.github/workflows/deploy.yml` an, der alles außer `.git`, `.github` und
`README.md` veröffentlicht. Es gibt keinen Build-Schritt.

### Einmalige Einrichtung

1. **Repo-Settings → Pages → Source** auf `GitHub Actions` stellen.
   Ohne diesen Schritt läuft der Workflow zwar, veröffentlicht aber nichts.
2. **`CNAME`** liegt im Wurzelverzeichnis und enthält `www.regiobrixx.de`.
   Diese Schreibweise muss mit dem Feld *Custom domain* in den Pages-Settings
   übereinstimmen — die Datei gewinnt, das Formular wird beim Deploy
   überschrieben. Die Variante ohne `www` leitet GitHub automatisch hierher
   um.
   Diese Datei muss bleiben: Bei Actions-basiertem Deploy reicht das Eintragen
   der Domain im Pages-Formular **nicht** — ohne die Datei im Repo ist die
   Domain nach dem nächsten Deploy wieder weg.
3. **DNS beim Domain-Anbieter** auf GitHub zeigen lassen. Die konkreten Werte
   zeigt GitHub selbst unter Settings → Pages an, sobald die Domain einge-
   tragen ist. Grundmuster:
   - Hauptdomain (`example.de`): vier `A`-Records auf GitHubs Pages-IPs
   - `www.example.de`: ein `CNAME` auf `<user>.github.io`
4. Nach dem DNS-Wechsel in den Pages-Settings **„Enforce HTTPS"** aktivieren.
   Das geht erst, wenn das Zertifikat ausgestellt ist — das dauert nach der
   DNS-Umstellung meist einige Minuten bis zu einer Stunde.

### Änderungen veröffentlichen

Commit auf `main` pushen. Der Workflow-Lauf ist unter *Actions* sichtbar und
braucht rund eine Minute.

## Noch anzupassen

- **Kontaktformular:** öffnet aktuell das Mail-Programm (`mailto:`) mit vorausgefüllter Nachricht.
  Für serverseitigen Versand das `submit`-Handling in `main.js` gegen einen Form-Endpoint tauschen.
- **E-Mail-Adresse:** Überall steht `info@regiobrixx.de`. Sobald das Postfach
  `info@regiobrixx.de` existiert, in `index.html`, `impressum.html`,
  `datenschutz.html` und `main.js` tauschen.
- **Rechtstexte:** Impressum und Datenschutzerklärung sind Entwürfe nach dem
  tatsächlichen Verhalten der Seite, aber juristisch nicht geprüft.
- **Bilder:** für Produktion zusätzlich als WebP/AVIF ausliefern.
