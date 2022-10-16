# Baremetal Migration | LND
## Umbrel (pre 0.5) to Baremetal LND Migration

Follow the Raspibolt guide installing on new HW til wallet creation ( don't create the wallet ). Pay attention to system configuration, security, privacy and then bitcoin client and lightning client. Electrs is not needed in principle. This can be done with a NUC / Laptop / RPi. https://raspibolt.org/

Decide if you want to download a new blockchain copy, rsync from old node or just use mv command to move ~/umbrel/bitcoin/blocks & ~/umbrel/bitcoin/chainstate from umbrel to /data/bitcoin. 

# Old Node 

1. Stop umbrel lnd. ~/umbrel/sudo scripts/stop. sudo systemctl stop umbrel-startup. Check the bitcoin debug.log and lnd logs or sudo journalctl -f -u lnd to ensure it's been stopped.
2. Keep a copy of old seed and channel.backup

# New Node

3. Install bitcoin client and lnd client until wallet creation following www.raspibolt.org
4. Delete new machine lnd folder or move to /tmp
5. Copy lnd directory from umbrel to new machine
6. sudo rsync -arhvP --append-verify ~/umbrel/lnd   /data   no trailing slash, no lnd after /data so dir copied

# Old Node

7. Old umbrel mv channel.db channel.db_bak and lnd.conf lnd.conf_bak

# New Node

8. Remove tls files (tls.cert, tls.key), Tor cert (v3_onion_private_key) from /data/lnd and admin macaroon, macaroons.db, *macaroon: from /data/lnd/data/chain/bitcoin/mainnet
9. Ensure correct ownership and permissions for lnd and bitcoin dirs : sudo chown -R lnd:lnd /data/lnd and sudo chown -R bitcoin:bitcoin /data/bitcoin 
l0. Modify lnd.conf adding autounlock and deleting zmq from old umbrel lnd.conf
11. ### User lnd
12. Create password.txt as .password.txt
13. chmod 600 /data/lnd/.password.tx
15. Start lnd as lnd user
16. Check lncli getinfo, might need reboot
17. ### User admin - autostart on boot (follow raspibolt guide)
18. sudo systemctl enable lnd
19. sudo systemctl start lnd
20. systemctl status lnd
21. Allow user admin to work with LND
22. ln -s /data/lnd /home/admin/.lnd
23. sudo chmod -R g+X /data/lnd/data/
24. sudo chmod g+r /data/lnd/data/chain/bitcoin

Done
