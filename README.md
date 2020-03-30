# Kecerdasan Buatan F

05111840000162 - Fransiskus Xaverius Kevin Koesnadi

Departemen Teknik Informatika - FTEIC ITS

## Daftar Isi
1. [Tic Tac Toe](https://github.com/FXKevinK/KB-F_05111840000162/blob/master/README.md#1-tic-tac-toe---algoritma-minimax)
2. [4 Queen (CSP)](https://github.com/FXKevinK/KB-F_05111840000162/blob/master/README.md#2-4-queen---csp-constraint-satisfaction-problems)

## 1. Tic Tac Toe - Algoritma Minimax

[Source Code](https://github.com/FXKevinK/KB-F_05111840000162/tree/master/Tic%20Tac%20Toe) Tic Tac Toe

Tic-tac-toe adalah permainan kertas dan pensil yang dimainkan oleh dua pemain, masing-masing menuliskan tanda X atau O, yang bergiliran menandai ruang-ruang dalam kotak 3 × 3 . Pemain yang berhasil menempatkan tiga tanda mereka di baris horizontal, vertikal, atau diagonal adalah pemenangnya serta setiap pemain diharuskan memakasimalkan kemungkinan untuk memenangkan permainan dan meminimalkan kemungkinan bagi lawan pemain.

Penjelasan:

Fungsi berikut digunakan untuk memeriksa ketika pemain telah memberi tanda berupa pada papan tic tac toe atau langkah tanda yang diberikan pemain sudah memiliki tanda yang saling terhubung.
```C
int win (const int board[9]){
    //determines if a player has won, returns 0 otherwise.
    unsigned wins[8][3] = {{0,1,2},{3,4,5},{6,7,8},{0,3,6},
    {1,4,7},{2,5,8},{0,4,8},{2,4,6}};
    int i;
    for(i = 0; i < 8; ++i) {
        if(board[wins[i][0]] != 0 &&
           board[wins[i][0]] == board[wins[i][1]] &&
           board[wins[i][0]] == board[wins[i][2]])
            return board[wins[i][2]];
    }
    return 0;
}
```
Fungsi berikut merupakan implemetasi algoritma minimax. Algoritma minimax memiliki konsep dasar ketika setiap *child node* akan dicari nilainya, kemudian dicari yang terkecil masing-masing untuk dijadikan nilai pada setiap *child node*. Selanjutnya dicari nilai terbesar dari nilai *child node* tersebut untuk menentukan langkah pemain pertama. Setelah langkah pemain pertma usai, akan diterapkan fungsi algoritma yang sama pada lawan pemain.
```C
int minimax(int board[9], int player){
    //How is the position like for player (their turn) on board?
    int winner = win(board);
    if (winner != 0){
    	return winner*player;	
	}
    int move = -1, i;
    int score = -2;//Losing moves are preferred to no move
    for (i = 0; i < 9; ++i) {//For all moves,
        if (board[i] == 0) {//If legal,
            board[i] = player;//Try the move
            int thisScore = -minimax(board, player*-1);
            if (thisScore > score) {
                score = thisScore;
                move = i;
            }//Pick the one that's worst for the opponent
            board[i] = 0;//Reset board after try
        }
    }
    if (move == -1){
    	return 0;	
	}
    return score;
}
```

Minimax adalah semacam algoritma backtracking yang digunakan dalam pengambilan keputusan dan teori permainan untuk menemukan langkah optimal bagi pemain, dengan asumsi bahwa lawan pemain juga bermain secara optimal. Metode minimax banyak digunakan dalam dua permainan berbasis giliran pemain seperti Tic-Tac-Toe, Backgammon, Mancala, Catur, dll.

Di Minimax kedua pemain disebut maximizer dan minimizer. Maximizer mencoba untuk mendapatkan skor setinggi mungkin sementara minimizer mencoba melakukan yang sebaliknya dan mendapatkan skor serendah mungkin.

Setiap state memiliki nilai yang terkait dengannya. Dalam keadaan tertentu jika maximizer lebih unggul, skor pada papan akan cenderung bernilai positif. Jika minimizer, maka akan cenderung menjadi nilai negatif. Nilai-nilai pada papan dihitung oleh beberapa heuristik yang unik untuk setiap jenis permainan.

Algoritma Minimax dapat didefinisikan sebagai fungsi rekursif yang melakukan hal-hal berikut:
1. Return nilai jika kondisi terminal ditemukan (+10, 0, -10)
2. Melihat kondisi tempat-tempat yang tersedia di papan tulis
3. Memanggil fungsi minimax di setiap tempat yang tersedia (rekursi)
4. Mengevaluasi nilai yang di-return dari panggilan fungsi dan return nilai terbaik

Dalam fungsi playerMove, lihat itu sambil mengulang sekali lagi.
```C
while (move> = 9 || move <0 && board [move] == 0);
```

Namun jika dilihat lebih lanjut, fungsi berikut akan lebih baik.
```C
while (move> = 9 || move <0 || board [move]! = 0);
```

Di loop sementara pertama akan diberikan dua kondisi:

* Jika perpindahan lebih besar dari ATAU SAMA dengan 9, bernilai tidak valid.
* Jika langkah kurang dari 0 dan ruang yang dipilih adalah 0 atau kondisi state pada papan kosong, juga bernilai tidak valid

Meskipun sedikit membingungkan, pengguna harus dapat menempatkan sesuatu di sana. Berikut adalah kondisi yang menyatakan while:

* Jika perpindahan lebih besar dari atau sama dengan 9, bernilai tidak valid.
* Jika langkah kurang dari 0, bernilai tidak valid
* Jika perpindahan tidak sama dengan 0 (artinya tidak dihuni), bernilai tidak valid.

Contoh Implementasi

Anggaplah ada 2 cara yang memungkinkan bagi X untuk memenangkan permainan dari status dewan pemberi.

    Langkah A: X dapat menang dalam 2 langkah
    Langkah B: X dapat menang dalam 4 gerakan

Meskipun langkah A lebih baik karena memastikan kemenangan yang lebih cepat, algoritma yang diberntuk akan lebih memilih B. Untuk mengatasi masalah ini, dapat dikurangi nilai kedalaman dari skor yang dievaluasi (dalam hal ini dimasukkan contoh return 10). Ini berarti bahwa jika menang, ia akan memilih kemenangan yang paling sedikit mengambil langkah dan jika kalah ia akan mencoba memperpanjang permainan dan memainkan sebanyak mungkin gerakan. Jadi nilai yang dievaluasi baru akan

    Bergerak A akan memiliki nilai +10 - 2 = 8
    Bergerak B akan memiliki nilai +10 - 4 = 6

Sekarang karena langkah A memiliki skor yang lebih tinggi dibandingkan dengan langkah B, algoritma akan memilih langkah A di atas langkah B. Hal yang sama harus diterapkan pada minimizer. Alih-alih mengurangi kedalaman, ditambahkan nilai kedalaman karena minimizer selalu mencoba untuk mendapatkan nilai senegatif mungkin.

Ilistrasi implementasi algoritma minimax:

![tttminimax](https://user-images.githubusercontent.com/58078219/77514002-944f2f80-6ea8-11ea-899f-9f04e084210f.png)

Kembali ke: [Daftar Isi](#daftar-isi)

## 2. 4 Queen - CSP (Constraint Satisfaction Problems)

[Source Code](https://github.com/FXKevinK/KB-F_05111840000162/tree/master/4%20Queen) 4 Queen

4 Queen problem merupakan suatu permasalahan untuk menempatkan 4 ratu catur pada papan catur 4 × 4 sehingga tidak ada ratu yang saling mengancam. Dengan demikian, sebuah solusi mensyaratkan bahwa tidak ada ratu berbagi baris, kolom, atau diagonal yang sama. Pendekatan yang diperlukan yaitu dengan menempatkan ratu satu per satu di kolom yang berbeda, mulai dari kolom paling kiri. Ketika ratu dalam sebuah kolom, maka diperiksa terlebih dahulu apakah terdapat bentrokan dengan ratu yang sudah ditempatkan. Di kolom saat ini, jika baris yang ditempati tidak ada bentrokan, maka baris dan kolom tersebut ditandai sebagai bagian dari solusi. Jika tidak ditemukan baris seperti baris yang dimaksudkan karena adanya bentrokan, maka akan dilakukan algoritma backtracking dan me-return `false`.

Pada dasarnya, harus memastikan 4 hal berikut:
1. Tidak ada queen yang berbagi kolom.
2. Tidak ada queen yang berbagi baris.
3. Tidak ada queen berbagi diagonal kanan atas ke kiri bawah.
4. Tidak ada queen berbagi diagonal kiri atas ke kanan bawah.

Penjelasan:

Program 4Queen berikut memiliki bebrapa fungsi dalam menjalankan algoritmanya.

Fungsi berikut berfungsi sebagai mencari penempatan pada setiap area persegi papan catur 4 × 4 untuk setiap queen.
```C
// Function to check queens placement 
void nQueens (int k, int n){ 

	for (int i = 1;i <= n;i++){ 
		if (canPlace(k, i)){ 
			arr[k] = i; 
			if (k == n){
				display(n); 	
			}
			else{
				nQueens(k + 1, n); 
			}
		} 
	} 
} 
```
Berikutnya terdapat fungsi pemeriksaan area persegi yang ditempati oleh queen tersebut. Hal ini dimaksudkan sehingga tidak terjadi bentrokan antar queen yang ditempati.
```C
// Helper Function to check if queen can be placed 
bool canPlace (int k, int i){ 
	for (int j = 1;j <= k - 1;j++){ 
		if (arr[j] == i || 
			(abs(arr[j] - i) == abs(j - k))){
				return false; 	
			}
	} 
	return true; 
} 
```

Setelahnya terdapat satu fungsi lagi untuk memastikan bahwa queen memiiki tempat yang sesuai untuk ditempati setelah menjalankan kedua fungsi di atas dan ditampilkan penempatannya.
```C
// Function to display placed queen 
void display (int n){ 
	cout << "\n---------------------------------\n";
	cout << "Arrangement No. " << ++no; 
	cout << "\n---------------------------------\n"; 

	for (int i = 1; i <= n; i++){ 
		for (int j = 1; j <= n; j++){ 
			if (arr[i] != j) 
				cout << "\t_"; 
			else
				cout << "\tQ"; 
		} 
		cout << endl; 
	} 

	cout << "\n---------------------------------\n"; 
} 
```
Tentunya yaitu terdapat main sebagai insisasi awal jumlah queen yaitu 4.

Jika kode tersebut dijalankan maka akan mengeluarkan output:

![image](https://user-images.githubusercontent.com/58078219/77938843-818d8e00-72e0-11ea-9640-234ad79a0267.png)

Dapat dilihat bahwa Q merupakan Queen. Program kode akan menampilkan kemungkinan mana saja Queen diletakkan:

* Pada solusi pertama letak queen terdapat pada baris1-kolom2, baris3-kolom1, baris2-kolom4, baris4-kolom3. Hasil tersebut terjadi, pada queen mula-mula pada baris 1 diletakkan pada kolom 2. Queen tersebut akan menandai setiap area persegi sesuai dengan kemampuan pergerakan queen itu sendiri.
* Pada solusi pertama letak queen terdapat pada baris1-kolom3, baris2-kolom1, baris3-kolom4, baris4-kolom2. Hasil tersebut terjadi, pada queen mula-mula pada baris 1 diletakkan pada kolom 3. Queen juga akan menandai area persegi sama seperti solusi 1.

Dalam kasus ini, Queen tersebut akan menandai setiap area persegi pada baris dan kolom yang ditempatinya secara diagonal, vertikal, dan horisontal. Queen berikutnya akan menempati setiap area persegi yang masih belum ditandai oleh Queen pertama. Hal ini terus berlanjut sampai semuan Queen ditempatkan. Namun dikarenakan adanya implementasi penggunaan Constraint Satisfaction Problems, maka seandainya terdapat Queen yang tidak sesuai, algoritma yang ada akan kembali mencai perhitungan ulang terhadap area persegi yang masih bisa ditempati dengan mempertimbangkan tahap sebelum dan sesudah yang dijalankan.

Dengan Implemtasi Constarint Programming (Constraint Satisfaction Problems), seperti yang sudah dijelaskan pada contoh pada poin diatas, CSP bekerja secara sistematis mencoba semua kemungkinan penempatan area persegi dalam masalah atau bentrok, untuk menemukan solusi yang layak bagi queen tersebut.

CSP menggunakan metode Propagation dan bactracking sebagai pencarian kemungkinan

* Propagation - Setiap kali pemecah memberikan nilai ke variabel, batas constraints menambahkan batasan pada nilai yang mungkin dari beberapa variabel yang tidak ditetapkan. Pembatasan ini menyebar ke penugasan variabel yang akan digunanakn sebagai penanda area penempatan queen. Misalnya, dalam masalah 4-queens, setiap kali pemecah menempatkan queen, maka queen lain tidak dapat ditempatkan di baris dan diagonal queen yang saat ini aktif. Propagasi dapat mempercepat pencarian secara signifikan dengan mengurangi sekumpulan nilai variabel yang harus dieksplorasi oleh pemecah.
* Backtracking terjadi ketika solver tidak dapat memberikan nilai ke variabel berikutnya, karena kendala batas atau sudah ditemukan solusi. Dalam kedua kasus tersebut, pemecah melakukan backtracks ke tahap sebelumnya dan mengubah nilai variabel pada tahap tersebut ke nilai yang belum pernah dicoba. Dalam contoh 4-queens, berarti dimaksudkan dengan memindahkan ratu ke area persegi baru pada kolom saat ini.


Kembali ke: [Daftar Isi](#daftar-isi)


### Sumber
* https://www.freecodecamp.org/news/how-to-make-your-tic-tac-toe-game-unbeatable-by-using-the-minimax-algorithm-9d690bad4b37/
* https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-3-tic-tac-toe-ai-finding-optimal-move/
* https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-2-evaluation-function/
* https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-1-introduction/
* https://gist.github.com/MatthewSteel/3158579
* https://www.geeksforgeeks.org/n-queen-in-on-space/
* https://www.geeksforgeeks.org/n-queen-problem-using-branch-and-bound/
* https://developers.google.com/optimization/cp/queens
