# KINGDOM CHAOS — Game Design & Brainstorming v0.1

**Tanggal konsep:** 12 November 2026  
**Repository:** `rizqunbw/gametiktok`  
**Status:** spesifikasi ide dan angka playtest, **belum implementasi atau angka hasil penelitian**.

## 1. Tujuan produk
Bangun game 3D ringan untuk TikTok LIVE (PC lokal + TikTok LIVE Studio/OBS) yang dapat dipahami viewer dalam 3 detik, mengundang partisipasi tanpa pembayaran, serta memberi respons visual kuat pada gift kecil maupun besar. **Keberhasilan diukur dari engagement dan retention, bukan dari asumsi bahwa gift pasti banyak.**

**Prinsip:** siapa pun bisa bergabung dan berkontribusi. Gift menambah *spectacle* dan pilihan taktik, bukan kemenangan otomatis. Tiap pemain punya karakter/username, setiap aksi relevan muncul secara visual.

## 2. Brainstorming: kandidat mekanik
| Ide | Daya tarik | Kekurangan / solusi | Keputusan |
|---|---|---|---|
| Red vs Blue castle siege | Rivalitas, jelas dari layar kecil | Snowball tim besar → penyeimbang target & comeback | **Inti game** |
| Monster evolution per viewer | Keterikatan dan comeback harian | Ratusan model berbeda berat → 4 kelas model bersama | **Inti progres** |
| Gift summon dragon | Efek sinematik menonjol | Terlalu dominan → multi-hit, damage cap, telegraph | **Gift besar** |
| Gift meteor langsung hancurkan kastil | Viral sesaat | Ronde berhenti mendadak, unfair → **tolak** | Tidak dipakai |
| Tap membesarkan setiap prajurit | Viewer merasa berdaya | Event tap tidak selalu tersedia per klik, raw spam → konversi like kumulatif ke energi | **Tap/like = energi** |
| Arena FFA 500 monster | Username pribadi dan ranking | Sulit dibaca di smartphone → jadikan mode tambahan | Pasca-MVP |
| Tim yang kalah dapat comeback gratis | Mengurangi viewer churn | Bisa dieksploitasi → objective terbatas & transparan | **MVP** |

### Hook yang harus terlihat tanpa suara
Di atas: `RED 55% | 02:31 | BLUE 45%`. Di tengah: pasukan berperang, giant/dragon terlihat. Di bawah: `FOLLOW TO JOIN • COMMENT RED/BLUE • TAP FOR ENERGY`. Notifikasi gift tampil maksimal beberapa detik dan tidak menutup arena.

## 3. Core gameplay
- **2 tim:** Red Kingdom dan Blue Kingdom; target menghancurkan kastil musuh atau HP terbanyak pada akhir waktu.
- **Viewer karakter:** join lewat follow (jika event tersedia) atau komentar `JOIN` sebagai fallback. Komentar `RED`/`BLUE` memilih tim. Nama akun tampil di atas unit yang disorot (bukan semua nama sekaligus).
- **Pasukan dasar:** 1 karakter per viewer aktif. Unit tambahan merupakan reinforcement NPC yang terkait nama donor; jangan membuat satu model unik per Rose.
- **Auto-battle:** unit mencari musuh terdekat, bergerak, serang berulang dengan wind-up, impact, hit reaction, death/respawn. Tidak ada kill sekali pukul.
- **Evolusi:** Soldier → Knight → Champion → Titan melalui XP dari pertempuran dan kontribusi; skill berdampak moderat, model/animasi makin spektakuler.
- **Persistent progression:** account ID TikTok sebagai identifier bila tersedia, XP kosmetik/meta-level disimpan; kekuatan ronde di-reset atau dinormalisasi agar pemula tetap kompetitif.
- **Ronde otomatis:** 4 menit; skor/hp reset antarronde; streak & MVP dipertahankan di riwayat.

### Timeline ronde 240 detik
| Waktu | Fase | Pemicunya |
|---|---|---|
| 00:00–00:30 | Join window | Perkenalan match-up, pemilihan tim, first reinforcement |
| 00:30–01:00 | Opening clash | Kedua wave berjalan dan bentrok |
| 01:00–03:00 | Total War | XP, summon gifts, objective netral, perubahan momentum |
| 03:00–03:45 | Final Chaos | Kedua tim mendapat objective ekstra; VFX meningkat tetapi tetap ringan |
| 03:45–04:00 | Last Stand | Tidak ada instakill; buff comeback terbatas, countdown |
| Setelahnya 12–18 dtk | Result screen | Winner, Top Kills, Top Support, Top MVP, Top Reinforcer; reset |

**Win condition:** kastil mencapai 0 HP atau HP tertinggi saat timer 0; tie-breaker berdasar objective netral lalu seri jika tetap sama. Untuk mencegah gift menit terakhir menghasilkan keputusan instan, semua ability tetap tunduk pada damage cap dan cast time.

## 4. Interaction design — follow, tap, likes, love
**Catatan terminologi:** 'tap tap layar' umumnya menghasilkan like/heart LIVE. Heart dari like **bukan** Hand Heart gift 100 coins. Jangan menganggap ada API event 'tap per tap'; gunakan hitungan like yang terkonfirmasi, yang dapat datang dalam batch, dan hindari double-count total vs increment.

| Aksi | Hadiah gameplay | Visual/audio | Anti-abuse & fallback |
|---|---|---|---|
| **Follow** | Unlock spawn awal 1 Soldier + bonus 10 War Energy sekali per viewer | Entry portal, username banner `WELCOME` | Dedup by viewer ID; jika follow event tidak tersedia, `JOIN` untuk free entry, jangan klaim follow sudah diverifikasi |
| **Tap/like / love (heart gratis)** | Setiap **25 like baru terhitung = +1 War Energy** pada tim viewer | Heart particles kecil; meter team charging | Rate cap contribution 20 energy/user/menit; threshold per viewer, cap global; like dari user belum pilih tim masuk pending max 60s |
| **War Energy** | Setiap **20 energy tim** = heal 1% castle HP (maks 5% castle HP per 30 detik), atau memberi 5s attack-speed +3% ke 10 unit depan (rotasi) | Meter 0–20, pulse heal/attack | Team heal cannot exceed max HP; softcap contribution |
| **Comment `JOIN`** | Masuk gratis bila fitur follow detection tidak tersedia | Spawn kecil + username | 1 per user/round, cooldown |
| **Comment `RED` / `BLUE`** | Pilih kerajaan sebelum mulai / 20 detik awal | Team flag on character | Lock hingga ronde selesai, auto-balance opsional untuk join tanpa pilihan |

**Contoh:** 250 likes baru yang valid dari viewer A = 10 energy sebelum cap. Sumbangan 250 likes tidak spawn 250 unit. Energy bersifat kontribusi tim dan tidak bisa ditukar uang.

## 5. Gift effects — empat gift, empat fungsi yang jelas

**Tier koin bersumber dari brief, bukan validasi harga TikTok saat ini.** Map gift berdasarkan gift ID dari event payload, bukan hanya nama teks. Jumlah `repeatCount` / streak mesti dinormalisasi agar tidak double process di akhir combo. Semua perhitungan server authoritative; jangan percaya label di frontend.

| Gift | Tier sesuai brief | Nama skill | Efek spesifik | VFX + TTS | Guardrail |
|---|---:|---|---|---|---|
| **Rose** | **1 coin** | **Rose Squad** | +1 reinforcement NPC kecil (HP 60% soldier), hidup maks 20 detik; +1 support point | Petal burst, mini portal, floating `+1` | 15 reinforcement aktif per team dari Rose; sisanya antre/batch sebagai `reserve`; user cap visual 1 summon animation/2s |
| **Doughnut** | **30 coins** | **Sugar Supply Drop** | Heal unit sekutu aktif dalam radius area 8% max HP masing-masing, speed +5% selama 5 detik | Donut supply crate turun, ring heal emas | Maks total 15% heal per unit per 10s dari Doughnut; stacking duration up to 10s, not multiplicative |
| **Hand Heart** | **100 coins** | **Heart Champion** | Summon 1 knight champion selama 25 detik, damage ~3× soldier, HP ~4× soldier | Heart portal, camera focus 1.5s, sebut username | Maks 2 champions gift per team aktif; lebihnya queued, **tidak** hangus |
| **Money Gun** | **500 coins** | **Dragon Barrage** | Naga mengudara 2s, lalu **3 fireball berurutan** selama 12s ke zona castle/army; total direct castle damage cap **12% max castle HP**, area damage unit capped | Cinematic 2.5s, siren + username, kamera kembali ke war | 1 dragon global aktif; berikutnya masuk queue; castle direct gift damage cap 18% max HP/30s per tim; no instant win |

**Penting untuk fairness:** Pembatasan cast / queue diumumkan jelas di UI. Jika antrean gift besar panjang, render efek tanpa menambah damage di luar cap atau batasi panjang antrean secara transparan; **jangan menjanjikan efek instan lalu diam-diam membuangnya**. Counterplay gratis tersedia: tim lawan bisa mengisi shield meter dengan like dan menyelesaikan objective netral.

### Bagaimana masing-masing gift memicu excitement?
- **Rose = micro reward:** setiap Rose menambah 'Rose Army' terlihat, tapi digabung secara render dan antrean agar tidak lag. Milestone 25 Rose → special marching animation **tanpa bonus damage tak terbatas**.
- **Doughnut = clutch save:** supply turun tepat saat pasukan hampir mati, memberi alasan support ikut masuk.
- **Hand Heart = personal spotlight:** champion memakai tag username, punya skill slash multi-hit dan kill log.
- **Money Gun = spectacle:** naga terbang melintas layar, menembak tiga kali secara bertahap (bukan langsung win), announcer menyebut username, leaderboard 'Dragon Caller'.

### Simulasi satu peristiwa, bukan angka revenue
1. `@A` follow → muncul Soldier Red.
2. `@B` kirim 300 likes terkonfirmasi → +12 War Energy Blue (di bawah 20/min cap).
3. `@C` kirim 10 Rose → 10 reinforcement ke antrean red, beberapa muncul bertahap.
4. `@D` kirim Doughnut → supply crate menjatuhkan heal unit Blue.
5. `@E` kirim Hand Heart → knight besar masuk arena dan menyerang berkali-kali.
6. `@F` kirim Money Gun → naga warnai langit dan menyerang 3 kali, tim lawan masih punya counterplay.

## 6. Evolving character & fairness
| Progress | Visual | Gameplay |
|---|---|---|
| Lv 1 Soldier | Sword + basic shield | Standard melee |
| Lv 5 Knight | Armor + banner | +5% HP dalam ronde |
| Lv 10 Champion | Glow weapon, combo | +8% damage dalam ronde |
| Lv 20 Titan | Scale 1.4×, shockwave | +10% HP, area skill cooldown 20s |

Level hanya aktif sepanjang ronde, lalu kembali ke baseline. Di luar ronde simpan cosmetic XP, badges dan historical leaderboard supaya pemain lama punya identitas, **bukan keunggulan kekuatan permanen yang merusak onboarding**. Progress gratis diperoleh dari waktu bertahan, kills, like energy, dan objective. Gift support point dipisahkan dari skill.

## 7. Comeback, pacing, dan counterplay
- **Neutral crystals:** muncul tiap 50 detik di tengah; siapapun bisa merebutnya lewat unit tanpa gift, memberi shield 10 detik.
- **Trailing team aid:** jika HP castle lebih rendah 20+ poin persentase selama 30s, munculkan side-path reinforcements netral yang dapat direbut melalui engagement gratis. Maks 1 kali per ronde per tim.
- **Anti-boredom:** setiap 20–30 detik munculkan objective/hit combo/team energy milestone; sesi gift tidak menjadi satu-satunya sumber peristiwa.
- **Prevent dominating whales:** damage cap per 30s, global casting queue, ability telegraph, no instant victory; leaderboard PvP terpisah dari top gifters.
- **No gambling:** hasil skill deterministik, tidak ada paid lootbox, slot machine, random cash prize, atau kompensasi uang.

## 8. UI portrait 1080×1920
```text
┌─────────────────────────────────┐
│ KINGDOM CHAOS  •  ROUND 12      │
│ RED CASTLE  72%  ⏳ 02:39  BLUE 64%│
│ █████████   VS    ████████       │
│                                 │
│        [Battlefield 3D]         │
│    ~300–500 batched low-poly    │
│         soldiers & giants       │
│          DRAGON ATTACK!         │
│       @username summoned       │
│                                 │
│     [War Energy ♥ 16/20]        │
│                                 │
│ FOLLOW / JOIN TO PLAY           │
│ COMMENT RED or BLUE             │
│ TAP ♥ TO CHARGE TEAM ENERGY     │
│ 🌹 ROSE   🍩 DOUGHNUT           │
│ 🫶 HAND HEART  💸 MONEY GUN     │
│ TOP KILLS | TOP SUPPORT | MVP   │
└─────────────────────────────────┘
```
Keep headline large, large battle area, safe margins for TikTok app chrome, effects don't cover critical gameplay. Center focus stays on active fight; camera zoom reserved for gift moments <= 2.5s.

## 9. Visual & sound direction
- **Style:** 3D stylized/low-poly with memorable silhouette and material differences RED/BLUE. Avoid loading dozens of high-poly models.
- **Animation:** actual rigged skeletal animation: idle, run, multi-hit attack combo, hit reaction, knockback, die, respawn, summon, cast, dragon flight & fireballs. Blend animation and interpolation; not just static sprites sliding.
- **Rendering:** Babylon.js or Three.js; prototype Three.js with InstancedMesh / batched skinned actors strategy; max 300–500 on screen, NPCs beyond cap simulated numerically.
- **Sounds:** layered sword clash, footstep, unit vocal, healing pop, chant, dragon roar; compressor + ducking.
- **Announcer:** template TTS for join, champion, dragon and MVP; prioritize biggest event, one voice at a time; batch small gifts, safe displayname sanitization and pronunciation fallback, throttling.

## 10. Technical architecture
```text
TikTok LIVE event source (authorized connector)
        │ (follow/like/comment/gift payloads; availability varies)
        ▼
Node.js Event Ingestion & Validation
  - normalize IDs / counts / streak end
  - dedup, ordering, rate limits
  - no credentials in frontend
        ▼
Game Simulation (authoritative state, fixed tick)
  - player registry + teams
  - War Energy + gift ability queue
  - collision/combat, HP, buffs, phase timers
        │
        ├── SQLite (progress, logs, leaderboard)
        └── WebSocket state updates
                  ▼
Web client (Three.js + HUD + TTS/VFX)
                  ▼
OBS Browser Source / TikTok LIVE Studio capture
```
Default: run all on local Windows PC, browser source at 1080×1920; can later host spectator frontend but don't rely on Vercel serverless for durable always-on WebSocket game simulation.

### Essential event model
```json
{
  "eventId": "deduplicated-source-event-id",
  "eventType": "follow|like|comment|gift",
  "viewerId": "stable-account-id-if-provided",
  "displayName": "sanitized username",
  "giftId": "platform-gift-id-if-any",
  "giftCount": 1,
  "likeDelta": 25,
  "timestamp": 0
}
```
Dedup key can be composite if event ID absent. Implement per-gift ID mapping in config, and correctly handle combo/streak accumulated counts. Event availability and count semantics must be verified with captured, consented test events, not presumed.

### Security and reliability
Local operator dashboard with **SIMULATE** buttons for every action. Store connection tokens/keys in server environment only. Ingest queue must not block rendering, use backpressure and metrics. If TikTok feed disconnects, game continues in demo/offline mode and shows operator-only status; no fake events attributed to real viewers.

## 11. Balancing spreadsheet (initial hypotheses, test before LIVE)
| Parameter | Starting value | Why |
|---|---:|---|
| Match length | 240s | Enough room for a story and comeback |
| Like conversion | 25 likes = 1 Energy | Avoid per-tap flood |
| Like cap | 20 Energy/user/min | Resist spam dominance |
| Team Energy threshold | 20 | One readable team meter |
| Rose summoned NPC lifetime | 20s | Visible effect, controlled crowd |
| Rose NPC per-team active cap | 15 | Prevent screen explosion |
| Doughnut heal | 8% unit max HP | Useful but not a full reset |
| Hand Heart champion | 25s | Personal highlight |
| Money Gun Dragon shots | 3 over 12s | Spectacle stretched over time |
| Money Gun castle damage cap | 12% max HP | No one-shot |
| Gift total castle damage per team | 18% max HP/30s | Counterplay |

**Potential contradiction to test:** full 12% dragon damage may hit 18%-per-30s team cap; if cap is reached, show reduced impact/visual explicitly. Clarify that Rose reinforcement count + team active cap means pending gift effects may be delayed. Avoid any implication that gift purchase guarantees a specific win outcome.

## 12. Operator controls and analytics
- Simulate follow, 25/100/500 likes, Rose x1/x10/x100, Doughnut, Hand Heart, Money Gun.
- Force phase for QA, pause, stop, restart match.
- Show event counts by type, dropped/duplicate events, gift activation latency, queued gifts, FPS, RAM/GPU load.
- Log daily **unique active participants**, join→participate conversion, follow→join rate (only where measurable), likes per participant, gift conversion rate, match completion, return viewers, support distribution, and p95 event-to-animation latency.
- Playtest variants: 2m vs 4m rounds; 25 likes vs 50 per Energy; Dragon total cap 8% vs 12%; one vs two champions active. Never optimize only for spending; monitor viewer drop-off and fairness complaints.

## 13. Roadmap
### Phase 0 — Prototype without TikTok
- [ ] Repository structure `server/`, `client/`, `shared/`, `assets/`
- [ ] 1080×1920 canvas + responsive HUD
- [ ] 2 castles + auto-battle + combat animation
- [ ] 300–500 light actors at stable FPS
- [ ] Operator simulator for 6 trigger types
- [ ] Round timer, winner, leaderboard

### Phase 1 — Gift & identity
- [ ] Follow/join, side selection, like-to-energy
- [ ] Rose reinforcement NPC + batching
- [ ] Doughnut supply drop/heal
- [ ] Hand Heart champion + username highlight
- [ ] Money Gun dragon 3-shot sequence
- [ ] Animation/sound/TTS announcement queue
- [ ] Account identity + cosmetic progression

### Phase 2 — LIVE integration and safeguards
- [ ] Verify TikTok live event source and permissions
- [ ] Confirm correct gift IDs, coin tiers and combo behavior in target region
- [ ] Duplicate/ordering/streak tests
- [ ] Integrate as OBS/TikTok LIVE Studio capture source
- [ ] Disconnect fallback, moderation and performance QA

### Phase 3 — Engagement iteration
- [ ] Offline playtests with simulated 10 / 100 / 500 users
- [ ] A/B test energy thresholds and round duration
- [ ] Retention metrics, team fairness, device load
- [ ] Additional game mode only after core metrics look promising

## 14. Open design decisions (next brainstorming)
1. Team selection: manual comment RED/BLUE vs auto-balance.
2. Role personality: soldiers as cats, humans, fantasy monsters, or robot armies.
3. Final round: first to 0 castle HP or score at end (can remove early ending for pacing).
4. Whether gift ability should be fully deterministic and visibly queued during burst traffic.
5. Whether persistent cosmetics are free unlocks based on play, plus a donor badge that **never** affects combat stats.
6. Confirm the phrase **'love'** means likes/tap hearts, not a separate gift. Hand Heart is modeled separately.
7. LIVE event reliability in actual broadcast environment; do not claim follow verification until tested.

## 15. Definition of success for the first prototype
- Viewer understands **JOIN → CHOOSE TEAM → TAP → WATCH WAR** within 5s.
- Every trigger shows an obvious visible response within a practical latency target.
- Three dragon shots are clearly distinguishable and won't skip to one final explosion.
- 300–500 units render smoothly on target local Windows PC.
- A free participant can meaningfully impact a round.
- At least one gift can be tested under cooldown/queue without losing the event.
- A 4-minute loop completes automatically 20 consecutive times without state leak/crash.

---
**Reminder:** this document is a product hypothesis. TikTok LIVE APIs, gift pricing, permissions, and platform policies change. Verify availability and platform rules before production; never promise viewers payouts or guaranteed outcomes.