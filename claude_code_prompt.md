# Penguin Travels — Dual Perspective Integration

## Context
Single-player game. Two character routes: **Porky** (Mr Porky, male, Izzy is his girlfriend) and **Squid** (Izzy/Squid, female, Mr Porky is her boyfriend). Route stored in `State.player`.

**Already done:** Tokyo, Paris, Morocco, Scotland — use these as syntax reference, do not touch.
**Do NOT touch:** NYC, GBR — special pre-proposal puzzle scenes with their own bespoke structure. Leave entirely as-is.
**Finale scenes** (`finale_pebble`, `finale_oath`, `finale_cert`, `finale_passport`, `ending_*`) — already dual-perspective via `P()` and `State.player` checks. Leave as-is.
**Your job:** Add Squid perspective to Hawaii, Amazon, Iceland, Venice, India, Everest — in that order.

**Everest is different:** It is an expandable location like Hawaii/Amazon/etc. (not a puzzle-only stub like NYC/GBR). It has a basecamp hike, yak, monastery, photo, sherpa encounters, prayer flags, r2 scenes — treat it identically to the other locations.

---

## Helpers (all already in file)
```js
P('porky text', 'squid text')     // core tool — use everywhere text differs
statFlavor({ adv, cur, soc, cau, def })
addStat(name) / statUnlocked(name, threshold)
```

---

## Character voices

**Porky:** Deeply romantic and openly so — lives for Izzy's happiness, notices everything about her, creates intimacy in small moments. Things happen TO him; endearingly clumsy. Dry self-deprecating humour with a streak of quick wit and loving teasing toward Izzy — he gives as good as he gets, warmly.

**Squid:** Initiates everything — Mr Porky reacts. Loud-affectionate, refers to him by name often. Chaotic but competent. Competitive. Her "cautious" choices protect Mr Porky, not herself. Success feels obvious; failure is funny not embarrassing. Notices small things about him with fond exasperation.

---

## Syntax — three patterns, pick by scene type

**Pattern A — no statFlavor:**
```js
text: () => { addCoins(N); return P(
  `Porky outcome. +N Fish Coins.`,
  `Squid outcome. +N Fish Coins.`
); },
```

**Pattern B — has statFlavor, P() wraps each key's string:**
```js
text: () => { addCoins(N); return statFlavor({
  adv: P(`porky adv`, `squid adv`),
  def: P(`porky def`, `squid def`),
}); },
```

**Pattern C — arrival/r2 with base text + statFlavor appended:**
```js
text: () => P(
  `Porky arrival`,
  `Squid arrival`
) + statFlavor({
  adv: P(` porky flavour`, ` squid flavour`),
  def: P(` porky default`, ` squid default`),
}),
```

**Choice labels — text: field only, never next:/effect::**
```js
{ text: P('🏄 Porky label', '🏄 Squid label'), next: 'scene_key', effect: () => addStat('x') },
```

**Hard rules:**
- Never change `next:`, `effect:`, `addCoins()`, `addStat()`, `statFlavor()` structure, coin values, or `State.flags`
- `partner()` / `me()` inside rewritten strings → replace with inline `P('Izzy','Mr Porky')` / `P('Mr Porky','Squid')` so sentences read naturally per voice
- After each location: grep the location key to confirm all scenes converted before moving on

---

## Squid voice beats per scene

### HAWAII
| Scene | Squid beat |
|---|---|
| `hawaii` arrival | She's claimed a hammock and is pointing at waves before he registers the scenery |
| `hawaii_surf_hub` | Already on her board watching him. "You've got this." Not fully confident of this. |
| `hawaii_surf_trick` success | She screams. Already planning how to tell this story. |
| `hawaii_surf_trick` fail | "I filmed it," she says helpfully. |
| `hawaii_surf_safe` | She'd been holding her breath. Pretends she wasn't. |
| `hawaii_surf_pose` | Photo is perfect. She negotiates with the surf magazine on his behalf. |
| `hawaii_surf_study` | She was going to push him on the next wave. She doesn't mention this. |
| `hawaii_snorkel` | Tries to race the turtle. Twice. Turtle wins both times. |
| `hawaii_volcano` | She goes quiet first — he recognises genuine awe. She takes his flipper. |
| `hawaii_lei` | He weaves it with tongue out in concentration. She cries immediately then says don't make a big deal. |
| `hawaii_nap` | Asleep in 90 seconds. He watches her sleep feeling fortunate. Then falls asleep. |
| `hawaii_r2` arrival | She's identified four things before dark. He's ready for perhaps one. |
| `hawaii_cliff` bold | She goes first to demonstrate it's fine. He jumps after a dignified pause. |
| `hawaii_cliff` normal | Same — she goes first, he follows. |
| `hawaii_luau` | Up front immediately. Dancers invite her to try hula. He films it, keeps it just for them. |
| `hawaii_bonfire` | Six friends before he finishes sitting down. He finds this mortifying and wonderful in equal measure. |
| `hawaii_sunset` | She goes quiet. He doesn't say anything either. |
| `hawaii_singalong` | She leads it. Doesn't know all the words. Fills gaps with confidence. Ukulele player follows her. |

### AMAZON
| Scene | Squid beat |
|---|---|
| `amazon` arrival | Toucan watches Squid closely. She watches it back. Already moving. He's two paces behind. |
| `amazon_butterfly` | "I told you." She didn't tell him anything. Kisses him on the beak. |
| `amazon_river` | Leans over the side. He grabs her tail feathers. Dolphins wave anyway. |
| `amazon_snake` | Named Carl before he introduced himself. "I called it." |
| `amazon_document` | Names everything non-scientifically — Big Orange One, Stripey Nervous One. He writes them all down. |
| `amazon_plant` | Finds it first, grabs his flipper without a word. She decides this counts. |
| `amazon_r2` arrival | Pointing at things for 40 minutes. He's identified most of them. One was a leaf. Both having the best time. |
| `amazon_path` | Finds ruins first, starts theorising. He photographs while she talks. Theories: wrong, extremely compelling. |
| `amazon_guide` | Asks every question. He quietly takes better notes. Guide asks to keep a copy. |
| `amazon_meal` | Helps cook (chaotic). A toddler attaches to him. He is quietly thrilled. |
| `amazon_station` | He talks to researchers an hour. She falls asleep in a hammock. Wakes up, asks three excellent follow-up questions. |
| `amazon_swim` | In before he finishes reading the sign. "Porky. Get in. NOW." |

### ICELAND
| Scene | Squid beat |
|---|---|
| `iceland` arrival | She stops moving — which never happens. He watches her watching the sky. |
| `iceland_quiet` | Quiet for a full hour. He checks twice she's alright. She is. Just holding his flipper. |
| `iceland_beach` | Lies on the black sand and vanishes. Sits up laughing. "I'm camouflage." |
| `iceland_hotspring` | Befriends everyone in 12 minutes. He listens to her warmly embellish the Amazon story. |
| `iceland_glacier` | Goes quiet — third time this trip, he's keeping count. He holds her flipper. That's enough. |
| `iceland_r2` arrival | Finds the midnight sun personally hilarious. "The sun gave up, Porky." |
| `iceland_boat` | Skipper offers her the wheel immediately — she has that energy. He's beside her, terrified and deeply happy. |
| `iceland_saga` | Decides they're basically the saga heroes. He reads it. He kind of agrees. |
| `iceland_bonfire2` | Everyone's story within the hour. He carefully collects three email addresses in his notebook. She watches this with enormous fondness. |
| `iceland_window` | Falls asleep watching the aurora. He watches both the sky and her. Doesn't wake her. |
| `iceland_folk` | Plays the unknown instrument with complete confidence. He learns the rhythm by watching her. They lead the room together. "How." "No idea." |

### VENICE
| Scene | Squid beat |
|---|---|
| `venice` arrival | Already negotiating the gondola price before he's taken in the scenery. Gets a discount. She always does. |
| `venice_gondola` | Sings along off-key with the gondolier. He sits between them mortified and unable to stop smiling. |
| `venice_pizza` | Stares the pigeon down for 45 seconds without blinking. It leaves. He is impressed. |
| `venice_masque` | Finds a mask immediately, pulls him in. He dances until 1am then watches her dance until 4am. |
| `venice_glass` | Her piece is "expressive." His is a reasonable small bowl. He gives it to her. She names it Porky Jr. |
| `venice_serenade` | Makes HIM serenade her from the bridge above the canal. He sings badly. She cannot decide if mortified or happiest ever. Both. |
| `venice_r2` arrival | Already pointing at the water. "One more thing. Just one more." Fourth time today. He always says yes. |
| `venice_island` | Rows them over herself. Asks the monk 17 questions over pears. |
| `venice_library` | Finds the 1487 maybe-penguin manuscript. He spends 20 minutes confirming it. She sends it to everyone. |
| `venice_aperitivo` | Friends with the Venetian couple in minutes. He learns their story by listening quietly. Better at this than she realises. |
| `venice_bridge` | Cat appears. She looks at him like this proves something. Second cat arrives. "I'm a cat person." |
| `venice_midnight` | Leans back into him. "We're coming back." Not a question. He says "obviously." |

### INDIA
| Scene | Squid beat |
|---|---|
| `india` arrival | Nineteen things she wants to do TODAY. They compromise on seventeen, at speed. |
| `india_taj` | Goes completely silent — he's learned this means genuine awe. He watches her face. This was worth it. |
| `india_curry` | Takes level 4 with composure. Looks at him expectantly. He makes level 3. Declares team win. |
| `india_rickshaw` | Co-pilots. Twelve creative turns at speed. He holds on with both flippers having the time of his life, won't say it. |
| `india_henna` | Requests tiny penguins on both flippers specifically. He looks at his flipper for a long time. "I know," says Squid. |
| `india_music` | Sitar player invites her on stage. Claps the rhythm incorrectly but with complete confidence. He watches smiling the whole time. |
| `india_r2` arrival | Has a list. Had it since the plane. "We can do at least two." He picks one. She picks the other three anyway. |
| `india_bazaar` | Negotiates 40 minutes on a brass elephant. He gets three scarves and a friendship with the vendor's nephew. Both did well. |
| `india_history` | Asks seventeen questions. He takes notes for both. Historian asks to keep a copy. |
| `india_wedding` | Given a lamp to carry within 30 seconds. Carries it with great solemnity. He walks beside her matching her energy. |
| `india_rooftop` | Orders for both without looking at the menu. Correct about everything. He tells her. She already knew. |
| `india_stepwell` | Descends to the bottom, goes quiet — fourth time this trip. He sits beside her. Stone, silence, each other. |

### EVEREST
| Scene | Squid beat |
|---|---|
| `everest` arrival | She steps off and looks up. And up. And up. Goes quiet. "Porky. It's big." Already walking toward it. |
| `everest_basecamp` hub arrival | She's already two steps ahead. He's doing up his coat. |
| `everest_ridge` | Reaches the top first. Waits with flippers on hips. Cheers when he crests. He acts embarrassed. He isn't. |
| `everest_flora` | She names the altitude flowers after people she knows. He writes "Gerald" in his notebook without question. |
| `everest_sherpa` | Has Dawa's life story in twenty minutes. He writes it all down. Dawa is moved someone recorded it. |
| `everest_careful` | Slows to his pace immediately without being asked, says nothing about it. He notices. Says nothing either. They walk. |
| `everest_monastery` | Tea offered. She accepts immediately, sits cross-legged. He sits beside her. Mountains are the walls. Nothing needs saying. |
| `everest_yak` | Approaches with complete confidence. It lets her scratch its horns. He approaches more carefully. Yak headbutts him into snowdrift. "It likes you." |
| `everest_photo` | She photographs him with Everest behind. Then reverse. Then both via careful rock placement. Best photo of the trip. |
| `everest_predawn` | Sets the 4am alarm with zero hesitation. He doesn't complain once. They watch Everest catch first light before the world below knows it's morning. |
| `everest_r2` arrival | She's still looking at the mountains. "One more thing." He was already putting his coat on. |
| `everest_tea` | Butter tea. She tries it without making a face (effort required). He genuinely likes it — surprises everyone. Dawa laughs. |
| `everest_flags` | She writes on her flag and ties it on before he can read it. He writes on his before she can read his. Both decide not to ask. |
| `everest_clouds` | She names the clouds. He times them. "Gerald 3," she says. He writes "Gerald 3" in his notebook. |
| `everest_avalanche` | She grabs his flipper at the rumble. Guide confirms it's far. She immediately pretends she didn't grab it. He pretends not to notice she did. |
| `everest_valley` | She finds the path by asking three locals the same question. He helps wash up after the food. Both right. |

---

## Process per location
1. Read the existing single-voice block for the location
2. For each scene: identify pattern (A/B/C), write Squid string from the beat above, str_replace
3. After all scenes in a location: `grep -n "location_key" index.html` to confirm nothing missed
4. Move to next location

~80 scenes total. Work through them systematically, one location at a time.
