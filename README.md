Nama: Muhammad Nanda Pratama
NPM: 2206081654

Latihan: Creating Particles

## Proses Pengerjaan Tutorial

Pada tutorial ini, saya mempelajari dan mengimplementasikan sistem particles di Godot. Berikut adalah proses pengerjaan yang saya lakukan:

### 1. Implementasi Environment Particle (Hujan)

- Menambahkan node GPUParticles2D ke scene Level 1 sebagai root node
- Mengkonfigurasi ParticlesMaterial dengan parameter:
  - Amount: 500 (untuk mendapatkan jumlah partikel yang cukup untuk efek hujan)
  - Lifetime: 4 (agar partikel bertahan lebih lama di layar)
  - Scale max: 10 (agar partikel terlihat ada yang besar dari kamera)
  - Emission Shape: Box dengan extents x=2000 (agar hujan muncul di area luas)
  - Color: Biru (#84a6b6) untuk menciptakan efek warna biru hujan
  - Spread: 20 (untuk mengontrol arah persebaran partikel)
  - Gravity: x=-500, y=500 (memberikan arah diagonal ke kiri bawah untuk efek hujan)
  - Visibility Rect: w=2000, h=500 (memastikan partikel tetap terlihat saat kamera bergerak)
  - Trail: Enabled (untuk memberikan efek ekor pada partikel)


### 2. Implementasi Trail Particle (Efek Jejak Berjalan)

- Menambahkan node GPUParticles2D ke scene Player
- Mengkonfigurasi dengan texture dari kenney_platformerpack/PNG/Particles/brickGrey_small.png
- Parameter yang diatur:
  - Amount: 4 (jumlah partikel yang sesuai untuk trail)
  - Lifetime: 0.5 (durasi pendek agar trail tidak terlalu panjang)
  - Gravity: y=-200 (partikel bergerak ke atas)
  - Spread: 180 derajat (partikel menyebar ke segala arah)
  - Initial Velocity: 50 (kecepatan awal partikel)
  - Emission Shape: Box dengan nilai x=30 (partikel muncul dari area kaki player)
  - Local Coord: Off (agar partikel tidak terus mengikuti player)

- Mengubah script Player.gd untuk mengontrol kapan partikel harus aktif:
  ```gdscript
  @onready var particle = $GPUParticles2D
  
  func get_input():
      velocity.x = 0
      if is_on_floor() and Input.is_action_just_pressed('jump'):
          velocity.y = jump_speed
      if Input.is_action_pressed('right'):
          velocity.x += speed
          if is_on_floor():
              particle.set_emitting(true)
      elif Input.is_action_pressed('left'):
          velocity.x -= speed
          if is_on_floor():
              particle.set_emitting(true)
      else:
          particle.set_emitting(false)
  ```

Implementasi ini membuat partikel hanya muncul ketika player berada di lantai dan sedang bergerak, menciptakan efek jejak berjalan yang realistis.

Latihan: Game Balancing

## Proses Pengerjaan Game Balancing

Dalam tutorial ini, saya melakukan proses game balancing untuk memastikan level memiliki tingkat kesulitan yang tepat bagi pemain. Berikut adalah proses yang saya lakukan:

### 1. Mengidentifikasi Masalah

- Saya menambahkan scene Spawner.tscn ke Level 1 pada dua lokasi:
  - Spawner pertama di platform awal (position x=734, y=90)
  - Spawner kedua di akhir platform tengah (position x=1731, y=352)
- Saya menjalankan game dan mengidentifikasi masalah: dengan spawn rate default, terlalu banyak musuh (tikus) yang di-spawn sehingga pemain tidak memiliki celah untuk melompati rintangan
- Masalah utama teridentifikasi pada Spawn Rate yang terlalu cepat, menyebabkan musuh muncul terlalu berdekatan tanpa memberikan celah bagi pemain

### 2. Melakukan Penyesuaian Awal

- Saya mengubah nilai Spawn Rate dari konfigurasi default menjadi 2 detik
- Setelah pengujian, saya menemukan bahwa dengan Spawn Rate 2 detik, permainan menjadi terlalu mudah
- Pemain dapat dengan mudah melewati rintangan tanpa tantangan yang berarti

### 3. Mencari Titik Keseimbangan

- Saya melakukan beberapa kali pengujian dengan nilai Spawn Rate yang berbeda-beda untuk kedua spawner
- Tujuannya adalah mencapai "flow state" dimana pemain merasa tertantang tetapi tidak frustrasi
- Setelah beberapa kali percobaan, saya menemukan bahwa nilai yang tepat adalah:
  - Spawner pertama: 1.5 detik (memberikan tantangan di platform awal)
  - Spawner kedua: 1.3 detik (memberikan tantangan yang sedikit lebih tinggi di platform akhir)
- Konfigurasi ini memberikan keseimbangan yang baik:
  - Cukup menantang karena pemain harus mengatur timing lompatan dengan tepat
  - Memberikan dinamika progresif dimana tingkat kesulitan meningkat saat mendekati akhir level
  - Tetapi masih memberikan celah yang cukup untuk pemain melewati rintangan jika mereka bereaksi dengan cepat

### 4. Hasil Akhir dan Evaluasi

- Dengan dua spawner dan spawn rate yang berbeda (1.5s dan 1.3s), level menjadi seimbang (balanced) sekaligus dinamis
- Pemain memiliki tantangan yang meningkat secara bertahap saat mendekati tujuan akhir
- Tingkat kesulitan berada pada level optimal yang memberikan kepuasan ketika berhasil melewati rintangan
- Penggunaan dua spawner dengan rate berbeda menciptakan variasi gameplay, menghindari kesan monoton

### Kesimpulan Game Balancing

Proses game balancing ini mengajarkan saya pentingnya menyesuaikan parameter gameplay untuk menciptakan pengalaman bermain yang optimal. Terlalu mudah akan membuat pemain bosan, sementara terlalu sulit akan membuat pemain frustrasi. Menemukan keseimbangan yang tepat adalah kunci untuk membuat game yang menarik dan memberikan kepuasan kepada pemain.

Dalam kasus ini, penyesuaian parameter Spawn Rate pada dua lokasi berbeda memberikan dampak yang signifikan pada tingkat kesulitan dan dinamika game. Saya juga belajar bahwa menciptakan progressi tantangan (dari 1.5s ke 1.3s) membuat gameplay lebih menarik daripada tingkat kesulitan yang konstan di seluruh level.