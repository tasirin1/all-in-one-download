# Self-host Cobalt (Instagram / X / Reddit / lainnya)

Semua platform non-YouTube di situs ini (Instagram, X/Twitter, Reddit, dll.)
diekstrak lewat **Cobalt API**. Instance Cobalt publik hampir semuanya
mati/diblokir oleh Instagram (CLOUDFLARE `Client Challenge`, login wajib),
jadi cara paling andal adalah **self-host satu instance sendiri** lalu tempel
URL-nya di **Advanced → Custom Cobalt API instance**.

Tidak perlu daftar / tidak perlu akun Instagram — Cobalt mengekstrak konten
publik secara anonim dari sisi server, lalu men-tunnel byte-nya ke browser kamu.

---

## Opsi A — Render (gratis, tanpa kartu) ✅ rekomendasi

### Cara termudah — tombol deploy 1 klik

Repo ini sudah punya **Render Blueprint** (`render.yaml`) yang mendefinisikan
Web Service image `ghcr.io/imputnet/cobalt:11` lengkap dengan env-nya. Cukup:

1. Klik **Deploy to Render**:
   [![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/tasirin1/all-in-one-download)
2. Login/signup Render (gratis, tanpa kartu) → pilih service **cobalt** → isi
   nama yang sama (`cobalt`) → **Create Web Service**.
3. Tunggu status **Live** → tempel `https://cobalt.onrender.com` di situs
   (Advanced → Custom Cobalt API instance) → Kirim.

> Kalau nama `cobalt` sudah terpakai, Render menambah suffix acak — sesuaikan
> field `API_URL` di dashboard (Settings → Environment) dengan URL asli service
> kamu (wajib slash di akhir), lalu Redeploy.

Apabila ingin form manual (tanpa blueprint), ikuti langkah di bawah.


Render bisa menjalankan image Cobalt resmi langsung; tier gratisnya tidak
perlu kartu kredit.

### Langkah deploy (dashboard Render)

1. Buka [dashboard.render.com](https://dashboard.render.com) → **New → Web Service**.
2. Pilih **"Deploy an existing image"** lalu isi:
   `ghcr.io/imputnet/cobalt:11`
3. Pengaturan:
   - **Instance type:** Free
   - **Region:** Frankfurt
4. **Environment variables** — tambahkan:
   - `API_PORT` = `10000`  ← samakan dengan port yang diharapkan Render
   - `API_URL` = `https://<nama-service>.onrender.com/`  ← URL yang diberikan Render
     (isi setelah deploy pertama, lalu redeploy; **wajib ada slash di akhir**)
5. Klik **Create Web Service**. Tunggu sampai *Live*, lalu cek:
   ```bash
   curl https://<nama-service>.onrender.com/
   ```
   Seharusnya mengembalikan JSON Cobalt.

### Jaga tetap hidup (penting)

Service Render gratis **tidur setelah 15 menit idle** dan butuh 30–60 detik
untuk bangun — lebih lama dari timeout request situs. Jadi jaga tetap hangat
dengan pinger gratis (mis. [UptimeRobot](https://uptimerobot.com) atau
[cron-job.org](https://cron-job.org)) yang memanggil
`https://<nama-service>.onrender.com/` setiap ~14 menit.

---

## Opsi B — Docker / VPS

```bash
docker run -d --name cobalt -p 9000:9000 \
  -e API_URL=https://<host-kamu>/ \
  -e API_PORT=9000 \
  ghcr.io/imputnet/cobalt:11
```

Pastikan HTTPS (mis. via Caddy/Nginx) dan tempel `https://<host-kamu>` di Advanced.

---

## Pasang di situs

Tempel URL instance (tanpa slash di akhir) di **Advanced → Custom Cobalt API
instance**, lalu tekan tombol **Kirim**. Contoh: `https://nama-service.onrender.com`.

> Catatan: satu instance = satu URL tunnel. Kalau pakai lebih dari satu host,
> tunnel yang dibuat instance A tidak akan dikenali instance B (efeknya mirip
> "Preview unavailable"). Jadi cukup satu instance saja.
