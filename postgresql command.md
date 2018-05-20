## create sequence untuk autoincrement
```
CREATE SEQUENCE autoincr_sentbox MINVALUE 1;
ALTER TABLE sentbox 
  ALTER id SET DEFAULT nextval('autoincr_sentbox');
```
- **autoincr_sentbox** adalah nama sequence

## cara insert dengan auto increment
```
insert into sentbox (date,time,text,id_message,number) values (CURRENT_DATE, CURRENT_TIME,'Hallo pesan pertama',1,'08562636509')
```
- **sentbox** adalah tabel
- **CURRENT_DATE** adalah tanggal sekarang, **2018-11-19**
- **CURRENT_TIME** adalah jam sekarang, **20:18:12...**