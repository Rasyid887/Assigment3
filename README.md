!pip install -U control   #Instal library control

import numpy as np    #Import library-library yang diperlukan (numpy, control, dan matplotlib)
import control as co
import matplotlib.pyplot as plt

def generateRouth(den):   #Membuat fungsi generateRouth dengan parapemer den. Fungsi ini berfungsi untuk membuat tabel routh
  r = [[],[]]   #Membuat variabel r sebagai array untuk tabel routh
  for i in range(len(den)):   #Mengisi array r dengan nilai dari den sesuai ketentuan tabel routh
    r[i%2].append(den[i])

  i = 0   #Mengembalikan nilai i menjadi 0
  while True:   #Pengulangan ini berfungsi mengisi nilai b1, b2, c1, dst
    if len(r[i])>len(r[i+1]) and r[i][len(r[i])-1]!=0:    #Mengecek apakah kolom paling kanan pada tabel routh terisi dan tidak 0
      r[i+1].append(0)    #Menambah nilai 0 pada baris selanjutnya kolom terakhir pada tabel routh
    if r[i][len(r[i])-1] == 0:    #Mengecek apakah kolom paling kanan pada tabel routh bernilai 0
      r[i].pop()    #Menghapus nilai 0 pada kolom paling kanan
    if len(r[i]) > 1:   #Mengecek apakah panjang baris > 1
      r.append([])    #Menambah baris
    if r[i+1][0] == 0:    #Mengecek apakah kolom paling kiri kecuali baris 1 ada yang bernilai 0 atau tidak
      den = [a+b for a, b in zip(den+[0], [0]+den)]   #Antisipasi jika ada kolom paling kiri yang bernilai 0
      return generateRouth(den)
    
    for j in range(len(r[i])-1):    #Mengisi nilai dari b1, b2, c1, dst
      r[i+2].append((r[i+1][0]*r[i][j+1] - r[i][0]*r[i+1][j+1])/r[i+1][0])
    
    if len(r[i]) == 1:    #Mengecek apakah panjang baris = 0
      break   #keluar dari pengulangan
    
    i += 1    #i = i+1
  return r    #Mengembalikan nilai r
  
  
def cekKestabilan(den):   #Membuat fungsi cekKestabilan dengan parameter den. Fungsi ini berfungsi untuk mengetahui sistem stabil atau tidak stabil
  if den[len(den)-2] == 0 and den[len(den)-1] == 0:   #Mengecek sistem stabil atau tidak berdasarkan beberapa kutub pada asalnya
    print("SISTEM TIDAK STABIL !")    #Menampilkan ke layar "SISTEM TIDAK STABIL"
    return False
  
  flag = 0    #Inisialisasi variabel flag = 0
  row = generateRouth(den)    #Memanggil fungsi generateRouth
  print("Tabel routh\n")    #Menampilkan ke layar "Tabel routh"
  for i in range (len(row)):    #Menampilkan ke layar tabel routh
    print(row[i])
  print(" ")
 
  for i in range(len(row)):   #Mengecek apakah ada nilai dikolom paling kiri yang bernilai negatif
    if row[i][0] < 0:
      flag = 1
  
  if flag == 0:   #Jika kolom paling kiri semua bernilai positif maka sistem stabil, jika ada yang negatif maka sistem tidak stabil
    print("\nSISTEM STABIL !")    #Menampilkan ke layar "SISTEM STABIL !"
    return True
  else :
    print("\nSISTEM TIDAK STABIL !")    ##Menampilkan ke layar "SISTEM TIDAK STABIL !"
    return False
    

print("Silahkan masukan nilai K!")    #Menampilkan ke layar "Silahkan masukan nilai K"
k = int(input())    #Menerima input dari user
print(" ")    #Membuat baris kosong
num = [k]   #Inisialisasi variabel num
den = [1,3,3,2,k]   #Inisialisasi variabel den
print("Tranfer Function")   #Menampilkan ke layar "Tranfer Function"
print(co.tf(num,den))   #Menampilkan ke layar bentuk tranfer fungsi
print(" ")    #Membuat baris kosong
print("Bentuk polinomial")    #Menampilkan ke layar "Bentuk polinomial"
print(" ")    #Membuat baris kosong
print(" ")    #Membuat baris kosong
print(np.poly1d(den, variable = 's'))   #Menampilkan ke layar bentuk polinomial
print(" \n")    #Membuat baris kosong

result = cekKestabilan(den)   #Memanggil fungsi cekKestabilan
