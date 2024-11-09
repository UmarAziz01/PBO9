# PBO9
Penugasan matakuliah pemrograman berorientasi obyek pertemuan keduabelas yakni mengubah program crud dari Repo PBO8 yang awalnya menggunakan JDBC PostgreSQL sekarang ditambahkan PERSISTENCE UNIT atau PERSISTENSI UNIT agar data tidak hilang ketika sistem ataupun program di restart secara tiba tiba yang biasanya terjadi karena sistem/program yang mengalami suatu kesalahan fungsi, selain itu juga persistensi berguna untuk saling bertikar data (Integrasi Sistem) yang memungkinkan banyak aplikasi menjadi terintegrasi dalam satu Database. Dan masih benyak lagi kegunaan persistensi.

Untuk itu saya akan berbagi cara untuk membuat program CRUD (Create Read Update Delete) dengan bahasa java (JDK versi 22) yang akan menggunakan JDBC PostgreSQL sebagai DBMS dan EclipseLink(JPA 2.2) sebagai Persistence Library.
NOTE : Gunakan Aplikasi Apache NetBeans Di versi selain versi 21, karena pada versi 21 kita tidak bisa mencreate Entity Clases from Database sebab adanya bug pada form seperti dibawah ini. Untuk itu saya sarankan agar menggunakan versi 21 keatas bisa versi ke 22 ataupun 23  (Saya akan mempraktikan dengan Apache NetBeans versi ke 23) -versi terbaru ketika Repository ini dibuat-.
![image](https://github.com/user-attachments/assets/2715a465-cf2b-4567-92d6-7d5d20ec891c)
Foto bug pada Apache NetBeans Di versi 21


Kumpulan Link Pendukung :

https://github.com/UmarAziz01/PBO8

https://github.com/UmarAziz01/PBO7

https://github.com/UmarAziz01/PBO6

https://github.com/UmarAziz01/PBO_UTS

https://drive.google.com/drive/folders/12E6Avw_WMRfr2LmdsNi6VL3eu4o3m3DJ?usp=sharing


Berikut langkah-langkahnya : 

- Karena perintahnya mengubah kode yang sudah ada (PBO8) dari SQL menjadi Persistence, maka saya akan menyalin projectnya dan mengubah namanya menjadi "PBO_Persistence"
  ![image](https://github.com/user-attachments/assets/f2de5025-d9f5-4944-baed-c8e8c9fa7cfe)
  ![image](https://github.com/user-attachments/assets/154cbbea-83b8-49b6-bf71-3c632f5314e1)
  Dan Klik “Copy” lalu setelah itu cari project dengan nama PBO_Persistence dan klik copy lalu klik 2x untuk membuka project baru yang telah dibuat
  ![image](https://github.com/user-attachments/assets/06132d7c-d0fd-4824-9680-0656617dd9f2)
  karena hasil copy project maka butuh penyesuaian nama package, ubah nama package yang awalnya dari pbo_upload menjadi pbo_persistence dan lakukan hal serupa pada kode didalamnya jika masih menggunakan package pbo_upload dan klik "Refactor"
  ![image](https://github.com/user-attachments/assets/dff67a81-cf9f-4278-aa6d-2e22acebda18)

- Selanjutnya hapus beberapa class yang sekarang sudah tidak dibutuhkan yakni class MataKuliah.java dan CRUDKu.java
  ![image](https://github.com/user-attachments/assets/fb9900f3-f080-4d6a-9a98-4c710dd878e7)
  ![image](https://github.com/user-attachments/assets/55422628-be32-48ff-9893-e453c7b703c3)
  lalu klik "Yes"

- Lalu buat Entity Classes from Database, disini pilih database yang telah kita buat sebelumnya, dan pilih database pada field Data Source dan pilih Add All Tables dan klik "Next" terus sampai muncul class baru pada directory (bukan package) dengan nama META-INF yang memiliki satu file bernama persistence.xml
  ![image](https://github.com/user-attachments/assets/6617e1c6-d590-4fdd-a70f-7192a9dd043d)
  ![image](https://github.com/user-attachments/assets/9ee29103-9cea-49c4-a955-992eaced69fd)
  ![image](https://github.com/user-attachments/assets/a1b28d8a-8404-49d8-8bcf-0b76badac78f)

- Selanjutnya lakukan add class jika Entity Classes belum masuk, karena disini sudah masuk maka lakukan langkah selanjutnya
  ![image](https://github.com/user-attachments/assets/9f718f8c-ea07-4a24-88d8-be4c91b12e97)

- Dan pastikan sekarang pada library anda ada library baru dengan 8 library ini
  ![image](https://github.com/user-attachments/assets/e7d69d6d-8da3-47a9-a519-00477f84c078)

- Lalu klik lagi class CRUDForm.java dan masuk bagian design, karena kita akan mengubah 3 tombol (Tambah, Update, dan Hapus) yang awalnya mengunakan kode SQL tersebut
  ![image](https://github.com/user-attachments/assets/355ee763-f728-4eb6-b84d-04da26602f79)
  ![image](https://github.com/user-attachments/assets/ce3c2f17-4e5f-43df-be3e-ec26e6bdf9ff)

- Klik 2x pada tombol Tambah dan Paste kode dibawah ini supaya serupa dengan gambar dibawah kode
  ```java
  EntityManagerFactory emf = Persistence.createEntityManagerFactory("PBO_PERSISTANCEPU");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();

        MataKuliah satu = new MataKuliah();

        satu.setKodemk(tfKodeMK.getText());
        satu.setSks(Integer.parseInt(tfSKS.getText()));
        satu.setNamamk(tfNamaMK.getText());
        satu.setSemesterajar(Integer.parseInt(tfSmtAjar.getText()));

        em.persist(satu);

        em.getTransaction().commit();
        JOptionPane.showMessageDialog(null, "Data dengan Kode MK : " + tfKodeMK.getText() + ", berhasil di input");
        em.close();
        emf.close();
        loadDataPU();
        tfKodeMK.setEditable(true);
  ```
  ![image](https://github.com/user-attachments/assets/dc608dc9-fc00-472b-ae4f-a45dc3ebf8ef)
  ![image](https://github.com/user-attachments/assets/28841b4e-489f-41bf-b9f6-1a9541dd4b9d)

- Klik 2x pada tombol Update dan Paste kode dibawah ini supaya serupa dengan gambar dibawah kode
  ```java
  String kode, nama;
        kode = tfKodeMK.getText();
        nama = tfNamaMK.getText();

        int sks;
        int smtAjar;

        try {
            sks = Integer.parseInt(tfSKS.getText());
            smtAjar = Integer.parseInt(tfSmtAjar.getText());
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "SKS, Semester ajar harus berupa angka dan tidak boleh kosong", "Error", JOptionPane.ERROR_MESSAGE);
            return;
        }
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("PBO_PERSISTANCEPU");
        EntityManager em = emf.createEntityManager();
        em.getTransaction().begin();

        MataKuliah mk = em.find(MataKuliah.class, kode);
        if (mk == null) {
            JOptionPane.showMessageDialog(null, "Data tidak ditemukan");
        } else {
            mk.setKodemk(kode);
            mk.setNamamk(nama);
            mk.setSks(sks);
            mk.setSemesterajar(smtAjar);

            em.getTransaction().commit();
            JOptionPane.showMessageDialog(null, "Data dengan Kode MK " + tfKodeMK.getText() + ", berhasil diupdate");

            em.close();
            emf.close();
            clearForm();
        }

        loadDataPU();
        tfKodeMK.setEditable(true);
  ```
  ![image](https://github.com/user-attachments/assets/c0ffcc81-0fda-44a5-82c8-1d06cacdacd2)
  ![image](https://github.com/user-attachments/assets/bd4063bd-9d4b-4717-a90d-4b250e758db3)
  ![image](https://github.com/user-attachments/assets/4b056047-ef15-4aff-9602-bf5f35b8348a)

- Klik 2x pada tombol Delete dan Paste kode dibawah ini supaya serupa dengan gambar dibawah kode
  ```java
  EntityManagerFactory emf = Persistence.createEntityManagerFactory("PBO_PERSISTANCEPU");
        EntityManager em = emf.createEntityManager();
        String kodeMK = tfKodeMK.getText();
        MataKuliah mataKuliah = em.find(MataKuliah.class, kodeMK);
        if (mataKuliah == null) {
            System.out.println("MataKuliah dengan kodemk: " + kodeMK + " tidak ada");
        } else {
            em.getTransaction().begin();
            em.remove(mataKuliah);
            em.getTransaction().commit();

            JOptionPane.showMessageDialog(this, "Hapus mata kuliah dengan Kode MK : " + tfKodeMK.getText() + ", berhasil dihapus");
            loadDataPU();
        }
        tfKodeMK.setEditable(true);
  ```
  ![image](https://github.com/user-attachments/assets/7dae4ce5-b523-4431-829e-27195c6f65f6)
  ![image](https://github.com/user-attachments/assets/56212112-e397-4e0b-ac92-ea22a4aaea65)

- Lalu lakukan Clean and Build pada Project yang telah dibuat, jika masih terdapat error pelajari kode yang telah saya bagikan di repository ini
  ![image](https://github.com/user-attachments/assets/771f69ad-646d-40f6-aeb5-7ad982a8193d)
- Dan jalankan programnya seperti project kemarin yang telah dibuat, tampilannya sama namun kode dari ketiga tombol tadi sudah berbeda listing kodenya
  ![image](https://github.com/user-attachments/assets/fb65c622-0687-4cf0-b1d8-87f2934e5a86)
