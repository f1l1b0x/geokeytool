Prepare to create keys:

1. Build Parallel Bcrypt Engine 

- Get a copy of the geokeytool repository

  git clone https://github.com/oscar-davids/geokeytool

- Run "make"

  cd geokeytool
  
  make
  
- check engine so file 
 
  libclibhash.so.5.1.0
 
2. Install Dependencies:

- Install required python packages

  sudo pip install -r requirements.txt
  
3. Create Keys

python geokeys.py --create --password donotqwerty --gps +40.73150390,-73.96328405 --round 64 --unit dm

--password : Choose a easy to remember Password (try to have at least 13 chars use Capital letters numbers and special chars)
--gps : Collect Geo Coordinates in Decimaldegrees of location 
--round : the number of rounds bcrypt will hash. Use min rounds in production)
--unit : choose the quanitfier for the location (m, dm, cm, mm will define how long a recovery search takes)

The output contains the ECDSA keys and die resulting Bitcoin wallet import format WIF:

{
"password":"aaaaa", 
"gps":"+40.73150435,-73.96328396", 
"bcrypt output1":"$2a$16$XES0XUK3L0HkMBGzMESxM.ZhpdqZfjfoAwkoejes7GR9Plujl5oXy", 
"bcrypt output2":"$2a$16$WhPiWUSyWhjfXRC3XRWuWeZL1bfJgh0ILOLAM9bZpy8jzWAJQmEV.", 
"private key":"005a4c3162664a676830494c4f4c414d39625a7079386a7a57414a516d45562f", 
"public key":"049be5cdf427e5a895079ca692dab3549e3345d48140d6867800b6c9047962fd8bdbf4f7440a62234ab842b30e088b695c70824cd63b2a400d5f02c29cd509be76", 
"bitcoin address":"004c22cb5dae25afa2ceb6ba8d2e2fc42bbc8ad6c106964a9f", 
"bitcoin address base58":"17wa38uT7SZEz9pWpv6JN5qhZHe8rSbDhY", 
"wif":"5HpSbFifKDMTonAH6NfLBL52fkAaq8EzGdQSioRQAq27VPMTrpF"
}

From here you can backup the key and use it as you wish.


For recovery:

1. Prepare step 1-2 

2. Install Bitcoin client 

  https://bitcoin.org/en/full-node
  
- start bitcoin with daemon mode


3. Generate local bitcoin UTXO database 

- stop bitcoin daemon because can not access bitcoin's database at once.
  
  bitcoin-cli stop
 
- dump UTXO set from chainstate
  
  python updatebtcdb.py $HOME/.bitcoin/chainstate
  
4. Recovery of keys
  
  python geokeys.py --recover --password donotqwerty --gps +40.73150390,-73.96328405 --round 64 --unit dm --radius 0.5
  
  --password :  Remember your Password 
--gps : Collect Geo Coordinates in Decimaldegrees of location 
--round : Use same rounds as with creation
--unit : choose the maximum quanitfier for the search (m, dm, cm, mm will define how long a recovery search takes)
  
After a successful search you will find your keys in a local text file and in your terminal.

From here on you can use your key again.

Please read the paper to this proof of concept to get more details:


  


