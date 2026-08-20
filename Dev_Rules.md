# Fejlesztési Szabályzat — Vizsga Projekt

Ez a dokumentum leírja azokat a szabályokat, amiket mindenki követ fejlesztés közben, hogy elkerüljük az összevisszaságot és a felesleges konfliktusokat.

---

## 1. Branch struktúra

```
main        ← stabil, összeépíthető állapot. Ide csak Pull Request-tel kerül kód.
Frontend    ←
Backend     ←
Database    ←
```

**Szabályok:**
- Mindenki **csak a saját mappájában** módosít fájlokat a saját branch-én (`Frontend/`, `Backend/`, `Database/`).
- Senki nem nyúl bele más mappájába a saját branch-éről.
- `main`-re **soha nem commitolunk közvetlenül** — csak Pull Request-en keresztül.

---

## 2. Pull Request folyamat

1. Amikor egy feature kész és **működik lokálisan** → PR nyitása: `saját branch → main`
2. A PR leírásában röviden: mit csinál, mit érint, hogyan tesztelted
3. Legalább **1 másik csapattag review-zza**, mielőtt mergelünk
4. Merge után a PR szerzője értesíti a többieket, ha van olyan változás, ami érinti őket (pl. új API endpoint)

---

## 3. Szinkronizáció (`main` pull-olása)

- **Minden sprint elején/közepén** mindenki pull-olja `main`-t a saját branch-ébe:
  ```bash
  git checkout Backend
  git pull origin main
  ```
- Ha valaki tudja, hogy változás történt ami érinti a másikat (pl. új API válasz formátum) → **azonnal** szólni, nem várni a sprintre.
- Ne halmozódjon fel hetekig el nem húzott `main` — kisebb, gyakoribb pull = kevesebb konfliktus.

---

## 4. Közös fájlok kezelése

Ezekhez a fájlokhoz **mindenki hozzáférhet, de csak egyeztetés után módosítjuk**, mert ezek a leggyakoribb konfliktus-források:

| Fájl | Ki nyúlhat hozzá |
|---|---|
| `docker-compose.yml` | Csak egyeztetés után, PR-ben jelezve |
| `README.md` | Bárki, de kis, külön commitban |
| `.env.example` | Aki új env változót vezet be, jelezze a csapatnak |
| `API.md` | Frontend + Backend közösen karbantartja |

Ha egy közös fájlt módosítasz, **külön, kis PR-ben** csináld, ne rejtsd el egy nagyobb feature commit közé.

---

## 5. Commit üzenetek

Formátum:
```
<JIRA-ticket-ID>: <rövid leírás>
```
Példák:
```
SCRUM-12: user login endpoint hozzáadva
SCRUM-15: eszköz feltöltő form UI
SCRUM-9: adatbázis séma - foglalások tábla
```
Ez biztosítja, hogy a Jira ticket automatikusan mutassa a kapcsolódó commitokat.

---

## 6. Jira / Scrum

- Minden feladat **Jira ticketként** indul, mielőtt kód íródik hozzá
- Sprint hossz: [töltsétek ki — pl. 1 hét]
- Sprint elején: tervezés, ticketek felvétele
- Sprint végén: Review (mi készült el) + rövid Retro (mi ment jól/rosszul)
- Napi gyors egyeztetés (akár chatben): ki mit csinál, van-e blokkoló

---

## 7. Mappastruktúra

```
/VizsgaProjectRepo
├── Frontend/       ← React + TypeScript
├── Backend/         ← PHP
├── Database/         ← MySQL séma, init scriptek
├── docker-compose.yml
├── .env.example
├── .gitignore
├── API.md
└── README.md
```

---

## 8. Docker

- Minden komponens **saját Dockerfile-lal** rendelkezik a saját mappájában
- A `docker-compose.yml` a `main`-en él, ez fogja össze az egészet
- Fejlesztés közben mindenki lokálisan is tudja futtatni a saját részét külön (pl. `docker build ./Backend`), nem kötelező mindig a teljes compose-t indítani

---

## 9. Alapszabályok, amiket mindenki betart

- ❌ Ne commitolj `.DS_Store`, `node_modules`, `vendor`, `.env` fájlokat — mindezek a `.gitignore`-ban vannak
- ❌ Ne commitolj közvetlenül `main`-re
- ❌ Ne nyúlj más mappájába a saját branch-eden
- ✅ Kis, gyakori commitok — ne egy hatalmas commit egy hét munkával
- ✅ PR előtt teszteld lokálisan, hogy tényleg működik
- ✅ Ha bizonytalan vagy egy közös fájl módosításában, kérdezz a csapatban mielőtt commitolsz
