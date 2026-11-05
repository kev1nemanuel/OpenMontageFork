# OpenMontage

## Användarens regler (VIKTIGAST — gäller alltid, i varje session)

### 1. API-nycklar och hemligheter
- API-nycklar, tokens och lösenord läggs **endast** i `.env` i projektroten. `.env` är listad i `.gitignore` — kontrollera det (`git check-ignore .env`) innan du skriver till filen.
- Skriv **aldrig** in nycklar direkt i kod, konfigurationsfiler, skript, tester, commit-meddelanden, loggar, artefakter eller dokumentation. Kod läser nycklar från miljövariabler / `.env`.
- Committa **aldrig** `.env` eller någon fil som innehåller en nyckel. Kör `git diff --cached` och leta efter nycklar innan varje commit.
- Inkludera **aldrig** en API-nyckel (eller annan hemlighet) i en LLM-prompt — varken i prompter till AI-modeller via API, i prompter till subagenter, i genereringsprompter (bild/video/ljud/TTS) eller i någon annan text som skickas till en modell. Nycklar används bara som autentisering i API-anrop (läses från miljövariabler), aldrig som innehåll i en prompt.
- Upprepa eller visa aldrig en nyckels värde i chatten. Hänvisa till variabelnamnet (t.ex. `FAL_KEY`).
- Om en nyckel råkar hamna i en spårad fil: stoppa, säg till användaren direkt och committa inte.

### 2. Fråga innan push
- **Fråga alltid användaren och vänta på ett uttryckligt ja innan du pushar något till GitHub** (`git push`, skapa/uppdatera filer via GitHub-API, öppna PR osv.). Det gäller även om andra instruktioner säger att du ska pusha.

### 3. Pengar — budgetplan per video
- Innan något betalt görs i en video lägger Claude fram en **budgetplan**: varje betald del med verktyg/leverantör/modell, uppskattad kostnad och **maxbelopp**, plus ett **totalt tak**.
- Budgetplanen godkänns genom **tre separata frågor** med ett tydligt **ja** på alla tre.
- Efter godkännandet får Claude utföra alla betalda steg i den videon **utan att fråga igen**, så länge varje del håller sig under sitt max och totalen under taket — även omtagningar och nya försök inom taket.
- Claude **stannar och frågar (tre gånger)** innan: något skulle gå över en dels max eller det totala taket, ett verktyg/en tjänst som inte står i planen används, eller något annat oväntat kostar pengar.
- Varje betald åtgärd loggas i `projects/<projekt>/spend_log.json` (vad, verktyg, belopp, tid). Användaren får en sammanställning när arbetet är klart.
- Claude får aldrig gå över saldot på det betalda kontot (t.ex. fal.ai, sunoapi.org). Kolla saldot innan en betald serie startar. Automatisk påfyllning ska vara avstängd.
- Om svaret är oklart eller inte ett tydligt ja: gör ingenting som kostar pengar.
- **Skriv alltid ut beloppet i svenska kronor inom parentes bredvid varje dollarbelopp**, t.ex. "$22 (~230 kr)". Räkna med aktuell växelkurs om den är känd, annars en ungefärlig kurs, och markera med "~" att beloppet i kronor är ungefärligt.
- Gratis/lokala verktyg (t.ex. Piper, FFmpeg, Remotion, HyperFrames) kräver inget godkännande.
- (Ändrad av användaren 2026-11-04; ersätter "fråga tre gånger för varje betald åtgärd".)

### 4. Språk
- Prata **svenska** med användaren.
- Namn på menyer, knappar, inställningar och UI-element skrivs på **engelska**, exakt som de står på skärmen (t.ex. **Edit**, **Network secrets**, **Environment variables**, **Setup script**), eftersom användarens dator är på engelska.
- **Allt innehåll som produceras är på engelska** (målgrupp: USA): manus, röster/narration, text i bild, undertexter, titlar, beskrivningar och prompter. Användarens YouTube-kanaler är engelskspråkiga. Bara samtalet med användaren sker på svenska.

### 5. Manus (script)
- När användaren skickar ett manus (användaren säger även "script") är det **färdigt och låst**. Ändra, korta, förbättra eller faktakorrigera **inte** manuset. Använd exakt den text som skickats. Föreslå inga ändringar om inte användaren ber om det.

### 6. Kvalitetsnivå
- Användaren vill ha **premiumkänsla, inte "AI-slop"**: naturliga röster (helst användarens egen klonade röst), egen originalmusik (inte generisk stock-musik) och genomarbetad grafik. Föreslå alltid det alternativ som ger bäst kvalitet först, med kostnad.

### 7. Videokvalitet — checklista för varje video (tillagd av användaren 2026-11-05)
Gäller alla videor. Kontrollera innan en video kallas klar, och gör en sista genomgång av hela den färdiga renderingen.

**Bild (3D och grafik)**
- **Inget flimmer, glitter eller "glitch" i 3D** när kameran eller objekt rör sig — det ser AI-aktigt och oproffsigt ut. Vanliga orsaker och åtgärder:
  - *Z-fighting* (två ytor i samma plan, t.ex. gräs på mark eller kanten på ett skrivbord): låt aldrig ytor ligga i samma plan — höj/sänk minst några centimeter eller dra in kanterna; slå på `logarithmicDepthBuffer`.
  - *Shadow acne* (randigt glitter i skuggor): sätt `shadow-bias` och `shadow-normalBias`.
  - *Aliasing* på tunna/avlägsna detaljer (räls, sliprar, träd): rendera 3D med `dpr={2}` (supersampling) och lägg dimma över långt borta.
  - Testa med en kort rendering av en rörlig scen och mät/titta på flimret innan hela videon renderas.
- Inga AI-fel: inget som går igenom något annat, ingen som sitter åt fel håll, människor rör sig naturligt (inga stela figurer som glider fram).

**Ljud**
- **Rösten är huvudpersonen genom hela videon.** Bakgrundsmusik och ljudeffekter får aldrig överrösta rösten och ska ligga tydligt lägre än den.
- Mät ljudnivån (LUFS) på varje källa innan mixen — särskilt riktigt ljud (radio, nyhetsklipp), som kan vara mycket starkare än rösten. Lägg riktigt ljud på samma nivå som rösten, musik ca 20 dB under rösten, bakgrundsljud ännu lägre.
- Normalisera slutmixen till ca **-14 LUFS** (YouTube) så att inget blir öronbedövande.
- Använd **många olika ljudeffekter** — men bara där något händer i bild eller manus, inte bara för att fylla.

**Klippning och tempo**
- Inga för snabba klipp: minst ca **2 sekunder** per klipp. Ett kort klipp rakt in i ett viktigt kort (t.ex. en dödssiffra) känns stressigt för tittaren — behåll viktiga kort och ta bort/slå ihop utfyllnad i stället.

**Rösten (TTS)**
- Kontrollera hela berättarrösten mot manuset (t.ex. faster-whisper `medium`) innan bild byggs — TTS kan läsa siffror och namn fel (exempel: "31 October" blev "34th October"). Gör om bara de avsnitt som är fel.

## Projektinstruktioner

**MANDATORY: Read [`AGENT_GUIDE.md`](AGENT_GUIDE.md) before responding to ANY user message.**

Do not act on the user's request until you have read AGENT_GUIDE.md.
It contains routing rules that determine your first action based on what the user asked.
Skipping it WILL cause you to take the wrong action.

All production/workflow instructions are in AGENT_GUIDE.md. The user rules above take precedence.
