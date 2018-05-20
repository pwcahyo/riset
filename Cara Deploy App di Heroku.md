# Cara deploy django dan postgresql di heroku
### 1. Mempunyai akun di heroku, [register disini](heroku.com)

### 2. Pastikan telah melakukan installasi [Heroku CLI](https://devcenter.heroku.com/articles/getting-started-with-python#set-up) 
*apabila sudah terinstall silahkan login dengan cara sebagai berikut*
```
heroku login
```
masukan alamat email dan password akun heroku

### 3. Ciptakan app didalam heroku
```
heroku create
```
secara otomatis heroku akan menciptakan app dengan nama randomize, contoh apabila create berhasil:
```
Creating sheltered-mountain-39240 in organization heroku... done, stack is ........
http://sheltered-mountain-39240.herokuapp.com/ | https://git.heroku.com/sheltered-mountain-39240.git
Git remote heroku added
```
**sheltered-mountain-39240** adalah nama app yang saya dapatkan

### 4. Tambahkan addons database postgresql 
agar database postgresql dapat dijalankan, maka perlu menambahkan addons didalam heroku, penambahan dapat dilakukan dengan cara
```
heroku addons:create heroku-postgresql:hobby-dev
```
**hobby-dev** adalah free paket untuk postgresql didalam heroku *nama tersebut jangan diubah*

### 5. Ciptakan satu file dengan nama **Pipfile** didalam *root* app
file ini digunakan untuk mendefinisikan requirement yang dibutuhkan pada server heroku nantinya, isi **Pipfile** adalah sebagai berikut:
```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
dj-database-url = "*"
django = "*"
gunicorn = "*"
psycopg2 = "*"
whitenoise = "*"

[requires]
python_version = "3.6"
```

### 6. Ciptakan satu file dengan nama **Procfile** didalam *root* app
file ini nantinya digunakan untuk menjalankan module app yang dideploy didalam server heroku, isi file **Procfile** adalah sebagai berikut
```
web: gunicorn smsgateway.wsgi --log-file -
```
*smsgateway* adalah aplikasi django yang kita buat, **bukan nama app di heroku** dengan contoh app name sebelumnya adalah ~~sheltered-mountain-39240~~

**Hal yang sangat penting** heroku akan me__reject__ push app
heroku akan mereject push app apabila tidak melakukan setting dari awal mengenai requirement 

### 4. Pastikan telah melakukan installasi `Pipenv`
installasi dapat menggunakan pip, sebagai berikut:
```
pip install pipenv
```
apabila mengalami error `ValueError: unknown locale: UTF-8`:
tambahkan baris dibawah ini kedalam `.bash_profile` **apabila menggunakan mac os/unix**
```
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```
### 3. 