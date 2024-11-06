Tugas Implementasi Image Filtering Program ini membuat variabel Huntuk kernel/filter, Xuntuk input gambar, dan Yuntuk output gambar setelah dilakukan konvolusi.

Implementasi Program
import numpy as np
from scipy import ndimage
from PIL import Image

# 1. Buat variabel H (kernel/filter)
H = np.array([
    [0, -1, 0],
    [-1, 5, -1],
    [0, -1, 0]
])

# 2. Buat variabel X (gambar input)
def load_image_to_matrix(image_path):
    image = Image.open(image_path).convert("L")  # Ubah gambar ke skala abu-abu
    X = np.array(image)
    return X

# 3. Fungsi untuk konvolusi manual antara X dan H menghasilkan Y
def apply_convolution(X, H):
    # Ukuran gambar dan filter
    image_height, image_width = X.shape
    filter_height, filter_width = H.shape
    
    # Buat matriks Y (output) dengan ukuran yang sama dengan X
    Y = np.zeros_like(X)

    # Iterasi untuk setiap piksel dalam gambar
    for x in range(image_width - filter_width + 1):
        for y in range(image_height - filter_height + 1):
            # Hasil konvolusi untuk piksel (x, y)
            value = 0
            for k1 in range(filter_width):
                for k2 in range(filter_height):
                    value += H[k1, k2] * X[y + k2, x + k1]
            Y[y, x] = value

    # Normalisasi hasil agar berada dalam rentang 0-255
    Y = np.clip(Y, 0, 255)
    return Y

# Path gambar yang akan digunakan
image_path = "path/to/your/image.jpg"  # Ganti dengan path ke gambar Anda

# Proses gambar input
X = load_image_to_matrix(image_path)

# Terapkan konvolusi untuk mendapatkan output Y
Y = apply_convolution(X, H)

# Tampilkan dan simpan gambar hasil (output)
output_image = Image.fromarray(Y.astype(np.uint8))
output_image.show()  # Untuk melihat hasil
output_image.save("path/to/save/output_image.jpg")  # Simpan hasil

Penjelasan Kode

Variabel H(Kernel/Filter) : Hdidefinisikan sebagai matriks 3x3 untuk filter atau kernel. Contohnya di sini menggunakan penajaman kernel.

Variabel X(Input Gambar) : Fungsi load_image_to_matrix()memuat gambar dari file, mengubahnya menjadi skala abu-abu, dan mengubahnya menjadi matriks X.

Variabel Y(Output Gambar) : Fungsi apply_convolution()menerapkan konvolusi antara Xdan H, menyimpan hasilnya dalam matriks Y.

Normalisasi : Nilai di matriks Ydiklip ke dalam rentang 0-255 untuk memastikan hasilnya dapat diubah menjadi gambar.

Tampilkan dan Simpan Gambar Output : Gambar hasil konvolusi disimpan ke dalam file dan dapat dilihat.

Catatan

Gantilah path/to/your/image.jpgdengan path gambar input yang ingin Anda proses.
Gantilah path/to/save/output_image.jpgdengan path untuk menyimpan gambar keluaran.
