# Submission Belajar Membuat Aplikasi Back-End untuk Pemula dengan Google Cloud

## Kriteria
Terdapat 7 kriteria utama yang harus Anda penuhi dalam membuat proyek Bookshelf API.

## Kriteria 1 : Aplikasi menggunakan port 9000
Aplikasi yang Anda buat harus menggunakan port 9000. Jika komputer yang Anda gunakan untuk membuat submission tidak bisa memakai port 9000,  buatlah submission dengan port lain, lalu ketika submission hendak dikirimkan silakan ganti portnya ke 9000.

## Kriteria 2 : Aplikasi dijalankan dengan perintah npm run start.
Aplikasi yang Anda buat harus memiliki runner script start. Cara membuatnya, Anda tambahkan properti start ke dalam properti scripts pada package.json seperti berikut:
```sh
{
  "name": "submission",
  ...
  "scripts": {
    "start": "node src/server.js",
  }
}
```

Pastikan aplikasi tidak dijalankan dengan menggunakan nodemon. Jika Anda ingin menggunakan nodemon dalam proses development, masukkan nodemon kedalam runner script lain, contohnya:
```sh
{
  "name": "submission",
  ...
  "scripts": {
    "start": "node src/server.js",
    "start-dev": "nodemon src/server.js",
  }
}
```

## Kriteria 3 : API dapat menyimpan buku
API yang Anda buat harus dapat menyimpan buku melalui route:
- Method : **POST**
- URL : **/books**
- Body Request:
    ```sh
    {
        "name": string,
        "year": number,
        "author": string,
        "summary": string,
        "publisher": string,
        "pageCount": number,
        "readPage": number,
        "reading": boolean
    }
    ```
Server harus merespons **gagal** bila:
- Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, maka server akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        ```sh
        {
        "status": "fail",
        "message": "Gagal menambahkan buku. Mohon isi nama buku"
        }
        ```
- Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        ```sh
        {
        "status": "fail",
        "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
        }
        ```

Bila buku **berhasil** dimasukkan, server harus mengembalikan respons dengan:
- Status Code : **201**
- Response Body:
    ```sh
    {
        "status": "success",
        "message": "Buku berhasil ditambahkan",
        "data": {
        "bookId": "1L7ZtDUFeGs7VlEt"
        }
    }
  ```

## Kriteria 4 : API dapat menampilkan seluruh buku
API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route:
- Method : **GET**
- URL: **/books**

Server harus mengembalikan respons dengan:
- Status Code : **200**
- Response Body:
    ```sh
    {
    "status": "success",
    "data": {
        "books": [
                    {
                        "id": "Qbax5Oy7L8WKf74l",
                        "name": "Buku A",
                        "publisher": "Dicoding Indonesia"
                    },
                    {
                        "id": "1L7ZtDUFeGs7VlEt",
                        "name": "Buku B",
                        "publisher": "Dicoding Indonesia"
                    },
                    {
                        "id": "K8DZbfI-t3LrY7lD",
                        "name": "Buku C",
                        "publisher": "Dicoding Indonesia"
                    }
                ]
            }
    }
    ```
Jika **belum** terdapat buku yang dimasukkan, server bisa merespons dengan array books kosong.
```sh
{
    "status": "success",
    "data": {
        "books": []
    }
}
```

## Kriteria 5 : API dapat menampilkan detail buku
API yang Anda buat harus dapat menampilkan seluruh buku yang disimpan melalui route:
- Method : **GET**
- URL: **/books/{bookId}**

Bila buku dengan id yang dilampirkan oleh client tidak ditemukan, maka server harus mengembalikan respons dengan:
- Status Code : **404**
- Response Body:
```sh
{
    "status": "fail",
    "message": "Buku tidak ditemukan"
}
```
Bila buku dengan id yang dilampirkan ditemukan, maka server harus mengembalikan respons dengan:
- Status Code : 200
- Response Body:
```sh
{
    "status": "success",
    "data": {
        "book": {
            "id": "aWZBUW3JN_VBE-9I",
            "name": "Buku A Revisi",
            "year": 2011,
            "author": "Jane Doe",
            "summary": "Lorem Dolor sit Amet",
            "publisher": "Dicoding",
            "pageCount": 200,
            "readPage": 26,
            "finished": false,
            "reading": false,
            "insertedAt": "2021-03-05T06:14:28.930Z",
            "updatedAt": "2021-03-05T06:14:30.718Z"
        }
    }
}
```

## Kriteria 6 : API dapat mengubah data buku
API yang Anda buat harus dapat mengubah data buku berdasarkan id melalui route:
- Method : **PUT**
- URL : **/books/{bookId}**
- Body Request:
    ```sh
    {
        "name": string,
        "year": number,
        "author": string,
        "summary": string,
        "publisher": string,
        "pageCount": number,
        "readPage": number,
        "reading": boolean
    }
    ```
Server harus merespons **gagal** bila:
- Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, maka server akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        ```sh
        {
            "status": "fail",
            "message": "Gagal memperbarui buku. Mohon isi nama buku"
        }
        ```
- Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, maka server akan merespons dengan:
    - Status Code : **400**
    - Response Body:
        ```sh
        {
            "status": "fail",
            "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
        }
        ```
- Id yang dilampirkan oleh client tidak ditemukkan oleh server. Bila hal ini terjadi, maka server akan merespons dengan:
    - Status Code : **404**
    - Response Body:
        ```sh
        {
            "status": "fail",
            "message": "Gagal memperbarui buku. Id tidak ditemukan"
        }
        ```

Bila buku **berhasil diperbarui**, server harus mengembalikan respons dengan:
- Status Code : 200
- Response Body:
    ```sh
    {
        "status": "success",
        "message": "Buku berhasil diperbarui"
    }
    ```

## Kriteria 7 : API dapat menghapus buku
API yang Anda buat harus dapat menghapus buku berdasarkan id melalui route berikut:
- Method : **DELETE**
- URL: /books/{bookId}

Bila id yang dilampirkan tidak dimiliki oleh buku manapun, maka server harus mengembalikan respons berikut:
- Status Code : **404**
- Response Body:
    ```sh
    {
        "status": "fail",
        "message": "Buku gagal dihapus. Id tidak ditemukan"
    }
    ```

Bila id dimiliki oleh salah satu buku, maka buku tersebut harus dihapus dan server mengembalikan respons berikut:
- Status Code : **200**
- Response Body:
    ```sh
    {
        "status": "success",
        "message": "Buku berhasil dihapus"
    }
    ```
