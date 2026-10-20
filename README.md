INDIVIDUAL ASSIGNMENT - Semantic Search & LLM Application
Advanced Prompt Engineering & Context Management
README File
Mira Hidayati (CAI02)

**PROBLEM STATEMENT:**
**Objective:** 
Membangun prototype asisten AI yang dapat menjawab pertanyaan pelanggan secara otomatis berdasarkan knowledge base FAQ yang sudah ada, sehingga memudahkan penanganan customer service e-commerce.

**Sumber dan Struktur Knowledge Base:** 
Sumber: knowledge base FAQ yang sudah ada
Struktur data: berisi kolom faq_id, category, question dan answer

**Batasan/Asumsi:** 
- Dataset difokuskan pada teks berbahasa Indonesia.
- Karena menggunakan API Gemini, environment harus terhubung ke internet saat menjalankan prediksi LLM.

**Dataset: ecommerce_faq_knowledge_base.csv**

**Struktur Prompt**
Persona: "asisten customer service"
Task: "Jawab pertanyaan pengguna" 
Guardrails: "HANYA berdasarkan informasi FAQ berikut". 
Instruksi pembatasan ini sangat penting agar AI tidak berhalusinasi mengarang jawaban di luar sistem

**ALUR SISTEM:**
1. Ingest: Memuat knowledge base FAQ dan menyiapkannya sebagai unit-unit retrievable
2. Embed: Mengubah setiap unit retrievable menjadi vektor menggunakan Gemini Embeddings API
3. Store: Menyimpan seluruh embedding ke dalam vector database (FAISS)
4. Retrieve: Menjalankan semantic search untuk mengambil entri FAQ paling relevan terhadap query
5. Generate: Menyisipkan hasil retrieval sebagai context ke dalam prompt, lalu menghasilkan jawaban melalui LLM

**HASIL PENGUJIAN QUERY BESERTA ANALISI RELEVANSI** 
Berikut 5 pertanyaan yang saya berikan, berikut jawabannya:
1. Query yang jelas ada jawabannya di FAQ: 
"bagaimana cara melacak pesanan saya?"

JAWABAN:
Halo! Untuk melacak pesanan Anda, caranya sangat mudah:
1. Anda dapat memeriksa status pesanan secara *real-time* melalui menu **'Pesanan Saya'** di akun Anda.
2. Setelah barang dikirim, nomor resi akan muncul di halaman **'Pesanan Saya'** tersebut.
3. Anda bisa menggunakan nomor resi tersebut untuk melacak posisi paket langsung di website kurir terkait.Semoga membantu! Jika ada hal lain yang ingin ditanyakan, silakan beri tahu saya.Sumber teratas: Bagaimana cara melacak paket yang sedang dikirim?

2. Query yang jelas ada jawabannya di FAQ: 
"berapa lama waktu pengiriman?"

JAWABAN:
Halo! Untuk estimasi waktu pengiriman barang, umumnya membutuhkan waktu 2-5 hari kerja untuk area dalam kota dan 5-10 hari kerja untuk luar pulau, tergantung pada jasa kirim yang kamu pilih. 

Semoga membantu! Jika ada hal lain yang ingin ditanyakan, silakan beri tahu kami.
Sumber teratas: Berapa lama estimasi waktu pengiriman barang?

3. Query dengan kata-kata berbeda/parafrase (bahasa kasual)
    "pesanan saya bisa dibatalin gak sih kalau udah transfer?

JAWABAN:
Halo! Pesanan Anda masih bisa dibatalkan melalui halaman 'Pesanan Saya' selama status pesanan masih **'Diproses'**. Namun, jika statusnya sudah berubah menjadi **'Dikirim'**, pembatalan sudah tidak dapat dilakukan. 

Semoga informasi ini membantu! Jika ada hal lain yang ingin ditanyakan, silakan beri tahu saya ya.
Sumber teratas: Bagaimana cara membatalkan pesanan yang sudah dibuat?

4. Query dengan kata-kata berbeda/parafrase
    "paket pas nyampe cacat, cara nukernya gimana?"

JAWABAN:
Halo! Mohon maaf atas kendala yang kamu alami. 

Jika paket diterima dalam kondisi rusak atau cacat, langkah yang dapat kamu lakukan adalah:
1. Segera ambil foto atau video kondisi paket saat diterima sebagai bukti.
2. Laporkan kendala tersebut melalui **menu komplain** dalam waktu maksimal **2x24 jam** sejak barang diterima.

Terkait prosedur penukaran barang secara spesifik, saat ini informasi tersebut belum tersedia di panduan kami. Silakan langsung menghubungi tim customer service kami untuk mendapatkan bantuan lebih lanjut terkait penukaran barang, ya. Terima kasih!
Sumber teratas: Apa yang harus dilakukan jika paket diterima dalam kondisi rusak?

5. Query yang TIDAK ada jawabannya di FAQ (Uji guardrail)
    "apakah kalian menjual asuransi kendaraan bermotor?"

JAWABAN:
Halo! Mohon maaf, saat ini kami belum memiliki informasi mengenai penjualan asuransi kendaraan bermotor. 

Untuk bantuan dan informasi lebih lanjut, kami sarankan untuk langsung menghubungi tim customer service kami, ya. Terima kasih!
Sumber teratas: Apakah ongkos kirim sudah termasuk asuransi pengiriman?

**CATATAN ANALISIS RELEVANSI:**

1. Query Eksplisit (Ada di FAQ): Sistem berhasil menemukan sumber informasi yang sangat relevan dan memberikan jawaban yang akurat, meskipun terdapat sedikit perbedaan diksi (contoh: pesanan vs paket).

2. Query Parafrase/Bahasa Kasual: Model embedding menunjukkan performa yang baik dalam menangkap konteks dan niat pengguna (user intent), bukan hanya mencocokkan kata kunci eksak.

3. Query Out-of-Domain (Tidak ada di FAQ): Guardrail pada prompt terbukti efektif. Ketika diberikan pertanyaan di luar konteks e-commerce (misal: asuransi kendaraan), LLM tidak berhalusinasi mengarang jawaban, melainkan mematuhi instruksi untuk menyatakan ketidaktahuan dan menyarankan pengguna menghubungi customer service.

**KETERBATASAN SISTEM YANG DITEMUKAN**
1. Ketika menjalankan 5 query sekaligus, sistem terkendala error 503 UNAVAILABLE, yang kemungkinan terjadi saat server API Google Gemini sedang mengalami lonjakan permintaan yang tinggi (high demand), atau karena 5 pertanyaandikirimkan sekaligus secara beruntun dalam perulangan (looping) sehingga terkena limitasi sementara.  Solusinya dengan menambahkan jeda waktu antar setiap pertanyaan, misalnya 5-detik.

2. Setelah diberi jeda 5-detik antar pertanyaan, ketika kode dijalankan dengan memberikan 5 pertanyaan sekaligus, masih terkendala dengan error 503 UNAVAILABLE.  Supaya eksekusi tidak gantung, maka perlu diberikan exception, agar eksekusi bisa selesai.

3. Setelah diberi jeda, dan diberi exception ternyata masih ada pertanyan yg tidak terjawab, maka jalan keluar terakhir dengan mengurangi jumlah pertanyaan yg dikirim  dalam satu waktu, pertanyaan lain di-comment sementara.    

**CARA MENJALANKAN NOTEBOOK/SCRIPT**
1. Source code: /notebook/starter_notebook_semantic_search.ipynb
2. Sebelum menjalankan project, pastikan hal2 berikut:
   a. Data "ecommerce_faq_knowledge_base.csv" tersedia di subfolder "data/"
   b. GEMINI_API_KEY sudah dimuat dengan benar di environment variables sistem Windows agar API-nya tidak error. 
   d. Pastikan versi Gemini API yang digunakan sudah sesuai dengan endpoint terbaru Google AI Studio
   e. Kernel sudah dipilih
   f. Python environment sudah aktif.
   g. Install library dengan menjalankan pip install 
3. Jika semua point di no. 2 sudah dipastikan, jalankan (run all cells) di VS Code yang sudah terhubung dengan environment Python 3.14. dan lihat hasilnya.
