# Replay Catalog Manager — Everything JKT48

## Isi
- `index.html`: web statis yang ramah HP, bisa dipublikasikan ke GitHub Pages.
- `update_katalog.sh`: script Termux untuk mengunduh API dan memeriksa ID thumbnail baru dengan yt-dlp.
- CSV sebagai katalog yang bisa diekspor/impor tanpa token.

## Publikasikan ke GitHub Pages
1. Buat repositori baru, misalnya `replay-catalog-manager`.
2. Unggah `index.html` ke root repositori.
3. Buka **Settings → Pages**.
4. Pada **Build and deployment**, pilih **Deploy from a branch**, branch `main`, folder `/(root)`, lalu Save.
5. Buka URL yang muncul di halaman Settings → Pages.

Dokumentasi resmi: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

GitHub Pages bersifat publik. Jangan unggah CSV pribadi ke repositori publik.

## Siapkan Termux
```bash
pkg update
pkg install curl jq python
python -m pip install -U yt-dlp
termux-setup-storage
mkdir -p /sdcard/jkt/test
```
Simpan `update_katalog.sh` di `/sdcard/jkt/test/`, lalu:
```bash
cd /sdcard/jkt/test
chmod +x update_katalog.sh
./update_katalog.sh
```
Jika `pip` menolak pemasangan `yt-dlp`, coba `pkg install yt-dlp` jika tersedia di repositori Termux kamu.

## Alur pembaruan
1. Di web, impor katalog CSV sebelumnya bila sudah ada.
2. Jalankan `./update_katalog.sh` di Termux.
3. Di web, impor `replays.json`.
4. Impor `verification.txt` (hasil gabungan pemeriksaan metadata).
5. Tinjau status lalu ekspor katalog CSV.
6. Simpan CSV hasil ekspor sebagai `/sdcard/jkt/test/katalog-replay.csv` agar script bisa mendeteksi ID yang sudah ada pada pembaruan berikutnya.

Jika CSV belum tersedia di folder tersebut, semua ID API dianggap baru untuk verifikasi. Script tidak menghapus katalog CSV atau riwayat verifikasi lama.

## Catatan teknis
- Tombol “Ambil API terbaru” mencoba fetch langsung dari browser; bila CORS diblokir, gunakan Termux lalu impor JSON.
- Katalog tersimpan di `localStorage` browser dan dapat diekspor sebagai CSV/JSON.
- Metadata `unavailable` tidak memastikan apakah video dihapus, dibatasi, atau gagal diakses sementara.
- Tidak ada token GitHub di HTML. Script tidak mengunduh video dan tidak mengubah otorisasi atau status pembelian layanan.
