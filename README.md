# AddNode
This readme will explain you how to run a full devchain node and participate in the consensus by being a validator node.

#### Prerequisite
We use eris/monax framework to run our blockchain, so tto be able to run a full node you will have to install these tools:

[Install eris](https://monax.io/docs/tutorials/getting-started/index.html?redirect_from_eris=true)(run all the step 1)

If you not already have docker-machine and virtualbox you will need to install it:

[Install docker-machine](https://docs.docker.com/machine/install-machine/)
[Install virtualbox](https://www.virtualbox.org/wiki/Downloads)

##1. Create a new docker machine on your host
docker-machine create --driver=virtualbox MYVALNODE
> Note: we use virtualbox driver for drive to run our docker-machine for the tuto but you can use what driver you want

##2. Install all the monax/eris tools on it

`eris init --yes --machine MYVALNODE`

##3. Create a configuration folder with needed files

`mkdir configfolder`

To be able to sync your account with our blockchain you will need 3 files:

`genesis.json`(in this repo):
The basic account when the blockchain was created

`config.toml`(in this repo):
The configuration file of your node
Open the config.toml file and modify `moniker = "YOURNODENAME"` by modifying YOURNODENAME by a new name.

`priv_validator.json`
When you have invested for the first time, you have received a priv_validator.json file, this files contains you have your adress, your public key and your private key.

Copy these 3 files into your folder



Now you have to have a folder with this 3 files
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
your_ip:46657
You will get all the endpoint available on your running node
You can also watch logs with:
`eris chains logs --machine newvalnode devchain -f`

##6. Bond token to become validator
Now you have your own node, but you are not yet a validator, if you go on `your_ip:46657/list_validators` you will see than your adress is not yet present. You need to bond your token to be allowed to be a validator.
```
eris chains exec devchain "mintx bond --amt 50000 --pubkey YOURPUBKEY --to YOURADRESS --chainID devchain --node-addr=chain:46657 --sign-addr=keys:4767 --sign --broadcast" --machine MYVALNODE
```




