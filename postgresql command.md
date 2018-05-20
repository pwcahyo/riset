## create table
```
CREATE TABLE sentbox
(
  id serial,
  date date,
  "time" time without time zone,
  text text,
  id_message integer NOT NULL,
  "number" character(25),
  CONSTRAINT "primary_key id_pesan" PRIMARY KEY (id_message)
)
WITH (
  OIDS=FALSE
);
```
- **serial** adalah data type auto increment

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

## update data
```
UPDATE auth_user SET password='djangoframework' WHERE username='pwcahyo';
```