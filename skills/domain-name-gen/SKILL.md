---
name: domain-name-generator
description: Generate short, pronounceable, catchy domain names for products, startups, or projects. Use when the user asks for domain name ideas, brand names, startup names, or wants help naming something with an available web domain.
---

# Domain Name Generator

## Purpose

Generate creative, short, pronounceable, and catchy domain names that are memorable, brandable, and suitable for startups, products, SaaS tools, or side projects.

## When to Use

- User asks for domain name suggestions
- User needs a brand name or product name with a matching domain
- User says "name my startup/app/tool/project"
- User wants catchy, short URLs

## Instructions

### Step 1: Gather Context

Before generating names, understand the user's needs. Ask clarifying questions if not provided:

- **What does the product/service do?** (e.g., "AI-powered invoice processing")
- **Target audience?** (e.g., developers, SMBs, enterprise)
- **Preferred vibe/tone?** (e.g., playful, professional, techy, minimal)
- **Max character length?** (default: aim for 4–8 characters)
- **TLD preference?** (e.g., .com, .io, .ai, .co, .dev, .app)
- **Any words/themes to include or avoid?**

### Step 2: Generate Names Using Linguistic Techniques

Apply these proven techniques to create pronounceable, catchy names:

#### Technique 1: Consonant-Vowel (CV) Patterns
Build names using alternating consonant-vowel patterns for natural pronunciation.
- **CVCV**: Nava, Relo, Kibo, Zumo, Talo, Vero
- **CVCVC**: Molex, Sivot, Kanet, Borek, Lupen
- **CVCCV**: Norta, Belka, Finto, Zarba

#### Technique 2: Blend / Portmanteau
Combine two relevant words by overlapping sounds.
- Cloud + Outline → **Cloutline**
- Ship + Simple → **Shimple**
- Trade + Radar → **Tradar**

#### Technique 3: Truncation + Suffix
Take a root word and add a catchy, modern suffix.
- `-ly`, `-fy`, `-io`, `-zy`, `-ra`, `-va`, `-ix`, `-os`
- Compliance → **Complify**, Invoice → **Invora**, Trade → **Tradix**

#### Technique 4: Invented / Abstract Words
Create entirely new words that *feel* like real words due to familiar phoneme patterns.
- Use common English phoneme clusters: "str-", "bl-", "cr-", "-tion", "-ent", "-ive"
- Examples: Strivon, Blendra, Crestix, Zentiva, Plexo

#### Technique 5: Respelling / Letter Swap
Take a real word and creatively misspell it.
- Quick → **Qwik**, Pixel → **Pyxel**, Simple → **Simpl**

#### Technique 6: Compound Micro-Words
Combine two very short (2-3 letter) real words.
- **AirBit**, **SetFox**, **RunHub**, **ZipOwl**, **GetMesh**

### Step 3: Score and Rank

For each generated name, mentally evaluate on these criteria (don't show scores unless asked):

| Criteria | Weight | Description |
|----------|--------|-------------|
| **Pronounceability** | 30% | Can someone say it correctly on first try? |
| **Memorability** | 25% | Will someone remember it after hearing it once? |
| **Brevity** | 20% | Shorter is better (4-8 chars ideal) |
| **Relevance** | 15% | Does it evoke the right associations? |
| **Uniqueness** | 10% | Does it feel distinct and ownable? |

### Step 4: Check Domain Availability

After generating and ranking names, check availability for all candidates using the domain check API. Batch all generated names across the user's preferred TLDs into a single request:

```bash
curl -X POST http://localhost:3000/api/v1/domains/check \
  -H "Content-Type: application/json" \
  -H "x-device-id: test-device" \
  -d '{"domains": ["name1.com","name1.io","name1.ai","name2.com","name2.io","name2.ai"]}'
```

- Combine every candidate name with each preferred TLD (default: .com, .io, .ai) into the `domains` array
- Parse the response and annotate each name with its availability status
- Prioritize available domains when presenting results

### Step 5: Present Results

Present **10-15 names** organized by technique or theme. For each name:

1. **The name** (with suggested TLD)
2. **How to pronounce it** (phonetic hint if ambiguous)
3. **One-line rationale** (why it works / what it evokes)

#### Example Output Format

```
🔤 Domain Name Ideas for [Product Description]

1. **Kibo.io** ✅ — (KEE-boh) — Short, global-sounding, easy to spell
2. **Tradix.com** ❌ — (TRAY-dix) — Evokes "trade" with a techy suffix
3. **Shimple.co** ✅ — (SHIM-pull) — Blend of "ship" + "simple", playful
4. **Zentiva.ai** ✅ — (zen-TEE-vah) — Abstract, premium feel, flows well
5. **Qwik.dev** ❌ — (KWIK) — Respelling of "quick", dev-friendly
```

### Step 6: Iterate

After presenting initial options, offer to:
- Generate more names in a specific style the user liked
- Explore variations of a favorite (e.g., "You liked Kibo? How about Kibra, Kibex, Kibon?")
- Re-check domain availability for new variations using the API
- Suggest matching social media handle availability

## Anti-Patterns to Avoid

- ❌ Names longer than 12 characters
- ❌ Names with confusing spelling (is it "ph" or "f"? "c" or "k"?)
- ❌ Names that sound like existing major brands
- ❌ Names with awkward consonant clusters (e.g., "Strblx")
- ❌ Names that have negative meanings in common languages
- ❌ Hyphens or numbers in domains
- ❌ Generic dictionary words that are impossible to get as .com

## Tips for Quality

- **Say it out loud** — If you stumble, the user will too
- **The phone test** — Could you tell someone the domain over a noisy phone call?
- **The spell test** — If you say it, can someone type it without asking how to spell it?
- **Two-syllable sweet spot** — The best domain names are often 2 syllables
- **End on a vowel** — Names ending in vowels (Kibo, Nava, Zumo) feel friendlier and more international
