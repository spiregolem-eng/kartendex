# Kartendex – Projektnotiz

Digitales Sammelalbum für meine Pokémon-Karten, geordnet nach dem National Pokédex. Rein privat, nicht kommerziell. Installiert als Web-App (PWA) auf meinem Samsung-Handy.

Stand: November 2026 (Version 2)

## Arbeitsweise mit Claude (Inkognito)

- Claude kennt frühere Chats nicht. Zu Beginn diese Datei und die aktuelle `index.html` einfügen bzw. anhängen (bei Bedarf auch `sw.js` und `manifest.json`).
- Claude liefert Code im Chat. Bei größeren Änderungen die komplette Datei, sonst gezielte Stellen.
- Vor jedem Update: in der App „Backup speichern“.
- Testen ohne Risiko: neue Datei zusätzlich als `vorschau.html` hochladen und in Chrome unter `…github.io/<repo>/vorschau.html` öffnen. Nutzt dieselben Daten wie die App. Danach `index.html` ersetzen und `vorschau.html` löschen.
- Auf GitHub erst nach „Commit changes“ gespeichert. Live nach ca. 1–2 Minuten (Reiter „Actions“ → grüner Haken).
- App nie deinstallieren (Datenverlust möglich). Neue Version: App ganz schließen und neu öffnen, ggf. zweimal.

## Anforderungen & Entscheidungen

### Album
- Alle Pokémon in National-Dex-Reihenfolge, ein Platz pro Nummer (keine Plätze für Regional- oder Mega-Formen).
- Generationen-Header als Banner: „Generation X · Region“, rechts Fortschritt (z. B. 12 / 151), dahinter eine eigene, gezeichnete Landschafts-Silhouette der Region in eigener Farbe (hell/dunkel).
  - Kanto: Fuji · Johto: Pagode · Hoenn: Vulkan und Meer · Sinnoh: Schneeberge und Tannen · Einall: Skyline und Brücke · Kalos: Eiffelturm · Alola: Palmen und Sonnenuntergang · Galar: Uhrturm · Paldea: Windmühlen.
  - Bewusst keine Spiel-Artworks (geschützt) und keine Fotos aus dem Netz (Lizenzen).
- Leerer Platz: Nummer (#0001), deutscher und englischer Name, dezente graue Silhouette.
- Markierung „Keine IR / SIR verfügbar“: färbt einen leeren Platz orange. Ein- und ausschaltbar im Suchfenster unter „Karten suchen“. Nur auf leeren Plätzen sichtbar.
- Genau eine Karte pro Platz; eine neue ersetzt die alte (mit Rückfrage).
- Karten in allen Sprachen möglich; angezeigt wird die Sprachversion, die ich besitze. Fehlt das Bild, wird ersatzweise das deutsche, dann das englische Bild gezeigt (kleines Sprach-Badge mit der Kartensprache).

### Bedienung
- Design ruhig und modern, 3 Kacheln breit, Endlosscroll.
- Zwei runde Buttons unten rechts:
  - Rot (#d9483b) mit Lupe: Suche im Kartendex nach deutschem oder englischem Namen oder Nummer; springt zum Platz.
  - Preußisch Blau (#003153) mit weißem „+“: Karte hinzufügen.
  - Orange ist für die Markierung reserviert.
- Antippen einer Karte: Vollbild; erneutes Antippen schließt. Im Vollbild: „Andere Karte wählen“ / „Eigenes Bild“ / „Entfernen“.
- Menü ⋯: Backup speichern / Backup laden / Pokédex aktualisieren.
- Zurück-Taste von Android schließt die jeweils oberste Ebene.
- Kein Pokéball-Design (geschütztes Design).

### Karten suchen
- Pokémon-Name deutsch/englisch oder Nummer, optional Edition (Freitext oder Vorschlagsliste DE/EN).
- Ergebnisse aller Sprachen, gruppiert nach Sprache.
- Die Karte wird dem gesuchten Pokémon zugeordnet (wichtig bei Tag-Team-Karten).
- Fehlende Vorschaubilder werden automatisch aus weiteren Quellen nachgeladen (Badge zeigt die Bildsprache).
- Ganz oben in den Ergebnissen: „Bild manuell hinzufügen“ (auch wenn nichts gefunden wurde).

### Bildquellen (Reihenfolge)
1. TCGdex in der Kartensprache
2. TCGdex Deutsch, dann Englisch (nicht für JP/KO/ZH, dort andere Karten-IDs)
3. Pokémon TCG API (pokemontcg.io, nur englische Karten). Abgleich über Kartenname, Nummer und englischen Set-Namen.
4. Nichts gefunden: Fenster „Eigenes Bild“ mit Option „Ohne Bild eintragen“.

Mindestanforderung für Online-Bilder: mindestens 500 px breit und Kartenformat (Seitenverhältnis 0,66–0,77).

### Eigenes Bild
- Bild aus der Galerie wählen (vorher z. B. per langem Druck auf ein Bild in Chrome herunterladen).
- Im Kartenrahmen verschieben, zoomen (Regler oder zwei Finger), drehen.
- Gespeichert als 630 × 880 px WebP.
- Warnung, wenn der Ausschnitt unter 450 px breit ist.
- Beim manuellen Hinzufügen optional Edition, Kartennummer und Sprache angeben.

## Technik

### Dateien und Hosting
- Dateien: `index.html` (komplette App), `manifest.json`, `sw.js` (Offline-Start, Network-first), `icon.svg`, `icon-192.png`, `icon-512.png`, `PROJEKT.md`, `README.md`.
- Hosting: GitHub Pages, Branch main / root.

### Datenquellen
- Pokédex: PokéAPI GraphQL (graphql.pokeapi.co/v1beta2), Ersatz: v1beta bzw. REST. 30 Tage lokal zwischengespeichert. „Pokédex aktualisieren“ lädt neu.
- Silhouetten: PokeAPI-Sprites (official-artwork).
- Karten und Bilder: TCGdex REST-API (`api.tcgdex.net/v2/{sprache}/cards?name=…`). Bilder unter `{image}/high.webp` bzw. `low.webp`. Set-Listen 7 Tage zwischengespeichert.
- Zusatzquelle: Pokémon TCG API (`api.pokemontcg.io/v2/cards`), ohne API-Schlüssel. Offiziell veraltet (Nachfolger Scrydex ist kostenpflichtig). Fällt sie aus, gibt es nur weniger Treffer.

### Bewusst NICHT genutzt
- Cardmarket: keine freie API, Scraping verboten.
- tcgcheck.de: keine öffentliche API, automatisierter Zugriff laut robots.txt untersagt, werbe- und spendenfinanziert. Nur manuell als Quelle für eigene Bilder.
- Scrydex: kostenpflichtig, und ein API-Schlüssel wäre in der öffentlichen App sichtbar.

### Speicherung (nur lokal auf dem Gerät)
- localStorage:
  - `kd.coll`: Sammlung `{ PokédexNr: Karte }`
  - `kd.mark`: Liste markierter Pokédex-Nummern
  - `kd.dex`: Pokédex-Cache
  - `kd.sets.{sprache}`: Set-Namen
- IndexedDB `kartendex`, Store `img`: eigene Bilder als Blob, Schlüssel = Pokédex-Nummer.
- Die App fordert dauerhaften Speicher an (`navigator.storage.persist`).

Felder eines Karteneintrags:

| Feld | Bedeutung |
|------|-----------|
| `cardId` | TCGdex-ID; `null` bei manuellem Eintrag |
| `lang`, `label` | Kartensprache (z. B. `de`, `DE`) |
| `name`, `set`, `localId` | Kartenname, Edition, Kartennummer |
| `img` | TCGdex-Bildbasis (ohne `/high.webp`) |
| `imgUrl`, `thumbUrl` | volle Bild-URLs (Pokémon TCG API) |
| `imgLang` | Sprache des angezeigten Bildes |
| `src` | Bildquelle: `tcgdex`, `ptcg` oder `own` |
| `own` | `true`, wenn eigenes Bild in IndexedDB |
| `added` | Zeitstempel |

### Backup
- JSON-Datei mit `{ app, v: 2, date, collection, marks, images }`. Eigene Bilder sind als Data-URL enthalten, dadurch wird die Datei größer.
- Laden ersetzt Sammlung und eigene Bilder; Markierungen nur, wenn im Backup enthalten.
- Alte Backups (v1, ohne Bilder und Markierungen) lassen sich weiterhin laden.

### Installation auf Android
- Chrome → ⋮ → „Installieren und Verknüpfung erstellen“ → „Installieren“.
- Dafür sind die PNG-Icons nötig; SVG allein reichte nicht.

## Wartung: neue Generation (z. B. 2027)

1. Die neuen Pokémon kommen automatisch über PokéAPI, sobald diese aktualisiert ist. In der App „Pokédex aktualisieren“ antippen.
2. In `index.html` im Array `GENS` einen Eintrag ergänzen: `[höchste Pokédex-Nr. der Generation, 'Regionsname']`.
3. Im Objekt `ART` einen Eintrag `10: { h: Farbton 0–360, svg: '…' }` ergänzen:
   - Eigene Silhouette im Raster 360 × 90, Boden bei y = 90.
   - Ebenen über die Klassen `l1` (hinten), `l2`, `l3` (vorne), `sn` (Schnee/Highlight) und `sun`.
   - Ohne Eintrag erscheint ein neutrales Hügel-Motiv; die Region fehlt im Titel, solange `GENS` nicht ergänzt ist.
   - Farbtöne nahe Orange (ca. 20–40) vermeiden, Orange ist für die Markierung reserviert.
4. Neue Sets erscheinen in TCGdex mit etwas Verzögerung.

## Bekannte Einschränkungen

- Russische und thailändische Karten werden nur gefunden, wenn sie den englischen Namen tragen.
- Bei Japanisch, Koreanisch und Chinesisch sind Fehltreffer möglich (z. B. Mewtu bei Suche nach Mew).
- Für japanische, koreanische und chinesische Karten gibt es außer TCGdex keine freie Bildquelle; sonst bleibt nur das eigene Bild.
- Die Pokémon TCG API bekommt vermutlich keine neuen Sets mehr und kann ganz abgeschaltet werden.
- Fotos im HEIC-Format kann Chrome nicht öffnen; JPG, PNG oder WebP verwenden.
- Die Sammlung existiert nur auf diesem Gerät. Regelmäßig „Backup speichern“.
