### 1.2 Add such a sentence (you need to modify the text) right after the spawner.spawn (...);
![Image when theres a print outside of the spawn method](/images/1_2.png)

Pesan "Cleo's Computer: outside spawn" muncul lebih dulu karena letaknya setelah pemanggilan spawner.spawn(...) tetapi sebelum executor.run(), sehingga langsung dijalankan oleh thread utama. Sementara itu, pesan "howdy!" dan "done!" merupakan bagian dari future yang baru akan dieksekusi saat executor dijalankan. Akibatnya, setelah mencetak "outside spawn", executor mulai memproses future—menampilkan "howdy!", menunggu 2 detik, lalu mencetak "done!". Urutan ini mencerminkan bahwa eksekusi future bersifat malas (lazy) dan membutuhkan executor untuk berjalan.

### 1.3 Multiple Spawn and Removing Drop
#### Multiple Spawns With Drop
![Multiple Spawns](/images/1_3_drop.png)

#### Multiple Spawns Without Drop
![Multiple Spawns No Drop](/images/1_3_nodrop.png)

Pada gambar pertama, saya menjalankan tiga spawn yang masing-masing diberi pembeda (tidak diberi nomor, lalu bernomor 2 dan 3). Hasilnya menunjukkan bahwa ketiga future mulai dieksekusi hampir bersamaan. Output yang muncul mengikuti urutan saat masing-masing task di-spawn, dimulai dari yang pertama, kemudian yang kedua, dan terakhir yang ketiga. Hal ini menunjukkan bahwa tidak seperti pada pemrograman sinkron—di mana satu tugas harus selesai sebelum tugas berikutnya dimulai—di sini semua task bisa berjalan secara paralel tanpa saling menunggu, sehingga penyelesaiannya hampir bersamaan.

Ketika saya tidak menyertakan drop(spawner), program tidak langsung berhenti meskipun semua task sudah selesai dijalankan. Ini karena executor masih menganggap mungkin akan ada task baru yang ditambahkan. Dengan memanggil drop(spawner), kita secara eksplisit memberi sinyal bahwa tidak akan ada task tambahan, sehingga executor dapat menyimpulkan bahwa seluruh pekerjaan telah selesai dan program bisa berhenti.