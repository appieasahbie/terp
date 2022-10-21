# ˜”*°•.˜”*°• Terp Network •°*”˜.•°*”˜




   ![trepp](https://user-images.githubusercontent.com/108979536/195939456-266958e0-a623-436c-ac4e-f2a3de6a5836.png)
# Offecial Links of Terp

 + Github : https://github.com/terpnetwork/terp-core
 + Discord Channel :https://discord.gg/SQFJMGV8
 
 Terp node setup for testnet tutorial
 
 1-ɪɴᴛᴀʟʟᴀᴛɪᴏɴ ᴏɴᴇ ᴛɪᴍᴇ (ꜱᴄʀɪᴘᴛ ᴀᴜᴛᴏᴍᴀᴛɪᴄ ɪɴꜱᴛᴀʟʟᴀᴛɪᴏɴ)


              wget -O terp.sh https://raw.githubusercontent.com/appieasahbie/terp/main/terp.sh && chmod +x terp.sh && ./terp.sh
    
    
+ Minimal

    4 GB RAM

    100 GB SSD

    3.2 x4 GHz CPU

+ Recommended

    8 GB RAM

    1 TB NVME SSD

    3.2 GHz x4 GHz CPU / ubuntu linux x64-x86
    
# Open poorts

    sudo ufw default allow outgoing
    sudo ufw default deny incoming
    sudo ufw allow ssh/tcp
    sudo ufw allow ${TERP_PORT}656,${TERP_PORT}660/tcp
    sudo ufw enable

# ℙ𝕠𝕤𝕥 𝕚𝕟𝕤𝕥𝕒𝕝𝕝𝕒𝕥𝕚𝕠𝕟 :


   + Update packages:

      
          sudo apt-get update && sudo apt upgrade -y 
          
          
   + Install toolchain


          sudo apt-get install make build-essential gcc git jq chrony -y


   + Variables inloading into system

    
          source $HOME/.bash_profile


   + Check the status of your node


          journalctl -u terpd.service -f
          
          
# 𝐂𝐫𝐞𝐚𝐭𝐞 𝐰𝐚𝐥𝐥𝐞𝐭
( please save your keys on your nodepad and keep it save)

          terpd keys add $WALLET
          
( In orde to recover you old wallet use this command)

          terpd keys add $WALLET --recover
          
 + To see your current wallet use this command


          terpd keys list
          
# Add wallet and valoper address into variables 

         TERP_WALLET_ADDRESS=$(terpd keys show $WALLET -a)
         TERP_VALOPER_ADDRESS=$(terpd keys show $WALLET --bech val -a)
         echo 'export TERP_WALLET_ADDRESS='${TERP_WALLET_ADDRESS} >> $HOME/.bash_profile
         echo 'export TERP_VALOPER_ADDRESS='${TERP_VALOPER_ADDRESS} >> $HOME/.bash_profile
         source $HOME/.bash_profile
         
# Add tokens to your wallet (ask for tokens on discord app )


# Create Validator (after funding your wallet )

   + Firstly check your Balance you must have 1 terp to create a validator
   
         terpd query bank balances $TERP_WALLET_ADDRESS
         
         
 To create validator use this command 
 
         terpd tx staking create-validator \ 
         --amount 1000000uterpx \ 
         --commission-max-change-rate "0.05" \ 
         --commission-max-rate "0.10" \ 
         --commission-rate "0.05" \ 
         --min-self-delegation "1" \  
         --pubkey $(terpd tendermint show-validator) \ 
         --moniker $MONIKER_NAME \ 
         --chain-id $CHAIN_ID \ 
         --fees 300upersyx \
         --from <key-name>
         -y
        
        
  (Please replace enter you moniker and replace <key-name> bij wallet)
   
   
# Check health of your Validator
   
         terpd q staking validators | grep moniker
   
   
# Monitoring

   
  + Unjail Validator

        terpd tx slashing unjail --from wallet --chain-id athena-1 --gas-prices 0.1uterpx --gas-adjustment 1.5 --gas auto -y  
   
  + Jail Reason

        terpd query slashing signing-info $(terpd tendermint show-validator)  
   
  + List All Active Validators

        terpd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl  
   
   
   + List All Inactive Validators
   

          terpd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl  
   
   + View Validator Details

           terpd q staking validator $(terpd keys show wallet --bech val -a)  
   
   
   + Delegate

           terpd tx staking delegate <TO_VALOPER_ADDRESS> 1000000uterpx --from wallet --chain-id athena-1 --gas-prices 0.1uterpx --gas-adjustment 1.5 --gas auto -y 
   
   + Redelegate
   
           terpd tx staking redelegate $(terpd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uterpx --from wallet --chain-id athena-1 --gas-prices 0.1uterpx --gas-adjustment 1.5 --gas auto -y 
   
   
   + Unbond

           terpd tx staking unbond $(terpd keys show wallet --bech val -a) 1000000uterpx --from wallet --chain-id athena-1 --gas-prices 0.1uterpx --gas-adjustment 1.5 --gas auto -y 
   
   
   + Get Validator Info

           terpd status 2>&1 | jq .ValidatorInfo
   
   + Get Catching Up

           terpd status 2>&1 | jq .SyncInfo.catching_up
   
   + Get Latest Height

           terpd status 2>&1 | jq .SyncInfo.latest_block_height
   
   + Remove node
   
           sudo systemctl stop terpd && sudo systemctl disable terpd && sudo rm /etc/systemd/system/terpd.service && sudo systemctl daemon-reload && rm -rf $HOME/.terp  && rm $(which terpd) 
   
   + Enable Service

           sudo systemctl enable terpd
   

   + Disable Service

           sudo systemctl disable terpd
   

    + Run Service

           sudo systemctl start terpd
   

    + Stop Service

           sudo systemctl stop terpd
   

    + Restart Service

           sudo systemctl restart terpd
   

    + Check Service Status

           sudo systemctl status terpd
   

    + Check Service Logs

           sudo journalctl -u terpd -f --no-hostname -o cat
   
   That`s All
   
   (don`t forget to give me star enjoy )

   
   
 # [Buy me a cup of coffee.](https://paypal.me/AbdelAkridi?country.x=NL&locale.x=en_US)
   
   
        
    
