# Changelog – Redesign-Branch

Alle Änderungen liegen ausschließlich auf dem lokalen Branch `redesign`, **nicht gepusht**, **nicht live**. Chronologisch nach den 5 Schritten aus dem Redesign-Brief.

## Schritt 1 – Audit
- Vollständige Bestandsaufnahme in `AUDIT.md`: Tech-Stack, Sektionsliste, Widerspruchstabelle, Asset-Realitätscheck, Lighthouse-Baseline (Live-Seite: Performance 55 / Accessibility 82 / Best Practices 96 / SEO 100), gemessene WCAG-Kontrastwerte.
- **Korrektur während der Arbeit**: Der erste Audit-Durchlauf basierte versehentlich auf einem 22 Commits alten lokalen Checkout und enthielt dadurch veraltete Aussagen (alte Preise, fehlende Screenshots, falsche CTA-Anzahl). Nach `git pull` korrigiert; Ursache in `AUDIT.md` dokumentiert.

## Schritt 2 – Inhalt & Struktur
- Zeitangaben vereinheitlicht: "5–7 Werktage Setup" + "danach <1 Tag startklar" (an allen Stellen konsistent).
- Zielgruppen-Untergrenze von "ab 3" auf "ab 1 Außendienst-Mitarbeiter" geändert (Nutzerentscheidung).
- Neue FAQ "Was, wenn Sie als Einzelunternehmer ausfallen?" ergänzt – Antwort stützt sich ausschließlich auf die bestehende Datenexport-Garantie, kein erfundener Nachfolgeplan.
- Neue "Pilotphase"-Sektion mit Early-Adopter-Framing zwischen Stats und Preisen eingefügt.
- FAQ auf der Sales-Page von 32 auf 8 kaufentscheidungsrelevante Fragen reduziert; die übrigen 24 Fragen in neue Seite `hilfe.html` ausgelagert (4 Kategorien).
- § 19 UStG-Hinweis aus prominenter Platzierung entfernt.
- Gefundener und behobener Widerspruch: "20 Minuten" vs. "30 Minuten" Gesprächsdauer – Nutzer bestätigte 30 Minuten als korrekt.

## Schritt 3 – Visueller Umbau
- 5 gemessene WCAG-AA-Kontrastfehler behoben (`.section-label`, `.price-name`, `.price-note`, `.stat-shock`, `.step-col p`).
- Feature-Sektion von 9 identischen Karten zu einer gestaffelten Struktur umgebaut: 2 große Feature-Hero-Karten mit echten Screenshots + 4 kompakte + 3 breitere Karten.
- **Bewusst offen gelassen** (mit Nutzer abgestimmt): vollständiger Icon-Set-Austausch und komplettes Layout-Pattern-Audit über alle Sektionen.

## Schritt 4 – Technik & Recht
- Technisches Consent-Gate für GA4 eingebaut: Skript lädt erst nach aktiver Einwilligung, "Ablehnen" gleichwertig zu "Akzeptieren" platziert.
- `impressum.html` und `datenschutz.html` als echte, crawlbare Seiten erstellt (vorher nur Bootstrap-Modals mit `href="#"`). `noindex, follow`.
- Datenschutzerklärung aktualisiert: Google-Fonts-Nutzung (vorher undokumentiert) ergänzt; GA4-Passus auf Einwilligungs-Rechtsgrundlage (Art. 6 Abs. 1 lit. a) umgestellt.

## Schritt 5 – SEO-Subseiten
4 neue eigenständige Landingpages zur Erschließung nicht-markenbezogener Suchbegriffe, alle mit ausschließlich bereits verifizierten/veröffentlichten Inhalten (keine neuen Behauptungen):
- `aussendienst-digitalisieren.html`
- `pauschalpreis-vertriebssoftware.html` (Skynamo/Salesdock-Vergleich, bewusst nur auf öffentlich bekannte Preismodelle beschränkt, mit Disclaimer)
- `aussendienst-software-brauereien.html`
- `crm-alternative-kleine-teams.html`

Alle 4 in `sitemap.xml` aufgenommen.

## Performance- & Accessibility-Pass (nach Schritt 5)
- **Kontrast**: 13 weitere WCAG-AA-Fehler gefunden und behoben (u.a. `.btn-amber` weiß-auf-amber 3.06:1 → neuer Button-Ton `#96660a` 5.0:1, Navbar-Brand, Phone-Mockup-Timestamps, Preise-Sektion-Badges, Consent-Button, Hilfe-Seite-Link).
- **button-name**: Navbar-Toggler und Carousel-Buttons haben jetzt Labels/`aria-label`.
- **heading-order**: 4 Stellen korrigiert (Feature-Karten, Zielgruppen-Items, Prozess-Schritte, Kontaktformular) – h4/h5 sprangen ohne h3 dazwischen; jeweils auf h3 (bzw. Folgeelement auf h4) angepasst.
- **target-size**: Carousel-Indikatoren von 8×28px auf 24×24px Hit-Area vergrößert (visuelle Punktgröße unverändert).
- **Favicon** ergänzt (Blitz-Icon, inline SVG) – behebt 404-Konsolenfehler.
- **Render-Blocking CSS**: Google Fonts CSS und Bootstrap Icons CSS auf `preload`+`onload`-Pattern mit `<noscript>`-Fallback umgestellt.
- **jsDelivr-Abhängigkeit entfernt**: Bootstrap CSS/JS und Bootstrap Icons (inkl. Woff-Fonts) werden jetzt lokal aus `/vendor/` ausgeliefert statt vom CDN geladen.
- Alle 8 HTML-Seiten erhielten Favicon + lokale Vendor-Pfade; die 7 Nicht-Startseiten zusätzlich async Font-Loading.

### Lighthouse-Vergleich (lokal, `python -m http.server`, `redesign`-Branch)
| | Live-Seite (Baseline, Schritt 1) | Redesign vor Perf-Pass | Redesign nach Perf-Pass |
|---|---|---|---|
| Performance | 55 | 70 | 73 |
| Accessibility | 82 | 86 | **100** |
| Best Practices | 96 | 96 | **100** |
| SEO | 100 | 100 | 100 |

**Hinweis zur Performance-Zahl**: Lokal gemessen über Pythons `http.server` (kein Gzip/Brotli, keine Cache-Control-Header, Single-Thread) – das drückt den Wert künstlich, da Netlify in Produktion automatisch Kompression und Cache-Header setzt. Die 73 ist damit eine **Untergrenze**, kein realistischer Produktionswert. Verbleibende Diagnosen (unused CSS/JS, network-dependency-tree) stammen größtenteils aus dem vollen Bootstrap-Bundle, das nur teilweise genutzt wird – eine gezielte Bootstrap-Custom-Build (nur benötigte Komponenten) wäre der nächste Hebel, ist aber nicht Teil dieses Passes.

## Noch offen
- CSS-Subset/Custom-Bootstrap-Build zur Reduktion von "unused CSS" (aktuell ~45–312 KiB je nach Seite).
- Finales Lighthouse-Audit gegen die echte Netlify-Staging-URL (lokale Werte sind nur Richtwerte, siehe oben).
- Vollständiger Layout-Pattern-Audit (aus Schritt 3 zurückgestellt).
- Icon-Set-Austausch (aus Schritt 3 zurückgestellt).
