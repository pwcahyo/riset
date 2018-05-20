# Cara deploy django dan postgresql di heroku
### 1. Mempunyai akun di heroku, [register disini](heroku.com)

### 2. Pastikan telah melakukan installasi [Heroku CLI](https://devcenter.heroku.com/articles/getting-started-with-python#set-up) __apabila sudah terinstall silahkan login dengan cara__
```
heroku login
```
masukan alamat email dan password akun heroku

### 3. Ciptakan app dengan cara
```
heroku-create
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
**hobby-dev** adalah free paket untuk postgresql didalam heroku __nama tersebut jangan diubah__

### 5. 
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