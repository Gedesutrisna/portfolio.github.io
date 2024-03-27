```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>

// Struktur untuk menyimpan informasi barang
struct Barang {
    int kode;
    char nama[50];
    float harga;
};

// Fungsi urutkan barang
void urutkanProduk(int jumlah_barang[], int total_barang, struct Barang barang[]) {
    // Bubble sort untuk mengurutkan indeks barang berdasarkan jumlahnya
    for (int i = 0; i < total_barang - 1; i++) {
        for (int j = 0; j < total_barang - i - 1; j++) {
            if (jumlah_barang[j] < jumlah_barang[j + 1]) {
                // Tukar indeks barang
                int temp = jumlah_barang[j];
                jumlah_barang[j] = jumlah_barang[j + 1];
                jumlah_barang[j + 1] = temp;

                // Tukar juga barangnya agar sesuai dengan jumlah barang
                struct Barang tempBarang = barang[j];
                barang[j] = barang[j + 1];
                barang[j + 1] = tempBarang;
            }
        }
    }
}

// Fungsi untuk menampilkan daftar barang yang tersedia
void tampilkanDaftarBarang(struct Barang barang[], int total_barang) {
    printf("                                SELAMAT DATANG DI                                 \n");
    printf("                                   TOKO SKENSA                                    \n");
    printf("==================================================================================\n");
    printf("                    Silahkan Pilih Barang yang Anda Inginkan :                    \n");
    printf("==================================================================================\n\n");
    printf("|No|Barang \t\t|Harga      |\n");
    printf("-------------------------------------\n");
    for (int i = 0; i < total_barang; i++) {
        printf("%d\t%s\t\t%.2f\n", barang[i].kode, barang[i].nama, barang[i].harga);
    }
    printf("==================================================================================\n\n");
    printf("99. Struk Pembayaran\n");
    printf("55. Reset Pilihan\n");
    printf("00. Keluar Aplikasi\n\n");
}

// Fungsi untuk melakukan reset jumlah barang yang dibeli
void resetPilihan(int jumlah_barang[], float *total_belanja, int total_barang) {
    for (int i = 0; i < total_barang; i++) {
        jumlah_barang[i] = 0;
    }
    *total_belanja = 0;
    printf("Semua barang telah direset.\n");
}

// Fungsi untuk menampilkan struk pembayaran
void tampilkanStruk(struct Barang barang[], int jumlah_barang[], char nama_pembeli[], int total_barang) {
    // Panggil fungsi urutkanProduk() untuk mengurutkan daftar barang
    urutkanProduk(jumlah_barang, total_barang, barang);

    // Deklarasi variable Char / String
    char ZonaWaktu[80];
    char namafile[50];

    // Deklarasi Variabel Fungsi Waktu
    time_t sekarang;
    time(&sekarang);
    struct tm *waktu_lokal = localtime(&sekarang);
    strftime(ZonaWaktu, 80, "%a %b %d %H:%M:%S %Y", waktu_lokal); // Deklarasi Set Time untuk Tampilan

    // Menyimpan ID Struk sebelum mencetak struk ke konsol
    char IDStrukConsole[50];
    sprintf(IDStrukConsole, "%ld", sekarang);


    printf("\n\n                              REKAP PESANAN BARANG                                    \n");
    printf("==================================================================================\n");
    printf("|No |Nama Barang        |Harga        |Jumlah       |Total Harga\t|Diskon         |                  \n");
    printf("==================================================================================\n\n");
    float total_diskon = 0;
    float total_harga = 0;


    for (int i = 0; i < total_barang; i++) {
        if (jumlah_barang[i] > 0) {
            float subtotal = barang[i].harga * jumlah_barang[i];
            float diskon = 0;
            if (jumlah_barang[i] > 3 && jumlah_barang[i] <= 5) {
                diskon = subtotal * 0.10; // Diskon 10% jika jumlah produk antara 4 dan 5
            } else if (jumlah_barang[i] > 5) {
                diskon = subtotal * 0.15; // Diskon 15% jika jumlah produk melebihi 5
            }
            total_diskon += diskon;
            total_harga += subtotal;
            printf("%-2d | %-15s | %-13.2f | %-10d | %-11.2f | %-10.2f\n", i + 1, barang[i].nama, barang[i].harga, jumlah_barang[i], subtotal, diskon);
        }
    }
    printf("==================================================================================\n\n");
    float total_bayar = total_harga - total_diskon;
    printf("Total Harga   : %.2f\n", total_harga);
    printf("Total diskon  : %.2f\n", total_diskon);
    printf("Total Bayar  : %.2f\n", total_bayar);

    float uang_dibayarkan;

    do {
        printf("\nMasukkan uang yang dibayarkan: ");
        scanf("%f", &uang_dibayarkan);
        if (uang_dibayarkan < total_bayar) {
            printf("Uang yang dibayarkan kurang dari total harga. Mohon masukkan kembali.\n");
        }
    } while (uang_dibayarkan < total_bayar);

    float kembalian = uang_dibayarkan - total_bayar;
    printf("Uang kembalian          : %.2f\n", kembalian);

    // Menampilkan struk belanja
    printf("\n==================================================================================\n\n");
    printf("\n==================================================================================\n");
    printf("|                                   TOKO SKENSA                                  |\n");
    printf("|                          JALAN HOS COKROAMINOTO NO. 84                         |\n");
    printf("|                                  DENPASAR, BALI                                |\n");
    printf("|                                  TELP. 081628579                               |\n");
    printf("|                                                                                |\n");
    printf("|ID Struk : %s                                                           |\n", IDStrukConsole);
    printf("==================================================================================\n");
    printf("|No| Nama Produk    | Harga Satuan  | Jumlah     | Total Harga | Diskon    \n");
    printf("==================================================================================\n\n");
    for (int i = 0; i < total_barang; i++) {
        if (jumlah_barang[i] > 0) {
            float subtotal = barang[i].harga * jumlah_barang[i];
            float diskon = 0;
            if (jumlah_barang[i] > 3 && jumlah_barang[i] <= 5) {
                diskon = subtotal * 0.10; // Diskon 10% jika jumlah produk antara 4 dan 5
            } else if (jumlah_barang[i] > 5) {
                diskon = subtotal * 0.15; // Diskon 15% jika jumlah produk melebihi 5
            }
            printf("%-2d | %-15s | %-13.2f | %-10d | %-11.2f | %-10.2f\n", i + 1, barang[i].nama, barang[i].harga, jumlah_barang[i], subtotal, diskon);
        }
    }


    printf("==================================================================================\n");
    printf("|                                                                                |\n");
    printf("|Total Harga \t:Rp. %.2f,-                                                     |\n", total_harga);
    printf("|Total Diskon \t:Rp. %.2f,-                                                      |\n", total_diskon);
    printf("|Total Bayar \t:Rp. %.2f,-                                                     |\n", total_bayar);
    printf("|Pembayaran \t:Rp. %.2f,-                                                     |\n", uang_dibayarkan);
    printf("|Kembalian \t:Rp. %.2f,-                                                      |\n", kembalian);
    printf("|                                                                                |\n");
    printf("|Waktu / Hari : %s                                         |\n", ZonaWaktu);
    printf("|                                                                                |\n");
    printf("==================================================================================\n");

    // Simpan struk belanja ke dalam file teks
    char filename[50];
    sprintf(filename, "struk_%s.txt", nama_pembeli);
    FILE *file;
    file = fopen(filename, "w");
    if (file != NULL) {
        fprintf(file, "\n\t\t==================================================================================\n");
        fprintf(file, "\t\t|                                   TOKO SKENSA                                  |\n");
        fprintf(file, "\t\t|                          JALAN HOS COKROAMINOTO NO. 84                         |\n");
        fprintf(file, "\t\t|                                  DENPASAR, BALI                                |\n");
        fprintf(file, "\t\t|                                  TELP. 081628579                               |\n");
        fprintf(file, "\t\t|                                                                                |\n");
        fprintf(file, "\t\t|ID Struk : %s                                                           |\n", nama_pembeli);
        fprintf(file, "\t\t==================================================================================\n");
        fprintf(file, "\t\t|No| Nama Produk    | Harga Satuan  | Jumlah     | Total Harga | Diskon    \n");
        fprintf(file, "\t\t==================================================================================\n\n");
        for (int i = 0; i < total_barang; i++) {
            if (jumlah_barang[i] > 0) {
                float subtotal = barang[i].harga * jumlah_barang[i];
                float diskon = 0;
                if (jumlah_barang[i] > 3 && jumlah_barang[i] <= 5) {
                    diskon = subtotal * 0.10; // Diskon 10% jika jumlah produk antara 4 dan 5
                } else if (jumlah_barang[i] > 5) {
                    diskon = subtotal * 0.15; // Diskon 15% jika jumlah produk melebihi 5
                }
                fprintf(file, "\t\t%-2d | %-15s | %-13.2f | %-10d | %-11.2f | %-10.2f\n", i + 1, barang[i].nama, barang[i].harga, jumlah_barang[i], subtotal, diskon);
            }
        }
        fprintf(file, "\t\t==================================================================================\n");
        fprintf(file, "\t\t|                                                                                |\n");
        fprintf(file, "\t\t|Total Harga \t:Rp. %.2f,-                                                     |", total_harga);
        fprintf(file, "\n\t\t|Total Diskon \t:Rp. %.2f,-                                                      |", total_diskon);
        fprintf(file, "\n\t\t|Total Bayar \t:Rp. %.2f,-                                                     |", total_bayar);
        fprintf(file, "\n\t\t|Pembayaran \t:Rp. %.2f,-                                                     |", uang_dibayarkan);
        fprintf(file, "\n\t\t|Kembalian \t:Rp. %.2f,-                                                      |\n", kembalian);
        fprintf(file, "\t\t|                                                                                |\n");
        fprintf(file, "\t\t==================================================================================\n");
        fclose(file);
        printf("\nStruk telah berhasil dicetak dalam file '%s'.\n", filename);
    } else {
        printf("Gagal menyimpan struk belanja ke dalam file.\n");
    }
}

int main() {
    // Menyiapkan data barang
    struct Barang barang[] = {
        {1, "Sabun", 5000},
        {2, "Shampoo", 10000},
        {3, "Odol", 7000},
        {4, "Sandal", 3000},
        {5, "Toys", 80000},
        // Tambahkan barang lain di sini
    };
    int jumlah_barang[sizeof(barang) / sizeof(barang[0])]; // array untuk menyimpan jumlah barang yang dibeli
    int total_barang = sizeof(barang) / sizeof(barang[0]); // total barang yang tersedia
    float total_belanja = 0; // total harga belanja
    int i;

    // Inisialisasi jumlah barang menjadi 0
    for (i = 0; i < total_barang; i++) {
        jumlah_barang[i] = 0;
    }

    char nama_pembeli[100]; // menyimpan nama pembeli

    // Meminta nama pembeli
    printf("Masukkan nama pembeli: ");
    fgets(nama_pembeli, sizeof(nama_pembeli), stdin);
    strtok(nama_pembeli, "\n"); // menghapus karakter newline dari nama pembeli

    // Menampilkan daftar barang yang tersedia
    tampilkanDaftarBarang(barang, total_barang);

    // Memasukkan jumlah barang yang akan dibeli
    printf("\n==================================================================================\n\n");
    do {
        int kode;
        printf("Input Pilihan yang Anda Inginkan : ");
        scanf("%d", &kode);
        if (kode == 0) {
            break;
        } else if (kode == 55) {
            // Reset jumlah barang yang dibeli dan barang yang dipilih sebelumnya
            resetPilihan(jumlah_barang, &total_belanja, total_barang);
            continue; // Melanjutkan iterasi tanpa memproses input selanjutnya
        } else if (kode == 99) {
            // Menampilkan struk belanja
            tampilkanStruk(barang, jumlah_barang, nama_pembeli, total_barang);
            break;
        }
        // Mencari barang sesuai dengan kode yang dimasukkan
        for (i = 0; i < total_barang; i++) {
            if (barang[i].kode == kode) {
                printf("Jumlah barang (%s): ", barang[i].nama); // Menampilkan nama barang
                break;
            }
        }
        if (i == total_barang) {
            printf("Kode barang tidak valid.\n");
            continue;
        }

        int jumlah;
        scanf("%d", &jumlah);
        jumlah_barang[i] += jumlah;
        total_belanja += barang[i].harga * jumlah;

    } while (1);

    return 0;
}
