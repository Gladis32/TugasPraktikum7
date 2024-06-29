# Tugas Praktikum 7 { Pertemuan ke 15 } <img src=https://logos-download.com/wp-content/uploads/2016/05/MySQL_logo_logotype.png width="130px" >

|**Nama**|**NIM**|**Kelas**|**Matkul**|
|----|---|-----|------|
|Gladis Toti Anggraini |312310566|TI.23.A5|Basis Data|

## INPUT DATA

### 1. Perusahaan
```
CREATE TABLE Perusahaan(
id_p VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
alamat VARCHAR(45)
);

INSERT INTO Perusahaan VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);

SELECT * FROM Perusahaan;
```
![tabel1](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/b752d6dc-912f-41c6-b4c5-138f93833c70)

### 2. Departemen
```
CREATE TABLE Departemen(
id_dept VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_p VARCHAR(10) NOT NULL,
sup_nik VARCHAR(10),
FOREIGN KEY (id_p) REFERENCES Perusahaan(id_p),
FOREIGN KEY (sup_nik) REFERENCES Karyawan(nik)
);

INSERT INTO Departemen VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);

SELECT * FROM Departemen;
```

![tabel2](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/206f4190-50a6-4d28-a551-37c3ce165dde)

### 3. Karyawan
```
CREATE TABLE Karyawan (
nik VARCHAR(10) PRIMARY KEY,
nama VARCHAR(50),
id_dept VARCHAR(10),
sup_nik VARCHAR(10),
gaji_pokok INT,
FOREIGN KEY (id_dept) REFERENCES Departemen(id_dept),
FOREIGN KEY (sup_nik) REFERENCES Karyawan(nik)
);

INSERT INTO Karyawan (nik, nama, id_dept, sup_nik, gaji_pokok) VALUES
('N01', 'Ari', 'D01', NULL, 2000000),
('N02', 'Dina', 'D01', NULL, 2500000),
('N03', 'Rika', 'D03', NULL, 2400000),
('N04', 'Ratih', 'D01', 'N01', 3000000),
('N05', 'Riko', 'D01', 'N01', 2800000),
('N06', 'Dani', 'D02', NULL, 2100000),
('N07', 'Anis', 'D02', 'N06', 5000000),
('N08', 'Dika', 'D02', 'N06', 4000000),
('N09', 'Raka', 'D03', 'N06', 2000000);

SELECT * FROM Karyawan;
```

![tabel3](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/d8e20d10-32e6-4f24-a0d9-6bddc512c7e3)

### 4. Project

```
CREATE TABLE Project(
id_proj VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
tgl_mulai DATE,
tgl_selesai DATE,
status INT
);

INSERT INTO Project VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');

SELECT * FROM Project;
```

![tabel4](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/f3ec1de8-bbdb-4159-8052-4a73b4178df1)

### 5. Project Detail
```
CREATE TABLE Project_detail(
id_proj VARCHAR(10),
nik VARCHAR(10),
PRIMARY KEY (id_proj, nik),
FOREIGN KEY (id_proj) REFERENCES proyek(id_proj),
FOREIGN KEY (nik) REFERENCES pegawai(nik)
);

INSERT INTO Project_detail VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');

SELECT * FROM Project_detail;
```

![tabel5](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/a55e894a-840a-47db-857e-43abe80bc6b7)


## Latihan

### 1. Tampilkan data karyawan yang bekerja pada departemen yang samadengan karyawan yang bernama Dika

```
SELECT nik, nama, id_dept FROM Karyawan WHERE id_dept = (SELECT id_dept FROM Karyawan WHERE nama = 'Dika');
```
Query ini menggunakan `subquery` untuk mendapatkan departemen dari karyawan yang bernama 'Dika', lalu menggunakan hasil tersebut untuk memfilter data karyawan yang memiliki departemen yang sama.

![2](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/bb9dbf3c-7aea-4025-8cfc-b787735010a4)

### 2. Tampilkan data karyawan yang gajinya lebih besar dari rata-rata gaji semuakaryawan. urutkan menurun berdasarkan besaran gaji

```
SELECT nik, nama, id_dept, gaji_pokok FROM karyawan WHERE gaji_pokok > (SELECT AVG(gaji_pokok) FROM Karyawan) ORDER BY gaji_pokok DESC;
```

Rata-rata gaji semua karyawan dihitung menggunakan fungsi `AVG` dan hasilnya digunakan untuk memfilter data karyawan yang memiliki gaji lebih besar. Data karyawan yang memenuhi kriteria tersebut kemudian diurutkan menurun berdasarkan besaran gaji menggunakan sintaks `ORDER BY`.

![2](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/90313a74-63af-447a-94b9-f975690feee5)


### 3. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja didepartment yang sama dengan karyawan dengan nama yang mengandunghuruf 'K'.

```
SELECT nik, nama FROM Karyawan WHERE id_dept IN (SELECT id_dept FROM Karyawan WHERE nama LIKE '%K%');
```

query ini menggunakan fungsi `IN` untuk memfilter data karyawan yang memiliki departemen yang sama dengan departemen karyawan yang memiliki nama yang mengandung huruf 'K'.

![3](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/d5455e93-a95f-48bc-8deb-9d22cba6943c)



### 4. Tampilkan data karyawan yang bekerja pada departemen yang ada dikantor pusat.

```
SELECT karyawan.nik, karyawan.nama, karyawan.id_dept FROM karyawan JOIN departemen ON karyawan.id_dept = departemen.id_dept WHERE departemen.id_p = 'P01';
```

`JOIN` untuk menggabungkan tabel karyawan dan departemen berdasarkan kolom `id_dept`, lalu memfilter data karyawan yang memiliki departemen yang ada di kantor pusat dengan menggunakan sintaks `WHERE`.

![4](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/497ab480-789c-468f-a54d-6ad6ffc2c393)

### 5. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja didepartment yang sama dengan karyawan dengan nama yang mengandunghuruf 'K' dan yang gajinya lebih besar dari rata-rata gaji semua karyawan

```
SELECT DISTINCT k1.nik, k1.nama FROM karyawan k1 JOIN karyawan k2 ON k1.id_dept = k2.id_dept WHERE k1.gaji_pokok > (SELECT AVG(gaji_pokok) FROM karyawan WHERE nama LIKE '%K%');
```

`JOIN` untuk menggabungkan tabel karyawan, lalu memfilter data karyawan yang memenuhi kriteria menggunakan sintaks `WHERE`. Data karyawan yang memenuhi kriteria tersebut kemudian diurutkan menggunakan sintaks `SELECT DISTINCT` untuk menghilangkan duplikasi.

![5](https://github.com/Gladis32/Tugas_Praktikum7/assets/148181064/f6a93580-f7b6-4637-a788-85ccac661d5e)


