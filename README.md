# BitcoinUserPublicKeySimplification-Mapreduce

The data available for anomaly detection in bitcoin transaction network was of the following format:

1. input_edges
   Fields : sender_key, receiver_key, transaction_id, date, amount
2. input_user_keys : multiple comma separated public keys for the same user for different transactions.

### A sample in input_user_keys:
- 3,5,100,7
- 5,9
- 430,6,7000

### A sample in input_edges:
- 3,9,T100,20171208,65
- 5,7000,T78008, 20090909, 6
- 6,430, T45, 20120711, 100

#### It is important to identify that 3 and 5 the same user with different keys. It is best to group them with a common name say 1

- #1 3,5,100,7
- #2 5,9
- #3 430,6,7000

#### The input_edges file would then become

- #1,#2,T100,20171208,65
- #2,#3,T78008, 20090909, 6
- #3,#3, T45, 20120711, 100 <<-- see what is happening here, it gets easier to spot such suspicious self transactions

#### A MapReduce solution to this immediately came into mind. Since the files were big, the jars were compliled using small fractions of the sample file and then the actual data was run on AWS EMR

 
