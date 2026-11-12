# KINGDOM CHAOS — TikTok LIVE Interactive Game

> **Status:** brainstorming / pre-production. Belum ada game yang diimplementasikan.
>
> **Konsep pilihan:** Kingdom War + Monster Evolution, sistem hybrid free engagement dan gifts, tampilan 3D stylized untuk TikTok LIVE portrait 1080×1920.

## Premis
Viewer masuk sebagai prajurit kecil, memilih **RED KINGDOM** atau **BLUE KINGDOM**, lalu bertempur secara otomatis. Pemain bisa berkembang menjadi knight, champion, dan titan. Follow, likes/tap, komentar, dan gifts memberi efek **langsung terlihat**, tetapi gift terbesar tidak otomatis memenangi ronde. Pertempuran berlangsung ~4 menit dan berulang otomatis dengan hasil, MVP, serta leaderboard.

## Daftar interaksi (proposal awal, untuk playtest)
| Trigger | Efek | Prinsip |
|---|---|---|
| Follow | Unlock/daftarkan karakter + banner masuk | Satu kali per akun; efek hanya setelah event follow valid terdeteksi |
| Komentar `RED` / `BLUE` | Pilih tim | Bisa memilih sebelum ronde atau di awal ronde; anti-switch abuse |
| Tap / like | Tambah **War Energy** tim, setiap 25 like terhitung = +1 energi | Berdasarkan counter like/event yang benar-benar tersedia, **bukan** mengasumsikan setiap tap menghasilkan event individual |
| Rose — 1 coin | **Mini Reinforcement**: +1 unit cadangan + efek bunga | Murah, dapat ditumpuk/diantrekan; tidak membanjiri render |
| Doughnut — 30 coins | **Supply Drop**: heal 8% HP unit aktif sekutu + 5 detik buff speed kecil | Efek support berguna saat perang |
| Hand Heart — 100 coins | **Champion Call**: summon 1 champion selama 25 detik | Hero unik, announcer + username |
| Money Gun — 500 coins | **Dragon Barrage**: naga menyerang 3 kali selama 12 detik, total damage dibatasi 12% max HP kastil lawan | Sinematik besar dan bisa dilawan; tidak instant-win |

*Coin di atas adalah tier yang diminta untuk desain; harga/nama gift aktual, event metadata, dan ketersediaannya perlu diverifikasi di akun/region LIVE sebelum implementasi.*

## Dokumen utama
**[GAME_DESIGN.md](./GAME_DESIGN.md)** memuat:
- Brainstorming dan keputusan konsep, termasuk alternatif yang ditolak
- Desain loop 4 menit dan progresi evolusi
- Detail follow, tap, likes, komentar, dan semua empat gift
- Angka balancing awal, cooldown, cap, stacking, counterplay, fairness
- UI 1080×1920, animasi, audio/announcer
- Event pipeline TikTok → game lokal → TikTok LIVE Studio/OBS
- Roadmap implementasi serta metrik dan eksperimen playtest

## Prioritas MVP
1. Simulasi offline untuk 300–500 unit 3D ringan, 2 kerajaan, kastil, pertempuran, ronde 4 menit.
2. Simulator event dengan tombol Follow / Likes / Rose / Doughnut / Hand Heart / Money Gun untuk testing tanpa gift asli.
3. Four gift VFX, usernames, leaderboard, announcer dengan antrean.
4. Bridge event TikTok LIVE setelah akses data sah dan stabil teruji.
5. Playtest balancing dan performance sebelum public LIVE.

**Batasan:** interaksi/gift adalah dukungan sukarela. Tidak ada janji hadiah uang, gambling, pembelian wajib, atau klaim gift menjamin kemenangan.