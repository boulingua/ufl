# UFL — Ukrainian course roadmap

Build plan for the boulingua **Ukrainian as a Foreign Language** course (`ufl`,
signature accent `#C723A1`). This repo is currently a scaffold (LICENSE, README,
brand icons) marked *coming soon*. This document is the actionable path to a
complete course — content **and** website — instantiated from `pagegen`, mapped
to the boulingua `curriculum` framework, and compliant with the VG Wort standard.

---

## 1. Overview

**What it is.** A free, openly-licensed Ukrainian course for German
*Gesamtschule* learners and independent adult learners, following the boulingua
five-step unit model (Activate → Input → Practise → Apply → Reflect), with
committed slide decks, printable worksheets, native-voice audio, and exams as
first-class deliverables.

**Target learners.** German-L1 (and other L1) adults and secondary pupils, many
with a concrete real-world motive — displacement since 2022, family ties,
volunteering, or work with Ukrainian speakers. Assume **no prior Cyrillic**.

**Realistic CEFR scope.** A dedicated **Level 0 (script onboarding)** →
**A1 → A2 → B1**. Target conformance **`core` (A1–B1)**. B2/C1 are declared
gaps for a later stretch, not promised up front (§4).

**Defining challenges (language-specific):**

1. **Cyrillic from zero.** Ukrainian uses a 33-letter Cyrillic alphabet. Learners
   cannot read the target language on day one — reading itself is a prerequisite
   skill, not an assumed one. This forces a Level 0 stage and shapes every early
   unit (transliteration scaffolding, sound-spelling drills).
2. **The Ukrainian-specific letters `ґ і ї є`.** These distinguish Ukrainian from
   Russian Cyrillic and are a frequent failure point for fonts, keyboards, TTS,
   and learners coming via Russian. `і`/`и`, `е`/`є`, `г`/`ґ` contrasts, and the
   apostrophe (`'`) must be taught, rendered, and pronounced correctly everywhere.
3. **Rich inflection.** Seven cases, three genders, aspectual verb pairs, and a
   vocative case make even A1 grammar heavier than in Germanic/Romance courses —
   the curriculum mapping and appendix tables must carry more weight.
4. **TTS/voice correctness.** Native-voice audio must use a genuinely Ukrainian
   Piper voice (not a Russian fallback), or the `ґ/ї/є` and stress patterns will
   be wrong. Availability must be verified before audio phases start (§2, §6).
5. **Strong, time-sensitive demand.** Interest in Ukrainian is high right now.
   That argues for shipping a usable MVP (Level 0 + A1) early rather than
   withholding until the full A1–B1 course is complete (§8).

---

## 2. Language & localisation decisions

Each decision below is a recommendation, not an open question.

- **Script / writing system.** Ukrainian Cyrillic only, in NFC-normalised UTF-8.
  **Decision:** every glossed target-language token in early units carries a
  Latin **transliteration** (ISO 9 / national system) plus an IPA hint, phased
  out through A1. Handwriting (cursive Cyrillic differs markedly from print) is
  taught in Level 0 as an optional worksheet track, not required for progression.
- **Variant / standard.** **Decision:** Standard literary Ukrainian
  (Наддніпрянський-based standard, 2019 orthography — «Український правопис»
  2019). No dialect coverage; note surzhyk and regional variation as
  intercultural asides only.
- **Directionality.** LTR — **no RTL work** (unlike the `afl` Arabic course).
- **Web fonts / script coverage.** hugo-coder's stack must render full Ukrainian
  Cyrillic **including `ґ і ї є` and the apostrophe**. **Decision:** verify the
  theme's system-font stack shows these four letters correctly across
  Windows/macOS/Linux/Android; if any glyph falls back or boxes, self-host a
  libre Cyrillic-covering face (e.g. a Ukrainian-tested cut of a SIL/OFL family)
  as a `woff2` under `static/` and wire it via `assets/css/custom.css` — the only
  sanctioned CSS touch-point. Do **not** edit the accent system (driven by
  `data/accents.yaml`, `code = ufl`, already defined).
- **Keyboard / input guidance.** Ship a short "typing Ukrainian" note in
  *get-started* (Ukrainian keyboard layout, the `ґ`/apostrophe positions,
  phonetic input options) — learners must be able to type answers.
- **Native voice / Piper TTS.** **Decision:** use a genuine Ukrainian Piper voice.
  rhasspy/piper-voices publishes Ukrainian voices (e.g. `uk_UA-ukrainian_tts-*`,
  `uk_UA-lada-x_low`). **Action — verify before Phase A2 audio:** add the chosen
  `uk_UA-*` entry to `audiogen/get_voices.sh`, download, and spot-check `ґ ї є`,
  the apostrophe, and word stress on real sentences. A second `uk_UA-*` voice is
  desirable for dialogue alternation; if only one exists, alternate by
  pitch/speed. **Never** substitute a Russian voice.
- **Level-0 script onboarding stage.** **Decision:** a real, first-class course
  section `content/level-0/` (script onboarding) shipped **before** A1 — see §5.

---

## 3. Instantiation from pagegen

Stand up the buildable site by copying the template and changing only the marked
values (per `pagegen/README.md` step list). Concretely:

1. **Copy the template** into this repo, preserving `pagegen`'s layout, scripts,
   gates, `go.mod`, `layouts/`, `archetypes/`, `data/`, and CI. Do **not** copy
   `public/`. Keep the existing `ufl/README.md`, `LICENSE`, and `brand/`.
2. **Edit `hugo.toml`** (only the marked keys):
   - `baseURL = "https://boulingua.github.io/ufl/"`
   - `title = "Ukrainian — S. Le Boulanger"`
   - `languageCode = "uk"`, `defaultContentLanguage = "uk"`
   - `[params].navTitle = "Українська"` (or "Ukrainian"), `description`,
     `keywords = "Ukrainian,Ukrainian language,Cyrillic,CEFR,curriculum,OER"`
   - `params.code = "ufl"` (selects the `#C723A1` accent from `data/accents.yaml`)
   - `[[params.social]].url = "https://github.com/boulingua/ufl"`
   - `[params.plausible].domain = "boulingua.github.io/ufl"` (kept **last**, per
     the TOML sub-table trap warning)
   - `[[menu.main]]` sections mirroring `content/`: **Level 0**, **A1**, **A2**,
     **B1**, **Materials** (Slide decks / Worksheets), **About**, **Legal**.
3. **Brand.** `data/accents.yaml` already carries `ufl` (`#C723A1` / hover
   `#A01C82` / dark `#E77ECE` / dark_hover `#EFA9DE`) — confirm, do not edit.
   Regenerate the pentagon + favicons: `python brand/make_icon.py` (writes into
   `static/` / `brand/`). The existing `brand/icon.svg|png` are already on-hue.
4. **Legal.** Fill the ⟨…⟩ placeholders in `impressum.md`, `datenschutz.md`,
   `haftungsausschluss.md`. In `datenschutz.md` keep the VG Wort METIS disclosure.
   Once filled, remove `|| true` from the legal-placeholder gate in CI.
5. **First green build.** `hugo --minify --gc` locally; fix any theme glyph/menu
   issues; push to `main`; confirm `.github/workflows/build-deploy.yml` runs the
   gate battery and deploys to **GitHub Pages**. This is the "empty but live"
   milestone — the site builds and serves before any content lands.

---

## 4. Curriculum conformance target

**Declared target: `core` (A1–B1).** This is the honest, reachable target for a
free Ukrainian OER; B2/C1 are declared as `no-coverage` gaps, not silently
omitted (per `curriculum/docs/conformance.md` — declaring a gap is correct, not a
defect). Level 0 is pedagogical onboarding and maps to `preA1` descriptors where
they exist.

**Scope declaration (required, machine-readable).** Publish a
`conformance.yml` in this repo (mirroring `curriculum/examples/de-a1/`) that
names `framework: boulingua-curriculum`, `framework_version`, `language: uk`,
`declared_conformance: core`, and — per in-scope scale — the levels covered vs.
`no-official-descriptor`. The registry has **88 scales, 79 in-scope**
(`SIGN` excluded). For `core` we must touch **every in-scope scale that has an
A1/A2/B1 descriptor**; a scale the CV leaves empty at a level is satisfied by
recording `no-official-descriptor` — a *silently* missing scale is the only
conformance failure.

**Implement vs. declare-empty:**

- **Implement fully** the high-frequency activity scales that carry A1–B1
  descriptors: `REC.*` (oral/reading comprehension, orientation, instructions),
  `PROD.*` (oral/written production, sustained monologue), `INT.*` (conversation,
  information exchange, transactions, written interaction), core `LING.*`
  (general range, vocabulary, grammatical accuracy, phonological control — the
  last is load-bearing for Cyrillic), `SOC.sociolinguistic-appropriateness`,
  and `PRAG.*` (coherence, spoken fluency, turntaking).
- **Thin / declare-empty where the CV has no descriptor at these levels or the
  material is unavoidably light:** most `MED.*` (mediation) and `PLUR.*`
  (plurilingual/pluricultural) scales — implement the few with A2/B1 descriptors
  (e.g. `MED.processing-text`, `MED.collaborating-to-construct-meaning`),
  `no-official-descriptor` the rest. `SIGN` is out of scope entirely.

**Mapping units → descriptor IDs.** Every unit's front-matter `curriculum` block
uses `framework: cefr` with `cefr_level` and `cefr_can_do`. Each can-do resolves
to a curriculum ID `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` (e.g.
`A1.INT.information-exchange.01`, `A2.REC.reading-for-orientation.02`). Maintain a
unit↔ID coverage manifest; run `curriculum/scripts/id-audit.sh` (format, global
uniqueness, `implements:` resolves to a real in-scope (scale, level)) as a
**course gate** so no unit references a non-existent descriptor.

---

## 5. Content creation plan

Phased by CEFR level; effort tags **S / M / L** per phase. Recurring cast and
themes give continuity: follow **Оксана** (a student in Lviv), **Марко** (her
cousin abroad), and **пані Коваленко** (a teacher) across units — the same cast
threads dialogues, reading texts, and audio from Level 0 through B1.

**Phase 0 — Script onboarding (`content/level-0/`) · L.**
~6 units: (0.1) the alphabet in blocks + `ґ і ї є` contrasts; (0.2) sound↔letter
mapping and stress; (0.3) reading first words/names; (0.4) numbers, days, basic
signs; (0.5) the apostrophe, soft sign, iotated vowels; (0.6) print↔cursive +
typing. Maps to `preA1` where descriptors exist. Acceptance: a learner can decode
any Ukrainian word aloud and type `ґ`/apostrophe correctly.

**Phase 1 — A1 (`content/level-a1/`) · L.**
~14 units: greetings/introductions, family, home, food/shopping, daily routine,
directions, time, weather, nominative + accusative basics, present tense, gender,
plurals. Acceptance: every A1 in-scope scale mapped; each unit has objectives,
input, practise, produce, reflect; answer keys + teacher notes; ≥1 committed
deck + worksheet; sibling exam.

**Phase 2 — A2 (`content/level-a2/`) · L.**
~16 units: past tense + aspect intro, more cases (genitive, dative, locative,
instrumental), travel, health, work, past experiences, comparisons, future,
prepositions of place/motion. Acceptance as A1, plus audio for all dialogues/
texts using the verified `uk_UA-*` voice.

**Phase 3 — B1 (`content/level-b1/`) · L.**
~18 units: opinions/argument, conditionals, reported speech, aspect in depth,
vocative in use, work/study, media, culture/history (openly-licensed sources
only), abstract topics. Acceptance as A2, plus mediation-scale coverage where
descriptors exist.

**Exams — first-class siblings.** Each unit gets a sibling `…-exam/` bundle
(`--kind exam`), plus one **level exam** per level (Level 0, A1, A2, B1) with
`duration_min`, `total_points`, `notenschluessel`, and the module skills.
Committed exam PDFs live under `static/downloads/<level>/`.

**Appendices (`content/appendices/`) · M.** Alphabet & pronunciation chart;
Cyrillic↔Latin transliteration table; the seven cases (declension tables);
verb aspect pairs; number/time reference; Ukrainian↔German false friends &
common errors; cultural/intercultural notes; assessment grid. Each ≥1800 chars
carries a VG Wort mark (§7).

**Per-phase acceptance criteria (all phases).** (a) every unit's `curriculum`
block resolves via `id-audit.sh`; (b) `hugo --gc` builds clean; (c) VG Wort
coverage audit shows **0** unregistered editorial pages; (d) materials committed
and thumbnailed; (e) exams present and downloadable; (f) link-check green.

---

## 6. Website & materials

- **Section landings via shortcodes.** Every `_index.md` (`page_type: section`)
  uses `{{< hero >}} / {{< kicker >}} / {{< lead >}}` and `{{< card-grid >}}` /
  `{{< card >}}` for unit tiles — **never raw HTML** (per the standard). Level
  hubs, Materials hub, About all use the shortcode set already in
  `layouts/_shortcodes/`.
- **Materials pipeline.** Generate decks and worksheets locally from the branded
  **slidegen** (`.odp` + PDF) and **sheetgen** (PDF worksheets) LaTeX/ODF
  templates keyed to the `ufl` accent; **commit** outputs under
  `static/materials/` + `static/downloads/`. CI only *verifies* (`verify_downloads.py`,
  `pdf_attribution.py`) — no TeX Live in the deploy path. Cyrillic in LaTeX needs
  a Unicode engine (XeLaTeX/LuaLaTeX) with a Cyrillic-covering font — confirm the
  slidegen/sheetgen toolchain uses one before Phase 0 materials.
- **Native-voice audio.** Use **audiogen** (Piper) with a verified `uk_UA-*`
  voice (§2). Add it to `audiogen/get_voices.sh`; generate OGG/Opus into
  `static/materials/audio/<unit>/` with transcripts in `data/audio/<unit>.json`,
  paired with on-page transcripts. **Decision/note:** Ukrainian Piper voices are
  published; the exact voice + `ґ/ї/є`/stress quality must be spot-checked before
  audio ships. Audio is committed, not built in CI.
- **Thumbnails & downloads.** `render_thumbs.py` for deck/worksheet previews;
  the `{{< downloads >}}` shortcode surfaces committed files; `verify_downloads.py`
  gate confirms every referenced file exists and is attributed.

---

## 7. VG Wort — pixel assignment for ALL content pages

**Required and non-skippable.** Every content page that is an original creative
Sprachwerk of **≥ 1800 rendered characters** — every unit, every exam, every
appendix, and long-form editorial prose (About, get-started) — receives **one**
VG Wort Zählmarke, per `pagegen/docs/vgwort-standard.md`. Navigation surfaces
(home, Materials hub, tag indexes, paginated continuations) and the **templated
legal pages** (Impressum/Datenschutz/Haftungsausschluss) get **no** mark.

**Process (per the standard):**

1. Draw **fresh public codes** (32-hex `Öffentlicher Identifikationscode`) from
   the author's **T.O.M.** account — never invent codes, never expose the private
   code. One code per work, one work per URL.
2. Register each in `data/vgwort.yaml` keyed by `url:` (base-stripped
   `RelPermalink`) or `path:` (`content/<File.Path>`), with `pixel_url` /
   `public_id`, `min_chars: 1800`, `author`, `registered_at`.
3. Rendering goes through the single resolver `layouts/_partials/vgwort/url.html`
   → `<head>` preload + eager `<body>` `<img>` (no JS, no consent gate,
   `visibility:hidden`, never `display:none`).
4. Record every mark in the **private usage registry** (§8 of the standard) —
   `Used / Projekt=ufl / Sprache=Ukrainian / Niveau / Kurstitel / URL / Pixel_URL`
   — kept **outside** this repo.
5. Gates: **coverage audit** (warning — flags any ≥1800-char editorial page
   without a mark), **render verify** (blocking — every registered pixel appears
   on exactly one URL), **hub guard** (blocking — `met.vgwort.de` absent from the
   Materials hub). Exclude `vgwort.de` from lychee.

**Estimated marks needed (full Level 0 + A1–B1):**
~54 units + ~54 sibling exams + 4 level exams + ~8 appendices + ~2 editorial ≈
**~120 Zählmarken** total. Draw them in tranches as content lands — the MVP
(Level 0 + A1: ~14 units, ~14 exams, ~2 level exams, ~4 appendices, ~2 editorial)
needs roughly **~36 marks** first.

---

## 8. Milestones & sequencing

- **M0 — Site live (empty).** §3 done: template instantiated, `ufl` accent +
  pentagon, legal placeholders filled, first green build on GitHub Pages.
  *Dependencies:* none. *Blocks:* everything.
- **M1 — Font & voice de-risking.** Confirm `ґ і ї є` + apostrophe render in the
  web font and in slidegen/sheetgen (XeLaTeX); verify a real `uk_UA-*` Piper voice
  and stress quality. *Blocks:* materials + audio phases.
- **M2 — Curriculum scaffold.** `conformance.yml` (`core`), unit↔ID manifest,
  `id-audit.sh` wired as a gate. *Blocks:* content acceptance.
- **M3 — MVP: Level 0 + A1 live.** Script onboarding + A1 units, exams,
  materials, audio, VG Wort marks (~36). This is the **public launch** tranche
  given current demand.
- **M4 — A2 complete.**
- **M5 — B1 complete → full `core` coverage.**
- **M6 — Course complete.** All in-scope A1–B1 scales implemented or explicitly
  `no-official-descriptor`; all ≥1800-char pages carry marks; all gates green.

**Definition of done / ready to flip from *coming soon* to *active* on the world
map:** Level 0 + A1 complete and live (M3) — decode-and-type competence plus a
full A1 level with exams, committed materials, native audio, VG Wort coverage at
0 unregistered pages, `id-audit.sh` green, and a published `conformance.yml`.
A2/B1 then extend the live course without another "coming soon" gate.

---

## 9. Open decisions & risks

- **Piper voice quality (risk).** A `uk_UA-*` voice may mishandle stress, `ґ`, or
  the apostrophe. *Mitigation:* spot-check at M1; keep a second voice option;
  never fall back to Russian TTS. Audio phase is gated on this.
- **Font glyph coverage (risk).** Theme system fonts may box `ґ`/`ї`/`є` on some
  platforms. *Mitigation:* self-host an OFL Cyrillic face via `custom.css` only.
- **Orthography drift (decision).** Lock to the 2019 «Український правопис»; note
  where it differs from pre-2019 forms learners may encounter.
- **Transliteration scheme (decision).** Pick one system (recommend the Ukrainian
  national / BGN-PCGN family for names) and apply it consistently; phase out
  through A1.
- **Russian-interference (pedagogical risk).** Learners arriving via Russian
  conflate `і/и`, `г/ґ`, `е/є`, hard/soft signs. *Mitigation:* explicit contrast
  drills in Level 0 and a false-friends/interference appendix.
- **Cursive scope (decision).** Cursive Cyrillic is optional (worksheet track),
  not required for progression.
- **Sources (risk).** Cultural/historical B1 content must use only
  openly-licensed / public-domain sources, correctly cited — no copyrighted text
  reproduced.
- **Conformance honesty (decision).** Ship `core` (A1–B1); declare B2/C1 and thin
  mediation/plurilingual scales as explicit gaps rather than overpromising.
