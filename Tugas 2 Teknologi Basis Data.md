# Nama: Aulia Wardani
# NIM: 121450034
# Kelas: RC

Artikel ini membahas berbagai metode penyimpanan dan akses gambar menggunakan Python, dengan fokus pada LMDB dan HDF5. Artikel ini menekankan pentingnya perencanaan yang matang ketika menangani kumpulan data gambar berukuran besar dan memberikan wawasan tentang pertimbangan utama untuk penyimpanan yang efisien. Selain itu, artikel ini juga menyebutkan kumpulan data spesifik yang digunakan untuk eksperimen serta mengulas kelebihan dan kekurangan metode penyimpanan yang dibahas. Penting untuk memahami keputusan desain di balik sistem penyimpanan ini untuk implementasi yang efektif.

Untuk memastikan bahwa sebagian besar pembacaan dilakukan secara berurutan saat menyimpan gambar, strategi berikut dapat dipertimbangkan:

1. **Optimalisasi Organisasi File**: Menyusun gambar secara berurutan saat menyimpannya ke disk. Hal ini mempermudah pembacaan berurutan nanti.

2. **Batch Processing**: Menyimpan gambar dalam batch atau grup yang terkait secara logis. Ini memfasilitasi pembacaan berurutan saat mengakses kumpulan ini.

3. **Manajemen Metadata**: Menyertakan metadata atau informasi pengindeksan untuk mengatur gambar secara berurutan. Metadata ini dapat digunakan untuk mengambil gambar secara efisien dalam urutan tertentu.

4. **Pra-Pemrosesan**: Memproses gambar sebelum menyimpannya untuk memastikan gambar disimpan dalam format yang memungkinkan akses berurutan. Ini mungkin melibatkan pengubahan ukuran, kompresi, atau konversi gambar ke format standar.

Dengan menerapkan strategi ini, Anda dapat mengoptimalkan penyimpanan gambar untuk memfasilitasi pembacaan berurutan, yang bermanfaat saat bekerja dengan kumpulan data gambar besar.

Saat memilih kunci yang baik untuk menyimpan gambar, pertimbangkan hal-hal berikut:

1. **Keunikan**: Pastikan setiap kunci bersifat unik untuk menghindari konflik atau penimpaan data. Menggunakan pengidentifikasi unik untuk setiap gambar membantu menjaga integritas data.

2. **Deskriptif**: Pilih kunci yang deskriptif dan bermakna. Ini mempermudah pengambilan gambar tertentu berdasarkan karakteristik atau atributnya.

3. **Konsistensi**: Pertahankan konsistensi dalam konvensi penamaan kunci untuk memfasilitasi pengorganisasian dan pengambilan gambar. Format kunci yang konsisten menyederhanakan proses penyimpanan dan akses.

4. **Efisiensi**: Pilih kunci yang memungkinkan pengambilan gambar secara efisien. Pertimbangkan penggunaan kunci yang mudah diindeks atau dicari untuk meningkatkan kinerja saat mengakses gambar.

5. **Skalabilitas**: Rencanakan skalabilitas dengan memilih kunci yang dapat mengakomodasi pertumbuhan kumpulan data di masa mendatang. Struktur kunci yang dapat diskalakan mendukung pengelolaan gambar dalam jumlah besar secara efisien.

Dengan mempertimbangkan faktor-faktor ini saat memilih kunci untuk penyimpanan gambar, Anda dapat mengoptimalkan pengorganisasian dan pengambilan gambar dalam kumpulan data.

Menghitung `map_size` yang sesuai untuk menyimpan gambar serta mengantisipasi potensi perubahan kumpulan data di masa mendatang melibatkan langkah-langkah berikut:

1. **Perkirakan Ukuran Kumpulan Data Saat Ini**: Mulailah dengan memperkirakan ukuran kumpulan data saat ini. Ini bisa dilakukan dengan menghitung ukuran total semua gambar dan metadata terkait yang ingin Anda simpan.

2. **Faktor Pertumbuhan**: Antisipasi potensi perubahan ukuran kumpulan data di masa mendatang. Pertimbangkan laju pertumbuhan data, perkiraan penambahan data, dan perubahan ukuran atau format gambar.

3. **Buffer untuk Ekspansi**: Alokasikan buffer atau overhead untuk mengakomodasi pertumbuhan kumpulan data di masa depan. Buffer ini harus cukup untuk mengakomodasi gambar dan metadata tambahan tanpa perlu sering menyesuaikan `map_size`.

4. **Pertimbangan Batasan Sistem**: Pertimbangkan batasan atau batas sistem penyimpanan yang digunakan. Pastikan `map_size` yang dihitung tidak melebihi kapasitas maksimum yang didukung oleh sistem.

5. **Pemantauan dan Penyesuaian Reguler**: Pantau ukuran kumpulan data dan pola penggunaan secara berkala. Sesuaikan `map_size` sesuai kebutuhan agar selaras dengan persyaratan kumpulan data yang terus berkembang.

Dengan mengikuti langkah-langkah ini dan secara proaktif merencanakan perubahan kumpulan data di masa mendatang, Anda dapat menghitung `map_size` yang sesuai untuk menyimpan gambar sehingga memungkinkan skalabilitas dan pengelolaan data gambar yang efisien dari waktu ke waktu.

# Contoh Materialized Views:

1. Materialized view untuk menampilkan total kapasitas ruangan per gedung:
```
CREATE MATERIALIZED VIEW mv_total_capacity_per_building AS
SELECT building, SUM(capacity) AS total_capacity
FROM classroom
GROUP BY building;
```

2. Materialized view untuk menampilkan total anggaran departemen per gedung:
```
CREATE MATERIALIZED VIEW mv_total_budget_per_building AS
SELECT d.building, SUM(d.budget) AS total_budget
FROM department d
GROUP BY d.building;
```

3. Materialized view untuk menampilkan rata-rata gaji instruktur per departemen:
```
CREATE MATERIALIZED VIEW mv_avg_salary_per_department AS
SELECT i.dept_name, AVG(i.salary) AS avg_salary
FROM instructor i
GROUP BY i.dept_name;
```

4. Materialized view untuk menyimpan hasil join antara tabel mahasiswa dan mata kuliah yang diambil:
```
CREATE MATERIALIZED VIEW mv_student_course_join AS
SELECT s.ID, s.name, t.course_id, t.grade
FROM student s
JOIN takes t ON s.ID = t.ID;
```

5. Materialized view untuk menampilkan total kredit yang diambil oleh setiap mahasiswa:
```
CREATE MATERIALIZED VIEW mv_total_credits_per_student AS
SELECT ID, SUM(tot_cred) AS total_credits
FROM student
GROUP BY ID;
```

# Contoh Transaksi:

1. Transaksi untuk menambahkan departemen baru ke dalam database:
```
BEGIN TRANSACTION;
INSERT INTO department (dept_name, building, budget) 
VALUES ('New Department', 'New Building', 100000.00);
COMMIT;
```

2. Transaksi untuk mengupdate gaji seorang instruktur:
```
BEGIN TRANSACTION;
UPDATE instructor 
SET salary = 50000.00
WHERE ID = '12345';
COMMIT;
```

3. Transaksi untuk menghapus mata kuliah yang tidak diinginkan dari sebuah departemen:
```
BEGIN TRANSACTION;
DELETE FROM course 
WHERE dept_name = 'Computer Science' AND credits = 3;
COMMIT;
```

4. Transaksi untuk menambahkan mahasiswa baru ke dalam database:
```
BEGIN TRANSACTION;
INSERT INTO student (ID, name, dept_name, tot_cred) 
VALUES ('54321', 'New Student', 'Mathematics', 0);
COMMIT;
```

5. Transaksi untuk mengupdate nilai seorang mahasiswa pada suatu mata kuliah:
```
BEGIN TRANSACTION;
UPDATE takes 
SET grade = 'A+'
WHERE ID = '54321' AND course_id = 'CS101' AND semester = 'Fall' AND year = 2023;
COMMIT;
```
