# Run a full devchain validating node
This readme will explain you how to run a full devchain node and participate in the consensus by being a validator node.

#### Prerequisite
We use eris/monax framework to run our blockchain, so tto be able to run a full node you will have to install these tools:  
[Install eris](https://monax.io/docs/tutorials/getting-started/index.html?redirect_from_eris=true) (run all the step 1)  
If you not already have docker-machine and virtualbox you will need to install it:  
[Install docker-machine](https://docs.docker.com/machine/install-machine/)   
[Install virtualbox](https://www.virtualbox.org/wiki/Downloads)  

##1. Create a new docker machine on your host
`docker-machine create --driver=virtualbox MYVALNODE`  
> Note: we use virtualbox for driver to run our docker-machine for the tuto but you can use whatever driver you want (ex: you could use aws driver to launch your node on an aws ec2 instance)

##2. Install all the monax/eris tools on it  
`eris init --yes --machine MYVALNODE`

##3. Create a configuration folder with needed files  
`mkdir configfolder`

**To be able to sync your account with our blockchain you will need 3 files:**  
`genesis.json`(in this repo):  
The basic account when the blockchain was created

`config.toml`(in this repo):  
The configuration file of your node
Open the config.toml file and modify `moniker = "YOURNODENAME"` by modifying YOURNODENAME by a new name.

`priv_validator.json`:  
When you have invested for the first time, you have received a priv_validator.json file, this files contains you have your adress, your public key and your private key.

Copy these 3 files into your folder

**Now you have to have a folder with this 3 files**  
```
>ls configfolder/
config.toml
genesis.json
priv_validator.json
```

##4. Create your node
`eris chains new --dir configfolder/ --machine MYVALNODE devchain`

##5. verify your node is running
`docker-machine ls`
copy the ip of your machine and paste this line in your browser  
`your_ip:46657`  
You will get all the endpoint available on your running node.  
You can also watch logs with:  
`eris chains logs --machine newvalnode devchain -f`

##6. Bond token to become validator
>NB: here the token we are bonded are just the token who allow you to have weight in validating block. Your real devchain token are on a smart contract.  

Now you have your own node, but you are not yet a validator, if you go on `your_ip:46657/list_validators` you will see than your adress is not yet present. You need to bond your token to be allowed to be a validator.  
For that you will need your adress and your public key, present on your priv_validator.json, and your Total Token number, you can find on your account. There's no decimal on the blockchain so you will need to multiply by 100 You real devchain token number to get the number of voting token TOTALTOKEN hosted on your adress. Then run this command:  

```
eris chains exec devchain "mintx bond --amt TOTALTOKEN --pubkey YOURPUBKEY --to YOURADRESS --chainID devchain --node-addr=chain:46657 --sign-addr=keys:4767 --sign --broadcast" --machine MYVALNODE
```
Now if you refresh `your_ip:46657/list_validators` you will see your node appear.


