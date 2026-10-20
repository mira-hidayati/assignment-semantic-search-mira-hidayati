INDIVIDUAL ASSIGNMENT - SEMANTIC SEARCH & LLM APPLICATION
Retrieval Test Results
Mira Hidayati (CAI02)

**BERIKUT HASIL EKSEKUSI DARI SETIAP SECTION**

**SECTION-1: IMPORT LIBRARY & LOAD KNOWLEDGE BASE**
Total entri knowledge base: 59
faq_id	category	question	answer
0	1	Pemesanan	Bagaimana cara melakukan pemesanan di platform...	Pilih produk yang diinginkan, klik 'Tambah ke ...
1	2	Pemesanan	Apakah saya bisa memesan tanpa membuat akun?	Saat ini pemesanan hanya bisa dilakukan setela...
2	3	Pemesanan	Bagaimana cara membatalkan pesanan yang sudah ...	Pesanan dapat dibatalkan melalui halaman 'Pesa...
3	4	Pemesanan	Berapa lama waktu yang dibutuhkan sebelum pesa...	Penjual biasanya memproses pesanan dalam waktu...
4	5	Pemesanan	Apakah saya bisa mengubah alamat pengiriman se...	Perubahan alamat hanya bisa dilakukan sebelum ...


**SECTION-2: MENYIAPKAN UNIT RETRIEVABLE**
Contoh unit retrievable:
Q: Bagaimana cara melakukan pemesanan di platform ini? A: Pilih produk yang diinginkan, klik 'Tambah ke Keranjang', lalu lanjutkan ke halaman checkout untuk mengisi alamat pengiriman dan metode pembayaran.


**SECTION-3: MEMBUAT EMBEDDINGS**
Shape embeddings: (59, 3072)


**SECTION-4: MENYIMPAN EMBEDDINGS KE VECTOR DATABASE (FAISS)**
Jumlah vektor tersimpan di index: 59


**SECTION-5: MENGIMPLEMENTASIKAN SEMANTIC SEARCH**
{'rank': 1, 'question': 'Bagaimana jika paket saya belum sampai melebihi estimasi?', 'answer': 'Jika paket melebihi estimasi pengiriman lebih dari 3 hari, silakan hubungi customer service dengan menyertakan nomor resi untuk ditelusuri lebih lanjut.', 'category': 'Pengiriman', 'distance': np.float32(0.33353508)}
{'rank': 2, 'question': 'Kenapa status pengiriman saya tidak berubah selama beberapa hari?', 'answer': 'Status yang tidak berubah bisa disebabkan keterlambatan update sistem kurir. Disarankan menunggu 1x24 jam sebelum melaporkan ke customer service.', 'category': 'Pengiriman', 'distance': np.float32(0.4088098)}
{'rank': 3, 'question': 'Kenapa nomor resi saya belum bisa dilacak di website kurir?', 'answer': "Nomor resi baru dapat dilacak setelah paket diserahterimakan ke pihak kurir, biasanya butuh waktu beberapa jam setelah status 'Dikirim'.", 'category': 'Pengiriman', 'distance': np.float32(0.4320181)}


**SECTION-6: MENDESAIN PROMPT DAN CONTEXT**
Note: tidak ada output untuk section ini karena hanya mendefinisikan fungsi.


**SECTION-7: MENGINTEGRASIKAN LLM**
Jawaban LLM:
Halo! Ya, fitur cicilan tersedia untuk transaksi dengan kartu kredit tertentu pada nominal minimal yang ditentukan oleh pihak bank. 

Jika ada hal lain yang ingin ditanyakan, jangan ragu untuk memberi tahu saya ya!

Sumber FAQ yang digunakan:
- Apakah saya bisa membayar dengan cicilan?
- Apakah bisa membayar menggunakan lebih dari satu metode sekaligus?
- Metode pembayaran apa saja yang tersedia?


**SECTION-8: MENGUJI RELEVANSI HASIL RETRIEVAL DAN RESPONSE AI**
Catatan: output dibawah menunjukkan adanya error 503 UNAVAILABLE ketika 5 pertanyaan ditanyakan sekaligus.  Workaround yg dilakukan utk mendapatkan semua jawabah adalah dengan mengurangi jumlah pertanyaan dalam 1 waktu. Jawaban lengkap yang berisi keseluruhan jawaban dari 5 pertanyaan dibawah ada di file README.

Query: bagaimana cara melacak pesanan saya?
Error API: 503 UNAVAILABLE. {'error': {'code': 503, 'message': 'This model is currently experiencing high demand. Spikes in demand are usually temporary. Please try again later.', 'status': 'UNAVAILABLE'}}
Silakan coba jalankan cell ini lagi nanti.
---
Query: berapa lama waktu pengiriman?
Error API: 503 UNAVAILABLE. {'error': {'code': 503, 'message': 'This model is currently experiencing high demand. Spikes in demand are usually temporary. Please try again later.', 'status': 'UNAVAILABLE'}}
Silakan coba jalankan cell ini lagi nanti.
---
Query: pesanan saya bisa dibatalin gak sih kalau udah transfer?
Jawaban: Halo! Tentu bisa, pesanan kamu dapat dibatalkan melalui halaman **'Pesanan Saya'** selama statusnya masih **'Diproses'**. 

Namun, jika status pesanan kamu sudah berubah menjadi **'Dikirim'**, pembatalan sudah tidak dapat dilakukan ya. 

Semoga membantu! Ada hal lain yang bisa saya bantu?
Sumber teratas: Bagaimana cara membatalkan pesanan yang sudah dibuat?
---
Query: paket pas nyampe cacat, cara nukernya gimana?
Error API: 503 UNAVAILABLE. {'error': {'code': 503, 'message': 'This model is currently experiencing high demand. Spikes in demand are usually temporary. Please try again later.', 'status': 'UNAVAILABLE'}}
Silakan coba jalankan cell ini lagi nanti.
---
Query: apakah kalian menjual asuransi kendaraan bermotor?
Jawaban: Halo! Mohon maaf, saat ini saya belum memiliki informasi mengenai penjualan asuransi kendaraan bermotor. 

Untuk bantuan dan informasi lebih lanjut, silakan langsung menghubungi tim customer service kami, ya. Terima kasih!
Sumber teratas: Apakah ongkos kirim sudah termasuk asuransi pengiriman?
---


