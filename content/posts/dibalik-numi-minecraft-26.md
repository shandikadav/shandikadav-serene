+++
title = "Dibalik Pengembangan NUMI Minecraft 2026"
date = 2026-02-14
draft = false

[taxonomies]
categories = [ "2026" ]
tags = [ "minecraft", "numi", "behind-the-tech", "minecraft-rpg" ]
+++

{{ figure(src="/images/numi-minecraft-26/banner_event1.png", alt="Header Image", caption="NUMI Minecraft") }}

## Latar Belakang dan Perubahan Peran

NUMI Minecraft 2026 kembali hadir sebagai event tahunan NUMI Community yang diadakan setiap bulan Ramadhan. Tahun ini tetap dikoordinasikan oleh RyanRyzer dan dikembangkan oleh trio NUMI DEV: RyanRyzer, Autodie, dan gue sendiri.

Namun perlu gue jelaskan secara transparan, kontribusi teknis terbesar di 2026 ini justru berada di tangan Ryan dan Autodie. Gue tetap terlibat dalam diskusi konsep, arah sistem RPG, serta beberapa keputusan arsitektural, tetapi implementasi utama, integrasi ratusan mod, debugging dependency, hingga tuning performa intensif dikerjakan langsung oleh mereka berdua. Jadi kalau bicara soal kedalaman teknis server tahun ini, Ryan dan Autodie yang benar-benar jadi tulang punggung eksekusinya.

Perubahan terbesar tahun ini bukan sekadar penambahan fitur, melainkan perubahan pendekatan arsitektur secara total dibanding tahun sebelumnya.

## Transisi dari Purpur ke Forge

Pada NUMI Minecraft 2025, server dibangun menggunakan Purpur. Purpur adalah turunan dari Paper yang berfokus pada optimisasi performa server berbasis plugin. Dengan pendekatan ini, fitur ditambahkan melalui API Bukkit/Spigot tanpa mengubah struktur fundamental Minecraft. Plugin berjalan di atas abstraction layer yang relatif stabil dan dirancang memang untuk server-side customization.

Arsitektur ini sangat cocok untuk survival server dengan economy, quest berbasis plugin, serta optimisasi entity dan tick. Bahkan untuk kebutuhan multiplatform, pendekatan ini jauh lebih fleksibel karena kompatibel dengan GeyserMC dan Floodgate sehingga Java dan Bedrock bisa berjalan bersama.

{{ figure(src="/images/numi-minecraft-26/image_1.png", alt="Header Image", caption="Boss Monster NUMI Minecraft") }}

Namun kebutuhan 2026 berbeda. Kita ingin membangun sistem RPG yang tidak hanya menambahkan fitur, tetapi memodifikasi cara game bekerja. Sistem stat, scaling combat, custom world generation, hingga mekanik boss yang kompleks membutuhkan akses lebih dalam terhadap engine Minecraft. Di titik ini, plugin-based approach tidak lagi cukup.


Karena itu kita beralih ke Forge yang mod loader yang bekerja jauh lebih dalam dibanding sistem plugin. Mod dapat melakukan registrasi item dan block baru, menambahkan entity, memodifikasi AI behavior, mengubah world generation pipeline, bahkan menambahkan dimension baru. Forge menggunakan sistem event bus dan registry yang memungkinkan mod untuk terintegrasi langsung ke lifecycle game.

{{ figure(src="/images/numi-minecraft-26/image_6.png", alt="Header Image", caption="Class di NUMI Minecraft") }}

Dengan lebih dari 500 mod aktif, NUMI 2026 bukan lagi server vanilla yang ditambahi fitur, melainkan environment modded penuh dengan ekosistem internal yang kompleks.

## Perbedaan Fundamental antara Plugin dan Mod

Perbedaan utama antara Purpur dan Forge terletak pada layer integrasi. Pada Purpur, plugin bekerja melalui API yang sudah ditentukan. Ada batasan jelas mengenai apa yang bisa diakses dan dimodifikasi. Plugin cenderung lebih aman dari sisi stabilitas karena tidak mengubah core class secara langsung. Konsekuensinya, fleksibilitasnya terbatas.

Forge memungkinkan mod untuk masuk lebih dalam ke sistem internal Minecraft. Mod dapat melakukan injection melalui berbagai mekanisme event dan registry, yang berarti mereka dapat mengubah hampir semua aspek gameplay. Fleksibilitas ini datang dengan harga: kompleksitas dependency, potensi konflik antar mod, dan beban performa yang jauh lebih tinggi.

{{ figure(src="/images/numi-minecraft-26/image_5.png", alt="Header Image", caption="Error Handling Mod pada Forge") }}

Dari sisi performa, Purpur unggul dalam optimisasi tick dan entity handling karena memang dirancang untuk efisiensi server vanilla. Forge tidak difokuskan pada optimisasi vanilla, melainkan pada extensibility. Dalam konteks NUMI 2026, kebutuhan terhadap fleksibilitas sistem RPG jauh lebih besar daripada kebutuhan terhadap optimisasi vanilla-level.

## Perbedaan Client pada Environment Purpur dan Forge

Perbedaan lainnya yang sangat signifikan adalah sisi client. Pada arsitektur berbasis Purpur, pemain dapat menggunakan client vanilla selama protokol kompatibel. Fitur tambahan berada di sisi server melalui plugin, sehingga client tidak perlu memiliki mod khusus untuk dapat terhubung.

Dalam environment Forge, hal ini berubah total. Karena kita menggunakan lebih dari 500 mod, client juga wajib menggunakan modpack yang sama. Registry item, block, entity, dan dimension harus sinkron antara client dan server. Jika terjadi mismatch pada registry mapping, client tidak akan bisa join.

Artinya distribusi modpack menjadi bagian dari arsitektur sistem. Kita tidak hanya mengelola server, tetapi juga memastikan environment client berada dalam kondisi yang identik dengan server. Ini meningkatkan kompleksitas operasional, tetapi memberi kita kontrol penuh terhadap gameplay.

## Karakteristik Minecraft sebagai Sistem CPU-Bound

Terlepas dari Purpur atau Forge, satu hal yang tetap konsisten adalah bahwa Minecraft server secara fundamental bersifat CPU-bound. Target operasionalnya adalah 20 tick per detik, yang berarti setiap tick memiliki anggaran waktu sekitar 50 milidetik.

{{ figure(src="/images/numi-minecraft-26/image_2.png", alt="Header Image", caption="Tick Rate Stats") }}

Dalam setiap tick, server harus memproses entity update, AI pathfinding, physics calculation, block update, scheduled task, serta berbagai event tambahan. Dengan 500+ mod aktif, jumlah event listener meningkat drastis. Banyak mod menambahkan logic pada berbagai lifecycle event, sehingga waktu eksekusi per tick bertambah signifikan.

Jika waktu eksekusi melampaui 50 milidetik secara konsisten, TPS turun dan pemain merasakan lag. Karena itu pemilihan CPU menjadi krusial. Kita menggunakan AMD EPYC 7003 High Series dengan fokus pada stabilitas performa jangka panjang dan kemampuan menangani sustained load pada thread utama.

## Perencanaan RAM dan Validasi Penggunaan

Walaupun bottleneck utama berada di CPU, penggunaan RAM pada server modded skala besar tetap menjadi faktor kritis. Banyak penyedia hosting menyebut bahwa 3GB cukup untuk sekitar 25 mod, 4GB untuk 35–40 mod, dan 5–10GB untuk lebih dari 40 mod. Bahkan 16GB sering dikategorikan sebagai konfigurasi extreme modpack.

{{ figure(src="/images/numi-minecraft-26/image_4.png", alt="Header Image", caption="Kalkulasi Ram berdasarkan Apex Hosting") }}

NUMI 2026 berjalan dengan lebih dari 500 mod. Base load server bersama seluruh modpack sudah berada di kisaran 6 hingga 8GB sebelum aktivitas eksplorasi besar. Ketika pemain aktif membuka chunk baru dan sistem RPG mulai berjalan secara intensif, penggunaan memori meningkat secara signifikan. Dalam kondisi peak dengan 15 hingga 20 pemain aktif, estimasi realistis berada di rentang 18 hingga 20GB.

Karena itu kita memilih 32GB RAM sebagai capacity planning dengan headroom yang cukup untuk menjaga stabilitas garbage collection dan menghindari memory pressure mendadak saat spike event terjadi.

## World Generation dan Pregen Menggunakan Chunky

World generation adalah salah satu beban terberat dalam server modded. Setiap chunk baru memicu proses biome selection, structure placement, distribusi resource tambahan, serta berbagai feature dari mod. Kompleksitas ini jauh lebih tinggi dibanding vanilla.

Pada fase awal tanpa pre-generation, eksplorasi simultan menyebabkan spike CPU dan disk I/O yang cukup signifikan. Untuk mengatasi hal tersebut, kita menggunakan Chunky untuk melakukan pre-generation world sebelum server resmi dibuka.

{{ figure(src="/images/numi-minecraft-26/image_3.png", alt="Header Image", caption="Pre Generation World dengan Chunky") }}

Dengan pregen, sebagian besar beban komputasi world generation dilakukan dalam kondisi terkontrol. Ketika pemain masuk dan melakukan eksplorasi, server tidak perlu lagi melakukan komputasi berat secara real-time. Dampaknya sangat terasa pada stabilitas tick dan konsistensi TPS.

## Kesimpulan

NUMI Minecraft 2026 adalah perubahan arsitektur secara menyeluruh dibanding tahun sebelumnya. Dari Purpur berbasis plugin dan optimisasi multiplatform, kita beralih ke Forge dengan lebih dari 500 mod aktif dan kontrol penuh terhadap sistem gameplay.

Pilihan ini meningkatkan kompleksitas, kebutuhan resource, serta tanggung jawab operasional. Namun keputusan tersebut diambil secara sadar demi mencapai depth gameplay dan kualitas sistem RPG yang tidak mungkin dicapai melalui pendekatan plugin-based.

Dengan 32GB RAM, AMD EPYC 7003 High Series, pre-generation world menggunakan Chunky, serta integrasi mod skala besar, server tahun ini dirancang dengan pendekatan capacity planning dan engineering yang jauh lebih matang.

Dan meskipun implementasi terbesar ada di tangan Ryan dan Autodie, pengalaman ini tetap menjadi pembelajaran penting tentang bagaimana merancang server Minecraft modded skala komunitas secara serius, terukur, dan stabil di bawah beban nyata.