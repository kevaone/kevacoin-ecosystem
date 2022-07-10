# Kevacoin Node Setup - Ubuntu

![demo](/assets/node_guide/full.gif)

---

## 1. Update your system
`sudo apt update`

![1.1.0](/assets/node_guide/1.1.0.png)

`sudo apt upgrade`

![1.2.0](/assets/node_guide/1.2.0.png)

---

## 2. Setup directories
`mkdir kevacoin`

![2.1.0](/assets/node_guide/2.1.0.png)

`cd kevacoin`

![2.1.2](/assets/node_guide/2.1.2.png)

`mkdir blockchain`

![2.1.4](/assets/node_guide/2.1.4.png)

---

## 3. Get latest Kevacoin release
1. Download the release

    `wget https://github.com/kevacoin-project/kevacoin/releases/download/v0.16.8.0/kevacoin-0.16.8.0.tar.gz`

    ![3.1.0](/assets/node_guide/3.1.0.png)

2. Extract the download

    `tar xvzf kevacoin-0.16.8.0.tar.gz`

    ![3.2.0](/assets/node_guide/3.2.0.png)

3. Move the bin for simpler path

    `mv ./kevacoin-0.16.8.0/bin .`

    ![3.3.0](/assets/node_guide/3.3.0.png)

---

## 4. Kevacoin configuration
1. Create the config file

    `nano ./blockchain/kevacoin.conf`

    ![4.1.0](/assets/node_guide/4.1.0.png)
    ![4.1.1](/assets/node_guide/4.1.1.png)

2. Copy the following into the open editor

    ***\*Change `rpcuser` and `rpcpassword`***

    *\*`rpcallowip` may need adjusting pending other needs*

    *\*If you intend to have wallets on your server remove the `disablewallet` entry*

    ```
    server=1
    txindex=1
    disablewallet=1
    rpcuser=testuser
    rpcpassword=testpassword
    rpcport=19332
    rpcallowip=127.0.0.1
    ```

    ![4.2.0](/assets/node_guide/4.2.0.png)

3. Save and close open editor

    `Ctrl+X` then `y`

    ![4.3.0](/assets/node_guide/4.3.0.png)
    ![4.3.1](/assets/node_guide/4.3.1.png)

4. Touch the debug.log so it can receive permissions later

    `touch ./blockchain/debug.log`

    ![4.4.0](/assets/node_guide/4.4.0.png)

---

## 5. Setup service user
1. Create service user account

    `sudo useradd -U -r -s /bin/false srvkevacoin`

    ![5.1.0](/assets/node_guide/5.1.0.png)

2. Set service user as owner of the blockchain directory

    `sudo chown -R srvkevacoin: /home/ubuntu/kevacoin/blockchain`

    ![5.2.0](/assets/node_guide/5.2.0.png)

3. Set read permission on kevacoin.conf and debug.log for current user

    `sudo chmod -R +r /home/ubuntu/blockchain`

    ![5.3.0](/assets/node_guide/5.3.0.png)

---

## 6. Create Kevacoin service (Systemd)
1. Create the service file for systemd

    `sudo nano /etc/systemd/system/kevacoin.service`

    ![6.1.0](/assets/node_guide/6.1.0.png)
    ![6.1.1](/assets/node_guide/6.1.1.png)

2. Copy the following into the open editor

    ```
    [Unit]
    Description=Kevacoin Node
    After=network.target

    [Service]
    Type=forking
    ExecStart=/home/ubuntu/kevacoin/bin/kevacoind -daemon -datadir=/home/ubuntu/kevacoin/blockchain
    User=srvkevacoin
    LimitNOFILE=8192
    TimeoutStopSec=30min

    [Install]
    WantedBy=multi-user.target
    ```

    ![6.2.0](/assets/node_guide/6.2.0.png)

3. Save and close open editor

    `Ctrl+X` then `y`

    ![6.3.0](/assets/node_guide/6.3.0.png)
    ![6.3.1](/assets/node_guide/6.3.1.png)

4. Reload the systemd daemon so it can see new service

    `systemctl daemon-reload`

    ![6.4.0](/assets/node_guide/6.4.0.png)

5. Start the service

    `systemctl start kevacoin`

    ![6.5.0](/assets/node_guide/6.5.0.png)

6. Check that the service started without error

    `systemctl status kevacoin`

    ![6.6.0](/assets/node_guide/6.6.0.png)
    ![6.6.1](/assets/node_guide/6.6.1.png)

7. Enable the service to auto start

    `systemctl enable kevacoin`

    ![6.7.0](/assets/node_guide/6.7.0.png)

---

## 7. Notes

1. Can monitor status of your node by watching debug log

    `tail -n 1 -f ~/kevacoin/blockchain/debug.log`

    ![7.1.0](/assets/node_guide/7.1.0.png)
    ![7.1.1](/assets/node_guide/7.1.1.png)
