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

**Hinweis zur Performance-Zahl**: Lokal gemessen über Pythons `http.server` (kein Gzip/Brotli, keine Cache-Control-Header, Single-Thread) – das drückt den Wert künstlich, da Netlify in Produktion automatisch Kompression und Cache-Header setzt. Die Werte sind damit eine **Untergrenze**, kein realistischer Produktionswert. Zusätzlich zeigte der lokale Chrome-Headless-Lauf starke Schwankungen von Durchgang zu Durchgang (59–82 bei identischen Assets, vermutlich durch geteilte CPU-Last in der Sandbox) – die Tabelle unten zeigt daher eine Spanne statt eines Einzelwerts.

## CSS-Subset (nach dem ersten Perf-Pass ergänzt)
- `vendor/css/bootstrap.min.css` per PurgeCSS auf die tatsächlich genutzten Klassen getrimmt: **232 KB → 34,6 KB (–85 %)**. Safelist deckt alle dynamisch von Bootstrap-JS gesetzten Zustandsklassen ab (`show`, `collaps*`, `fade`, `active`, `carousel*`, `accordion*`, `dropdown`, `navbar*`, `modal`, `backdrop`, `disabled`, `invalid`, `valid`, `btn*`, `bg-*`, `text-*`, `d-*`, `col*`, `row*`, `container*`, `nav*`, `form*`, `focus`, `hover`, `visually-hidden`, `offcanvas*`).
- Regressionstest auf allen 8 Seiten: Konsole fehlerfrei, Accordion (FAQ), Carousel-Tab-Wechsel, Preis-Toggle (monatlich/jährlich) und Navbar funktional geprüft per DOM-/Computed-Style-Check.
- Lighthouse Performance über mehrere Läufe: 59 / 73 / 82 (identische Assets, siehe Hinweis oben) – tendenziell verbessert gegenüber dem Stand vor dem CSS-Trim (70), aber wegen der Messschwankung kein sauberer Einzelvergleich möglich.

### Lighthouse-Vergleich, ergänzt
| | Live-Seite (Baseline) | Redesign vor Perf-Pass | Redesign nach Perf-Pass | Redesign nach CSS-Trim |
|---|---|---|---|---|
| Performance | 55 | 70 | 73 | 59–82 (Streuung) |
| Accessibility | 82 | 86 | 100 | 100 |
| Best Practices | 96 | 96 | 100 | 100 |
| SEO | 100 | 100 | 100 | 100 |

## Noch offen
- Finales, verlässliches Performance-Audit gegen die echte Netlify-Staging-URL (lokale Werte sind Richtwerte mit hoher Streuung, siehe oben – erst dort lässt sich der reale Effekt des CSS-Trims sauber messen).
- Verbleibende "unused CSS/JS"-Reste stammen jetzt primär aus Bootstrap-JS (Carousel/Collapse/Accordion-Logik, die nicht jede Seite nutzt) – weiteres Trimmen dort ist möglich, aber riskanter (Verhaltensänderung statt nur Optik) und nicht Teil dieses Passes.
- Vollständiger Layout-Pattern-Audit (aus Schritt 3 zurückgestellt).
- Icon-Set-Austausch (aus Schritt 3 zurückgestellt).
