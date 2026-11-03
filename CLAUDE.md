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

### 3. Pengar — fråga TRE gånger
- Gör **aldrig** något som kostar pengar (betalda API-anrop för bild, video, röst, musik, avatar osv., betalda tjänster eller köp) utan att först ha frågat användaren **tre separata gånger** och fått ett uttryckligt **ja** på alla tre.
- Varje fråga ska ange: exakt vad som ska göras, vilket verktyg/leverantör/modell, och den uppskattade kostnaden.
- Detta gäller **varje gång** något kostar pengar — även provklipp (samples), omgenereringar och nya försök efter fel. Ett tidigare godkännande gäller aldrig för nästa betalda åtgärd.
- Om svaret är oklart eller inte ett tydligt ja: gör ingenting som kostar pengar.
- **Skriv alltid ut beloppet i svenska kronor inom parentes bredvid varje dollarbelopp**, t.ex. "$22 (~230 kr)". Räkna med aktuell växelkurs om den är känd, annars en ungefärlig kurs, och markera med "~" att beloppet i kronor är ungefärligt.
- Gratis/lokala verktyg (t.ex. Piper, FFmpeg, Remotion, HyperFrames) kräver inte detta.

### 4. Språk
- Prata **svenska** med användaren.
- Namn på menyer, knappar, inställningar och UI-element skrivs på **engelska**, exakt som de står på skärmen (t.ex. **Edit**, **Network secrets**, **Environment variables**, **Setup script**), eftersom användarens dator är på engelska.
- **Allt innehåll som produceras är på engelska** (målgrupp: USA): manus, röster/narration, text i bild, undertexter, titlar, beskrivningar och prompter. Användarens YouTube-kanaler är engelskspråkiga. Bara samtalet med användaren sker på svenska.

### 5. Manus
- När användaren skickar ett manus är det **färdigt och låst**. Ändra, korta, förbättra eller faktakorrigera **inte** manuset. Använd exakt den text som skickats. Föreslå inga ändringar om inte användaren ber om det.

### 6. Kvalitetsnivå
- Användaren vill ha **premiumkänsla, inte "AI-slop"**: naturliga röster (helst användarens egen klonade röst), egen originalmusik (inte generisk stock-musik) och genomarbetad grafik. Föreslå alltid det alternativ som ger bäst kvalitet först, med kostnad.

## Projektinstruktioner

**MANDATORY: Read [`AGENT_GUIDE.md`](AGENT_GUIDE.md) before responding to ANY user message.**

Do not act on the user's request until you have read AGENT_GUIDE.md.
It contains routing rules that determine your first action based on what the user asked.
Skipping it WILL cause you to take the wrong action.

All production/workflow instructions are in AGENT_GUIDE.md. The user rules above take precedence.
