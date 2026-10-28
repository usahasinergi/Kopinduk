# Koperasi Induk

Website statis untuk Cloudflare Pages dengan GitHub sebagai repository dan Supabase sebagai database.

## Sudah disiapkan
- Landing page responsif: `index.html`
- Admin Panel: `admin.html`
- Database PostgreSQL: tabel `profiles`, `cooperatives`, `news`, `products`, `activities`
- Row Level Security untuk konten publik dan pengelolaan oleh admin/editor.

## Menyiapkan administrator
1. Buka Supabase Dashboard → Authentication → Users.
2. Tambahkan pengguna dengan email dan password yang Anda kuasai.
3. Buka SQL Editor dan jalankan query berikut. Ganti email contoh dengan email akun administrator Anda.

```sql
insert into public.profiles (id, full_name, role)
select id, 'Administrator', 'admin'
from auth.users
where lower(email) = lower('EMAIL_ADMIN_ANDA')
on conflict (id) do update set full_name = excluded.full_name, role = 'admin';
```

4. Buka `/admin.html` di website yang sudah di-deploy, lalu login dengan akun tadi.

## Deploy ke Cloudflare Pages
1. Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git.
2. Pilih repository `usahasinergi/Kopinduk`.
3. Framework preset: None; build command kosong; output directory `/` atau `.` sesuai pilihan UI.
4. Deploy. Push ke branch yang terhubung akan memicu deploy otomatis.

## Keamanan
- Publishable key Supabase di frontend memang dirancang untuk aplikasi browser. RLS tetap wajib diaktifkan.
- Jangan pernah menaruh service_role atau secret key di file frontend.
- Periksa role admin di tabel profiles dan uji dengan akun non-admin sebelum website digunakan secara luas.
- Gambar saat ini menggunakan URL; unggah gambar ke Supabase Storage atau layanan gambar pilihan Anda dan simpan URL-nya di Admin Panel.

## Kontak dan konten
Informasi alamat, nomor telepon, email, teks, dan gambar pada halaman utama masih berupa contoh dan harus disesuaikan dengan data resmi koperasi.
