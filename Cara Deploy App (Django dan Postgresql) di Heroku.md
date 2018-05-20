# Cara deploy django dan postgresql di heroku (Mac OS/Unix)
### 1. Mempunyai akun di heroku, [register disini](https://heroku.com)

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
**hobby-dev** adalah free paket untuk postgresql didalam heroku, *nama tersebut jangan diubah*, apabila berhasil maka akan muncul
```
Creating heroku-postgresql:hobby-dev on ⬢ sheltered-mountain-39240... free
Database has been created and is available
 ! This database is empty. If upgrading, you can transfer
 ! data from another database with pg:copy
Created postgresql-flexible-35614 as DATABASE_URL
Use heroku addons:docs heroku-postgresql to view documentation
```
**postgresql-flexible-35614** perlu dicatat, digunakan untuk konfigurasi database distep selanjutnya

### 5. Ciptakan satu file dengan nama `Pipfile` didalam *root* app
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

### 6. Ciptakan satu file dengan nama `Procfile` didalam *root* app
file ini nantinya digunakan untuk menjalankan module app yang dideploy didalam server heroku, isi file **Procfile** adalah sebagai berikut
```
web: gunicorn smsgateway.wsgi --log-file -
```
*smsgateway* adalah aplikasi django yang kita buat, **bukan nama app di heroku** dengan contoh app name sebelumnya adalah ~~sheltered-mountain-39240~~

### 7. Pastikan telah melakukan installasi `Pipenv`
installasi dapat menggunakan pip, sebagai berikut:
```
pip install pipenv
```
test apakah `pipenv` berjalan dengan normal, ketikan `pipenv` di terminal, apabila tidak dikenali, tambahkan baris dibawah ini kedalam `.bash_profile`
```
export PATH="/Users/[nama user]/.local/bin:$PATH"
```
**[nama user]** diganti dengan nama user pengguna laptop. 

### 8. Menciptakan virtual environment untuk upload kedalam heroku
apabila **Pipenv** sudah dikenali, maka lanjutkan dengan cara masuk kedalam direktori *root* aplikasi django, kemudian jalankan perintah berikut.
```
pipenv --three
pipenv install
```
maka akan menghasilkan file **Pipfile.lock** didalam folder *root django* apabila mengalami error `ValueError: unknown locale: UTF-8`:
tambahkan baris dibawah ini kedalam `.bash_profile` **apabila menggunakan mac os/unix**
```
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```
apabila sudah maka jangan lupa untuk ulangi [perintah ini](#8-menciptakan-virtual-environment-untuk-upload-kedalam-heroku)
kemudian jalankan **pipenv shell**
```
pipenv shell
```

### 9. Edit konfigurasi aplikasi django yang telah dibuat
nama aplikasi django saya adalah smsgateway, maka yang harus diedit adalah file **smsgateway/settings.py** menjadi
```
import dj_database_url #tambahkan ini diatas sendiri
```
kemudian ijinkan semua host untuk akses app
```
ALLOWED_HOSTS = ['*'] #ijinkan all host
```
tambahkan middleware berikut
```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware', #tambahkan baris ini
]
```
konfigurasi untuk database, tambahkan baris berikut dibawah config **DATABASE** yang sudah ada
```
db_from_env = dj_database_url.config() #config database
DATABASES['default'].update(db_from_env)
```
konfigurasi file static, tambahkan baris berikut ini
```
STATIC_URL = '/static/' 
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
# Extra places for collectstatic to find static files.
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)
```

### 10. Push seluruh file aplikasi django yang sudah dibuat ke dalam heroku
Untuk push direpository heroku dapat dilakukan dengan cara
```
git push heroku master
```
apabila mengalami error berikut
```
fatal: heroku does not appear to be a git repository
fatal: could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
error tersebut dikarenakan belum ditambahkan *git remote* , maka tambahkan dengan cara sebagai berikut
```
git remote add heroku https://git.heroku.com/sheltered-mountain-39240.git
```
pastikan **sheltered-mountain-39240** adalah nama repository app didalam heroku, kemudian jangan lupa [ulangi](#10-push-seluruh-file-aplikasi-django-yang-sudah-dibuat-ke-dalam-heroku)

### 11. Migrasi database postgresql
Siapkan database untuk dimigrasi, dengan menjalankan perintah berikut
```
heroku run python manage.py makemigrations
```
kemudian jalankan migrasi
```
heroku run python manage.py migrate
```

### 12. Ciptakan superuser untuk django admin
menciptakan super user django admin
```
heroku run python manage.py createsuperuser
```
masukan username dan password
```
Username (leave blank to use 'u23814'): pwcahyo
Email address: pwcahyo@gmail.com
Password: 
Password (again): 
Superuser created successfully.
```

### 12. Menjalankan django di heroku
bangkitkan dyno untuk mengaktifkan server
```
heroku ps:scale web=1
```
kemudian jalankan 
```
heroku open
```
maka akan membuka browser dan menjalankan aplikasi django, untuk menuju admin tambahkan **/admin** dibelakang **url**

untuk melihat log dapat dilakukan dengan cara
```
heroku logs --tail
```

### 13. Menjalankan django di local
django dapat dijalankan juga di local, dengan perintah:
```
heroku local web
```
kemudian jalankan pada host **http://0.0.0.0:5000/admin**