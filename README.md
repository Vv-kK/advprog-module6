### Milestone 1 Reflection Notes
Pada method handle_connection diawali dengan BufReader untuk membaca hasil response yang diberikan. BufReader digunakan karena lebih efisien dibandingkan Read instance pada umumnya. Selanjutnya ada ```lines()``` yang berfungsi membuat iterator untuk tiap line yang dibaca oleh instance BufReader. ```map()``` digunakan untuk mengubah iterator yang dihasilkan sebelumnya menjadi iterator baru. Iterator baru ini isinya adalah hasil dari ```result.unwrap()``` yang berupa string. Selanjutnya, ```take_while(|line| !line.is_empty())``` akan membuat iterator dari element-element di iterator sebelumnya dimana isinya adalah element yang memenuhi, dalam kasus ini tidak berupa string kosong. Terakhir, ```collect()``` membuat iterator tadi menjadi sebuah vector.

### Milestone 2 Reflection Notes
Pada kode tambahan method handle_connection pertama dinyatakan variabel status_line berisi string yang menandakan bahwa response memiliki status berhasil. Selanjutnya, dibuat variabel contents yang membaca isi file hello.html menjadi sebuah string dengan ```fs::read_to_string("hello.html)```. Variabel length dibuat untuk menyimpan panjang dari string contents yang dihitung menggunakan ```len()```. Lalu, semua variabel tadi diformat menjadi sebuah string yang ditampung dalam variabel response
![Commit 2 screen capture](images/commit2.png)

### Milestone 3 Reflection Notes
Disini ditambahkan kode untuk membedakan response. Hal ini dilakukan dengan cara mengecek line pertama dari response yang dikembalikan menggunakan ```next()``` dimana line pertama ini berisi request apa yang dilakukan. Unwrap berguna untuk menghandle Option dan menghasilkan Result yang di unwrap lagi untuk mendapatkan string. Selanjutnya, menggunakan if else dipisahkan response nya berdasarkan request user tadi. Jika user request 127.0.0.1:7878, maka akan dikembalikan page hello.html. Jika user request apapun selain itu (misalnya 127.0.0.1:7878/kylie), maka akan dikembalikan page 404.html yang tadi dibuat. Terakhir dilakukan refactoring karena banyak code pada if dan else block yang sama sehingga menjadi duplikat. Maka, bagian duplikat dikeluarkan dari if else block dan hanya bagian berbeda yang masuk ke dalam if else block agar tidak terjadi duplikasi kode.
![Commit 3 screen capture](images/commit3.png)

### Milestone 4 Reflection Notes
Kode baru yang ditambahkan melakukan request handling untuk endpoint /sleep dimana ketika ada yang mengirim request dengan endpoint ini, maka thread akan sleep atau berhenti selama 10 detik. Ketika ada request baru yang masuk, request tidak akan diproses karena hanya terdapat 1 thread dan thread itu masih diberhentikan, maka user harus menunggu thread itu berjalan seperti normal lagi agar bisa mendapatkan response. Cara untuk request handling disini juga diubah menjadi menggunakan match yang fungsinya untuk mencari string yang sama. Dalam kasus ini mencocokan string dengan request yang akan dihandle.

### Milestone 5 Reflection Notes
ThreadPool adalah kumpulan thread yang idle dan siap untuk execute task baru. Implementasi kode disini, tiap thread memiliki sebuah worker yang memiliki behavior untuk menunggu task dalam ThreadPool (dimana behavior ini tidak dimiliki oleh ```thread::spawn```). Terdapat queue berisi task yang perlu dilakukan oleh thread dan program akan memberikan task di queue ini kepada thread pada ThreadPool. Task disini ditampung dalam type Job dimana akan dikirimkan pada worker melalui channel. Jika thread sudah selesai, maka thread kembali ke dalam ThreadPool dan menunggu agar diberikan task lagi dari queue. Dengan ini, program dapat memproses N koneksi disaat bersamaan dimana N adalah jumlah thread yang ada di ThreadPool. Meski banyak thread sekilas terlihat lebih baik karena semakin banyak proses dapat dilakukan disaat bersamaan, tetapi beban server akan menjadi sangat berat dan rentan terhadap DoS attack dimana seseorang bisa mengirim 1 juta request ke server. Maka dari itu, jumlah thread harus dibatasi.