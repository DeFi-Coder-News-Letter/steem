# VIT Witness HOW-TO

## Introduction

Welcome to the Vice Industry Token Witness HOW TO!

We'll go over what it means to be a Witness, what a witness server is, and how to set one up!  This HOW TO is a work in progress and we'll do our best to keep it up-to-date.  So if you're ready, let's get started!

## What Is A Witness Node?

I'm assuming you understand that a blockchain is built using multiple nodes, and that these nodes are responsible for maintaining the distributed ledger.

A Witness is a node in the network that is responsible for maintaining the chain, and has the ability to produce new blocks.

## Why Would I Want To Operate A Witness Node?  

Not just the prestige.  Not just the glory.  Not just the screaming fans... but you want to do it for the VIT!  That's right, Witnesses receive VIT for signing of new blocks.

Let's Get Started
    Installing the Docker Image

1. Install Docker Community Edition:
   https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1

2. Open a shell, and download the VIT Witness docker

```
    docker pull tokken/witness:3
```    
## Create A VIT Tube Account

1. Go to VIT Tube and create your account:
https://touch.tube/

2. Choose a username and press the **Continue** button  (If your username is said to be reserved, then you already registered, and contact me we'll go from there.  This is where things might get tricky, as most of you have likely signed up already and sent in your 1500VIT so I'll have to get your accounts unlocked.  If they are already unlocked then you are ready to continue.)

3. Enter your email you wish to associate with your VIT Tube account

4. Check the email account you provided, and click on the provided link to confirm your account.  You can also copy the link and paste it in your browser's address bar

5. Confirm your email, by clicking on the Continue button.  At this point, you'll need to wait for a VIT Tube Admin to approve your account.  When your account has been approved, you'll receive an email.

## Setting Up Your Keys

1. Follow the link you recieved in your email

2. Copy the password generated for you, save this password somewhere safe

3. Save the password from step 2 in a file, don't put it in the cloud

4. When I said don't put your password in the cloud in step 3, I meant it.  Take it out of the cloud
and put it some place much safer

5. Press Continue

6. Enter your password. Press **Continue**.

7. Sign in using your username and the password you didn't save in the cloud.

8. On the profile page select the **Wallet** button at the top right, and then press **"Display your private and public keys"**

10. Enter the password that you DID NOT store in the cloud.

11. Now copy your *ACTIVE PRIVATE KEY* and save it some place safe.

12. From the command line: 

```
docker run -it tokken/witness:3 
/usr/local/steemd-full/bin/cli_wallet -s wss://peer.vit.tube
```

12. At the ```locked >>>``` prompt type: ```suggest_brain_key```

13. Copy the ```pub_key``` and the ```wif_priv_key```.  Save these some place.

14. Exit the ```cli_wallet``` by pressing *Ctrl-D*

15. In a terminal window, start and editor and create a new file called: ```docker-compose.yml```  put this in a new directory

16.  In the ```docker-compose.yml``` file put (type DO NOT COPY AND PASTE) the following code:

```
vit-witness:
  restart: always
  image: tokken/witness:3
  ports:
    - 8090:8090
    - 2001:2001
  environment:
    USE_WAY_TOO_MUCH_RAM: 1
    USE_FULL_WEB_NODE: 1
    STEEMD_PRIVATE_KEY: <YOUR_PRIVATE_KEY>
    STEEMD_WITNESS_NAME: <YOUR_WITNESS_NAME>
    STEEMD_SEED_NODES: peer.vit.tube:2001
  volumes:
    - /var/lib/vit-blockchain/production-vit-witness:/var/lib/steemd
```

<YOUR_PRIVATE_KEY> = the ```wif_priv_key``` you generated earlier

<YOUR_WITNESS_NAME> = the name you want to publish for your witness

17. Save the above file

18. At the command line: ```docker-compose up -d```

16. Press ```<enter>```

17. Get your witness container name by: ```docker container ls```

18. To see the chain in action: ```docker logs -f <witness_container>``` - ```CTRL-C``` to stop

## Adding Your Wallet

1. At the command line(localhost this time):
```
docker exec -it <witness_container> /usr/local/steemd-full/bin/cli_wallet -s ws://localhost:8090**
```

2. At the command prompt (don't forget the quotes): ```set_password "<NEW_PASSWORD>"```  It'll return:  ```null```

3. ```unlock "<NEW_PASSWORD>"``` It'll return (don't forget the quotes): ```null```

4.  Finally you can import your active key by: ```import_key <ACTIVE_PRIVATE_KEY>```(Active Private Key from **Step 11** in previous section.)

5. And now, name your witness, advertise it's availability and go!: 

```
update_witness <YOUR_USERNAME> "<YOUR_WEBSITE_HERE>" "<PUB_KEY>" {"account_creation_fee":"0.100 VIT", "maximum_block_size":131072, "sbd_interest_rate":0} true
```

<PUB_KEY> = the public key generated in the first section when you performed the ```suggest_brain_key```

 The website is a site you either own, or just a string you can use later.  I put in ```"No website yet"```  because I don't web master a site

6. ```list_witnesses 1 100``` will show all the witnesses. ```CTRL-D``` to exit the cli_wallet.  This should show your witness as being registered.  It won't start signing blocks however until you receive some votes!

7. ```get_active_witnesses``` will show all the currently active witnesses

8. To see the chain in action: ```docker logs -f <witness_container>``` - ```CTRL-C``` to stop

9.  YOU'RE DONE. And Thank You!

