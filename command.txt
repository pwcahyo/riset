# create sequence untuk autoincrement
============================================
CREATE SEQUENCE test_id_seq MINVALUE 3;
ALTER TABLE test 
  ALTER id SET DEFAULT nextval('test_id_seq');

# cara insert dengan auto increment
============================================
insert into sentbox (date,time,text,id_message,number) values (CURRENT_DATE, CURRENT_TIME,'Hallo pesan pertama',1,'08562636509')