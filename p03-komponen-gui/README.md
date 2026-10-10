# Penggunaan AI

## Nama AI

**Google Gemini**

AI digunakan sebagai media pendukung selama proses pembuatan aplikasi `FormTiketTravel` menggunakan Java Swing di NetBeans. Pemanfaatan AI difokuskan pada pengembangan fungsi pemesanan tiket, validasi input pengguna, pengaturan tampilan aplikasi menggunakan FlatLaf, serta membantu menganalisis kesalahan yang muncul selama proses pengembangan.

---

## Prompt 1 – Membuat Validasi Form Pemesanan Tiket

### Prompt

> "Saya sedang mengembangkan aplikasi pemesanan tiket travel bernama FormTiketTravel menggunakan Java Swing di NetBeans. Saya ingin menambahkan validasi sebelum data diproses ketika tombol Pesan ditekan. Pastikan nama dan nomor HP tidak kosong, nomor HP hanya berisi angka, serta pengguna harus memilih tujuan dan kelas tiket. Jika terdapat input yang tidak valid, tampilkan pesan peringatan menggunakan JOptionPane. Berikan contoh kode Java yang mudah dipahami dan dapat disesuaikan dengan komponen pada form saya."

### Hasil Jawaban AI

Gemini menjelaskan bahwa validasi perlu dilakukan sebelum data pemesanan diproses. Pemeriksaan dapat dilakukan dengan mengambil nilai dari komponen input, memeriksa apakah kolom wajib telah diisi, serta memastikan nomor HP hanya mengandung angka.

Contoh implementasi:

```java
private boolean validasiForm() {
    String nama = namaPemesanField.getText().trim();
    String hp = nomorHpField.getText().trim();

    if (nama.isEmpty()) {
        JOptionPane.showMessageDialog(
            this,
            "Nama pemesan harus diisi!",
            "Peringatan",
            JOptionPane.WARNING_MESSAGE
        );
        namaPemesanField.requestFocus();
        return false;
    }

    if (hp.isEmpty() || !hp.matches("\\d+")) {
        JOptionPane.showMessageDialog(
            this,
            "Nomor HP wajib diisi dan hanya boleh berisi angka!",
            "Peringatan",
            JOptionPane.WARNING_MESSAGE
        );
        nomorHpField.requestFocus();
        return false;
    }

    if (kotaTujuanCombo.getSelectedIndex() <= 0) {
        JOptionPane.showMessageDialog(
            this,
            "Silakan pilih tujuan perjalanan!",
            "Peringatan",
            JOptionPane.WARNING_MESSAGE
        );
        kotaTujuanCombo.requestFocus();
        return false;
    }

    if (!ekonomiRadio.isSelected()
            && !bisnisRadio.isSelected()
            && !eksekutifRadio.isSelected()) {
        JOptionPane.showMessageDialog(
            this,
            "Silakan pilih kelas tiket!",
            "Peringatan",
            JOptionPane.WARNING_MESSAGE
        );
        return false;
    }

    return true;
}
```

Fungsi validasi tersebut dapat dipanggil pada event tombol Pesan:

```java
private void pesanButtonActionPerformed(
        java.awt.event.ActionEvent evt) {

    if (!validasiForm()) {
        return;
    }

    prosesPemesanan();
}
```

Melalui pendekatan ini, program hanya melanjutkan pemrosesan pemesanan ketika data wajib sudah memenuhi ketentuan. Pemeriksaan indeks tujuan juga perlu disesuaikan dengan isi `JComboBox` yang digunakan pada aplikasi.

---

## Prompt 2 – Menghitung Total Harga Tiket Berdasarkan Kelas dan Fasilitas

### Prompt

> "Bantu saya mengembangkan fitur perhitungan harga pada aplikasi FormTiketTravel menggunakan Java Swing. Saya memiliki pilihan kelas Ekonomi, Bisnis, dan Eksekutif menggunakan JRadioButton, serta pilihan fasilitas tambahan menggunakan JCheckBox, seperti bagasi, makan, dan asuransi. Buatkan contoh logika untuk menghitung total harga berdasarkan kelas yang dipilih dan menambahkan biaya fasilitas yang dicentang. Setelah tombol Pesan ditekan, tampilkan rincian harga dan total pembayaran menggunakan JOptionPane. Gunakan struktur kode yang sederhana dan mudah dipelajari."

### Hasil Jawaban AI

Gemini menyarankan agar harga tiket setiap kelas dan biaya fasilitas ditentukan dalam variabel terpisah. Program kemudian memeriksa kelas tiket yang dipilih dan menambahkan biaya fasilitas sesuai dengan pilihan pengguna.

Contoh kode:

```java
private void hitungHarga() {
    int hargaTiket = 0;
    int biayaFasilitas = 0;

    String kelas = "";

    if (ekonomiRadio.isSelected()) {
        kelas = "Ekonomi";
        hargaTiket = 100000;
    } else if (bisnisRadio.isSelected()) {
        kelas = "Bisnis";
        hargaTiket = 175000;
    } else if (eksekutifRadio.isSelected()) {
        kelas = "Eksekutif";
        hargaTiket = 250000;
    }

    if (bagasiCheck.isSelected()) {
        biayaFasilitas += 10000;
    }

    if (makanCheck.isSelected()) {
        biayaFasilitas += 15000;
    }

    if (asuransiCheck.isSelected()) {
        biayaFasilitas += 5000;
    }

    int totalHarga = hargaTiket + biayaFasilitas;

    String hasil =
        "RINCIAN PEMESANAN TIKET\n"
        + "----------------------------\n"
        + "Kelas Tiket : " + kelas
        + "\nHarga Tiket : Rp" + hargaTiket
        + "\nBiaya Fasilitas : Rp" + biayaFasilitas
        + "\nTotal Pembayaran : Rp" + totalHarga;

    JOptionPane.showMessageDialog(
        this,
        hasil,
        "Rincian Harga Tiket",
        JOptionPane.INFORMATION_MESSAGE
    );
}
```

Kode tersebut memberikan gambaran penerapan logika perhitungan harga berdasarkan pilihan pengguna. Nilai harga pada contoh hanya berupa ilustrasi dan perlu disesuaikan dengan ketentuan harga yang ditetapkan dalam aplikasi.

Dengan adanya fitur ini, aplikasi tidak hanya menerima data pemesanan, tetapi juga dapat memberikan informasi mengenai rincian biaya yang harus dibayar oleh pengguna.

---

## Prompt 3 – Memperbaiki Tampilan Form Menggunakan FlatLaf

### Prompt

> "Saya ingin memperbaiki tampilan aplikasi FormTiketTravel di Java Swing agar terlihat lebih modern dan konsisten menggunakan FlatLaf. Jelaskan cara menerapkan tema FlatLightLaf dan FlatDarkLaf, mengatur tampilan tombol dan komponen input, serta memastikan perubahan tema tidak merusak layout yang dibuat melalui NetBeans GUI Builder. Berikan contoh kode yang sesuai untuk aplikasi Java Swing dan jelaskan bagian kode yang perlu ditempatkan di dalam project."

### Hasil Jawaban AI

Gemini menjelaskan bahwa FlatLaf merupakan library Look and Feel yang dapat memperbarui tampilan komponen Java Swing tanpa harus membangun ulang seluruh layout aplikasi.

Tema dapat diterapkan sebelum form ditampilkan. Sebagai contoh, untuk menggunakan tema terang:

```java
import com.formdev.flatlaf.FlatLightLaf;

public static void main(String[] args) {
    FlatLightLaf.setup();

    java.awt.EventQueue.invokeLater(() -> {
        new FormTiketTravel().setVisible(true);
    });
}
```

Untuk memberikan tampilan tombol yang lebih konsisten, beberapa properti komponen juga dapat diatur melalui kode:

```java
pesanButton.putClientProperty(
    "JButton.buttonType",
    "roundRect"
);

pesanButton.setText("Pesan Tiket");

pesanButton.setFocusPainted(false);
```

Gemini juga menjelaskan bahwa pengaturan tema sebaiknya dilakukan secara terstruktur agar tidak mengganggu komponen yang sudah dibuat melalui GUI Builder. Jika ingin menyediakan pilihan tema terang dan gelap, pergantian Look and Feel dapat dilakukan melalui event tombol toggle, kemudian tampilan komponen diperbarui.

Untuk perubahan tema saat aplikasi sedang berjalan, implementasi perlu memperhatikan pembaruan seluruh komponen, termasuk jendela utama dan dialog yang masih terbuka. Selain itu, library FlatLaf harus sudah ditambahkan ke project agar kelas-kelas yang digunakan dapat dikenali oleh NetBeans.

---

# Alasan Menggunakan AI

Google Gemini digunakan sebagai alat bantu dalam proses pengembangan aplikasi `FormTiketTravel`, khususnya untuk memahami penerapan logika pemrograman Java Swing dan meningkatkan kualitas fungsi aplikasi.

Pemanfaatannya meliputi penyusunan validasi data pemesanan, perhitungan harga tiket berdasarkan kelas dan fasilitas, serta pengembangan tampilan antarmuka menggunakan FlatLaf. Gemini juga membantu memberikan penjelasan mengenai fungsi komponen Java Swing, seperti `JTextField`, `JComboBox`, `JRadioButton`, `JCheckBox`, dan `JOptionPane`.

Contoh kode yang diberikan AI digunakan sebagai referensi untuk memahami alur program. Penyesuaian tetap diperlukan berdasarkan nama komponen, struktur source code, dan kebutuhan aplikasi yang dikembangkan di NetBeans. Pengujian juga perlu dilakukan untuk memastikan setiap fitur berjalan sesuai harapan.

Sementara itu, penyusunan layout dan penempatan komponen antarmuka tetap dilakukan melalui **NetBeans GUI Builder**. Dengan demikian, Google Gemini berperan sebagai pendamping pembelajaran yang membantu memahami konsep pemrograman, mencari alternatif solusi, dan meningkatkan kemampuan dalam mengembangkan aplikasi Java Swing.