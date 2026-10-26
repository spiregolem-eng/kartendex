# Kartendex – Projektnotiz

Digitales Sammelalbum für meine Pokémon-Karten, geordnet nach dem National Pokédex.
Rein privat, nicht kommerziell. Installiert als Web-App (PWA) auf meinem Samsung-Handy.

## Anforderungen & Entscheidungen
- Alle Pokémon in National-Dex-Reihenfolge, ein Platz pro Nummer (keine Plätze für Regional- oder Mega-Formen).
- Generationen-Überschriften: „Generation X · Region“, rechts der Fortschritt (z. B. 12 / 151).
- Leerer Platz: Nummer (#0001), deutscher + englischer Name, dezente graue Silhouette.
- Genau eine Karte pro Platz; eine neue ersetzt die alte (mit Rückfrage).
- Karten in allen Sprachen möglich; angezeigt wird genau die Sprachversion, die ich besitze.
  Fehlt das Bild in dieser Sprache, wird das deutsche, ersatzweise das englische Bild gezeigt (mit kleinem Sprach-Badge).
- Suche: Pokémon-Name deutsch/englisch oder Nummer, optional Edition (Freitext oder Vorschlagsliste DE/EN).
  Ergebnisse aller Sprachen, gruppiert nach Sprache. Die Karte wird dem gesuchten Pokémon zugeordnet
  (wichtig bei Tag-Team-Karten).
- Design: ruhig und modern, 3 Kacheln breit, Endlosscroll, runder Button mit weißem „+“ in Preußisch Blau (#003153).
  Antippen einer Karte zeigt sie im Vollbild, erneutes Antippen schließt. Im Vollbild: „Andere Karte wählen“ / „Entfernen“.
- Menü ⋯: Backup speichern / Backup laden / Pokédex aktualisieren.
- Kein Pokéball-Design (geschütztes Design).

## Technik
- Dateien: index.html (komplette App), manifest.json, sw.js (Offline-Start), icon.svg, icon-192.png, icon-512.png.
- Hosting: GitHub Pages, Branch main / root.
- Pokédex-Daten: PokéAPI GraphQL (graphql.pokeapi.co/v1beta2), Ersatz: v1beta bzw. REST. 30 Tage lokal zwischengespeichert.
- Silhouetten: PokeAPI-Sprites (official-artwork).
- Kartendaten & Bilder: TCGdex REST-API (api.tcgdex.net/v2/{sprache}/cards?name=…), Bilder unter {image}/high.webp bzw. low.webp.
  Cardmarket bewusst NICHT genutzt (keine freie API, Scraping verboten).
- Sammlung: nur lokal auf dem Gerät (localStorage), Sicherung über Backup-Datei (JSON).
- Installation auf Android: Chrome → ⋮ → „Installieren und Verknüpfung erstellen“ → „Installieren“.
  Dafür sind die PNG-Icons nötig, SVG allein reichte nicht.

## Bekannte Einschränkungen
- Russische und thailändische Karten werden nur gefunden, wenn sie den englischen Namen tragen.
- Bei Japanisch, Koreanisch und Chinesisch sind Fehltreffer möglich (z. B. Mewtu bei Suche nach Mew).
- Neue Sets erscheinen in TCGdex mit etwas Verzögerung.

## Ideen für später
- Sprungfunktion zu einer Pokédex-Nummer oder Generation
- Akzentfarbe (Aufleuchten, App-Symbol) eventuell ebenfalls in Preußisch Blau
