The 'created' column is a text column with the UNIX epoch. This needs to be turned into dates for querying to work.

1st create a new text column and copy the 'created' column into it:

ALTER TABLE measurements_total 
ADD COLUMN date_created_text2 text 

UPDATE
  measurements_total
SET
  date_created_text2 = created;

The 'created' has too many zeros and a decimal point needs to be inserted. Use the regexp_replace function:

UPDATE
  measurements_total
SET
  date_created_text2 = regexp_replace(
    date_created_text2, '([0-9]{10})', '\1.', 'g'
  )

Now you can create a new column with the double precision type:

ALTER TABLE measurements_total 
ADD COLUMN date_created2 double precision 

Now you can copy the date_created_text2 column into the date_created2 column while casting as double precision:

UPDATE
  measurements_total
SET
  date_created2 = cast(date_created_text2 as double precision);

Now create another column and make it the date type:

ALTER TABLE measurements_total 
ADD COLUMN date_created_date date 

Finally copy the date_created2 double precision values while casting as dates:

UPDATE
  measurements_total
SET
date_created_date = to_timestamp(date_created2);



*other useful commands:

SELECT * FROM measurements_total LIMIT 100

ALTER TABLE measurements_total
DROP COLUMN date_created_text2;



