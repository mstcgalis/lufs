# lufs — Stručný sprievodca

## Čo to je?

`lufs` je nástroj pre príkazový riadok, ktorý meria hlasitosť audio súborov podľa štandardu EBU R128 (LUFS). Hodí sa keď chceš porovnať hlasitosť skladieb pred masteringom, alebo skontrolovať či máš tracky vyrovnané.

## Inštalácia

Potrebuješ [Homebrew](https://brew.sh). Ak ho nemáš, nainštaluj ho príkazom:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Potom nainštaluj `lufs`:

```sh
brew install mstcgalis/tap/lufs
```

Homebrew automaticky doinštaluje aj `ffmpeg`, ak ho ešte nemáš.

---

## Použitie

### Analyzuj všetky audio súbory v priečinku

Prejdi do priečinka so svojimi súbormi a spusti:

```sh
cd ~/Music/moj-album
lufs
```

Výstup bude vyzerať takto:

```
(5 analyzed, 0 cached)

TRACK          INT      TP       ST MAX   LRA
───────────────────────────────────────────────
intro          -14.2    -0.3     -12.1    4.2
verse          -13.8    -0.5     -11.9    3.8
chorus         -12.1    -0.1     -10.5    2.9
```

**Stĺpce:**
- `INT` — integrovaná hlasitosť v LUFS (čím vyššie číslo, tým hlasnejšie)
- `TP` — true peak v dBFS (nesmie presiahnuť 0)
- `ST MAX` — maximálna krátkodobá hlasitosť
- `LRA` — dynamický rozsah v LU

### Analyzuj konkrétne súbory

```sh
lufs track1.wav track2.flac
```

### Zoraď podľa hlasitosti

```sh
lufs -s integrated   # zoraď od najtichšieho po najhlasnejší
lufs -s peak         # zoraď podľa true peak
```

### Zobraz technické info (bit depth, sample rate)

```sh
lufs -i
```

### Vynúť nové meranie (ignoruj cache)

```sh
lufs --no-cache
```

---

## Čo znamenajú upozornenia?

Po tabuľke sa môžu objaviť farebné upozornenia:

**Červené — TP warnings**
True peak presiahol -0.1 dBFS. Hrozí clipping pri konverzii formátov.

**Žlté — Loudness outliers**
Track je o viac ako 1 LU hlasnejší alebo tichší oproti mediánu. Bude znieť nevyrovnane oproti ostatným.

**Žlté — LRA outliers**
Výrazne odlišný dynamický rozsah oproti ostatným trackom.

**`[MONO]`**
Súbor je mono namiesto stereo.

---

## Tipy

- `lufs` si pamätá výsledky v súbore `.lufs_cache` — opakované spustenie je rýchle
- Ak máš audio súbory v git repozitári, pridaj `.lufs_cache` do `.gitignore`
- Podporované formáty: wav, flac, aiff, mp3, ogg, opus, m4a, wma
