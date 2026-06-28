# Handoff — Malaga Sommerferie Dashboard

## Goal
Et interaktivt familie-feriedashboard til Sajids Malaga-ferie (4–15 juli 2026) med aktivitetsoversigt, flyinfo, dagsplan, pakkeliste per person, lejlighedsvejledning og nedtælling. Deployeret live og tilgængeligt for hele familien.

---

## Hvad er bygget

### Live site
- **URL:** https://malaga-sommerferie.vercel.app
- **GitHub:** https://github.com/saj1980/malaga-sommerferie (public repo, user: `saj1980`)
- **Lokale filer:** `/Users/sajidlatif/Documents/Claude/Projects/Malaga Sommerferie/`
  - `malaga-dashboard.html` — master-filen, redigér altid denne
  - `index.html` — kopi af ovenstående, bruges af Vercel (synkronisér altid begge)
  - `vercel.json` — `{ "cleanUrls": true, "trailingSlash": false }`
  - `Malaga Ferie - Muligheder & Plan.md` — original markdown-oversigt

### Indhold på siden
1. **Header** med nedtælling (dage til 4. juli, beregnet dynamisk), temperatur-stats
2. **Flykort** — SK2807 CPH→AGP 06:50–10:40 lørdag 4. jul · SK588 AGP→CPH 20:10–23:45 onsdag 15. jul *(booking-reference XQRN4G er bevidst fjernet fra offentlig side)*
3. **Aktivitetskort** (17 stk) med filter-knapper (Sport, Strand, Emily, Kultur, Dagture, Gratis, Mad)
4. **12-dages dagsplan** med klik-for-klaret afkrydsning (gemmes i localStorage)
5. **Husk at booke-tips** — Caminito, delfinsafari, gratis søndage
6. **Lejlighedsvejledning** — 5 kort: adresse & ankomst, alarm (Verisure), varme & AC (Daikin/KNX), lys & WiFi (Casambi), praktisk (grill, vaskemaskine, sengetøj)
7. **Pakkeliste** med faner per person (Far / Mor / Emily / Noah / Elias), afkrydsning gemmes i localStorage per enhed

### Lejlighedsdetaljer
- **Adresse:** Block 9, App. A · 2. sal · Av. Fuengirola 5, 29640 Fuengirola, Malaga
- **Maps:** https://maps.app.goo.gl/JhDSZyh4ABheq4gT6 (Google Maps viser forkert — brug dette link)
- Port: øverste knap på fjernbetjening · Garage: nederste knap · P-pladser 33 & 34
- Alarm: Verisure-nøglering til panel inden til højre for døren
- Varmtvand: skab inden køkken, skift fra Holiday Mode → Auto ved ankomst
- WiFi: router på tv-reol
- Lys: Casambi-app (App Store/Google Play)

---

## Hvad virkede

### Deploy-workflow (vigtigt — brug dette præcist)
```bash
# Redigér malaga-dashboard.html, sync til index.html, commit, push, deploy:
cp malaga-dashboard.html index.html
git add -A && git commit -m "Beskrivelse" && git push
printf "n\n" | npx vercel --prod --yes
```

Vercel CLI er logget ind via Mac-brugerens keychain. Kør via AppleScript i background:
```applescript
do shell script "/bin/bash -l -c 'cd \"/Users/sajidlatif/Documents/Claude/Projects/Malaga Sommerferie\" && cp malaga-dashboard.html index.html && git add -A && git commit -m \"...\") && git push && printf \"n\\n\" | npx vercel --prod --yes > /tmp/vercel-out.txt 2>&1; echo done' &"
```
Vent 18 sek, læs derefter `/tmp/vercel-out.txt` for at bekræfte deploy.

### GitHub CLI
`gh` er installeret og logget ind som `saj1980` (keyring). Virker via `do shell script "/bin/bash -l -c '...'"`.

---

## Hvad virkede IKKE

- **Bash-sandbox (`mcp__workspace__bash`) til interaktive processer** — timeout ved alt der kræver stdin (vercel, gh auth). Brug osascript `do shell script` i stedet.
- **`mcp__workspace__bash` til vercel** — ikke logget ind, timeout på 20 sek.
- **Chrome-extension til Vercel-deploy** — Claude in Chrome var ikke forbundet i tidligere session.
- **osascript med quote-escaping** — dobbelt-indlejrede single quotes crasher. Brug `\"` og `\'` konsekvent.
- **Terminal (click-tier)** — kan ikke skrive til terminal via computer-use. Brug osascript eller bash-tool til shell-kommandoer.

---

## Aktivitetsliste (acts[]) — verificerede links

Alle 17 aktivitetslinks er verificeret og rettede pr. juni 2026:

| # | Titel | Link |
|---|---|---|
| 1 | Padel — Higuerón Club | higueronresort.com/en/sport/facilities/ |
| 2 | Bioparc Fuengirola | bioparcfuengirola.es/en/entries/ |
| 3 | Monowa Butterfly Park | monowa.es/en/infos-visite |
| 4 | Parque de la Paloma | Google Maps |
| 5 | Júzcar Smølf-landsby | juzcar.es |
| 6 | Caminito del Rey ⚡ | caminitodelrey.info/en/tickets/buy |
| 7 | Go-kart Costa del Sol | getyourguide.com/malaga-l402/activities-tc1/ |
| 8 | Aqualand Torremolinos | aqualand.es/torremolinos/en/ |
| 9 | Los Boliches strand | Google Maps |
| 10 | Delfinsafari Benalmadena ⚡ | getyourguide.com/benalmadena-l429/…t391414/ |
| 11 | Kajak & snorkel La Herradura | getyourguide.com/la-herradura-l78895/ |
| 12 | Alcazaba + Gibralfaro | alcazabaygibralfaro.malaga.eu/en/visits/ |
| 13 | Malaga by — Soho & havn | visita.malaga.eu/en/what-to-see-and-do |
| 14 | Ronda — Puente Nuevo | turismoderonda.es/en/ |
| 15 | Nerja + Burriana | nerja.es/en/ |
| 16 | El Torcal, Antequera | torcaldeantequera.com/en/information/ |
| 17 | Paella-kursus | getyourguide.com/malaga-l402/…t58024/ |

⚡ = book tidligt (sælger ud i juli)

---

## Mulige næste skridt

- [ ] Tilføj vejr-widget (open-meteo.com gratis API for Fuengirola)
- [ ] Tilføj budget-overblik med estimerede totalomkostninger
- [ ] Tilføj praktisk info-sektion (nødnumre, taxaapp, apotek)
- [ ] Password-beskytte siden (kræver Vercel Pro) eller gøre GitHub-repo privat
- [ ] Tilpas dagsplanen yderligere (aktiviteterne kan redigeres i `dayPlan[]`-arrayet i JS)

---

*Sidst opdateret: 28. juni 2026*
