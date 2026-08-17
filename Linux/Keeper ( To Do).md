kpicli for keepass command line
the password we got for master db was not full so googling it shoed us the fulll pwd

had to install .net7.0 to use against keepassdump

puttygen for ssh key format
	in vi to remove spaces with regex :
		:%s/<number_of_spaces>//g
	also removed "Notes:"
	chmod 600 to the key and ssh to machine with input (-i) and key file
