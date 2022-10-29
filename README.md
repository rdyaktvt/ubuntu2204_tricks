# ubuntu2204_tricks
Some short and good tricks for ubuntu LTS 22.04
<a name="readme-top"></a>
<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#disable-login-password">Disable Login Password</a>
    </li>
    <li>
      <a href="#getting-started">startup da bash script çalıştırma</a>
      </li>
    </ol>
</details>

### Disable Login Password

* Edit this file:
  ```sh
  /lib/systemd/system/getty@.service
  ```

* #ExecStart=-/sbin/agetty -o '-p -- \\u' --noclear %I $TERM will be chenged.
  ```sh
  ExecStart=-/sbin/agetty -a asus19 --noclear %I $TERM
  ```

### getting-started

* Write new service. (/etc/systemd/system/start_newrl.service):
  ```sh
  /etc/systemd/system/start_newrl.service

  systemctl start my-service
  systemctl stop my-service
  systemctl enable my-service

  sudo systemctl status start_newrl

  ```

* start_newrl.service will start a bash script.
  ```sh
  [Unit]
  Description=My custom startup script
  # After=network.target
  # After=systemd-user-sessions.service
  # After=network-online.target

  [Service]
  # User=spark
  Type=simple
  # PIDFile=/run/my-service.pid
  #WorkingDirectory=/home/asus19/newrl
  #ExecStart=/usr/bin/screen -L -dm newrl -cp scripts/start.sh testnet
  ExecStart=/home/asus19/start_newrl.sh
  RemainAfterExit=yes
  # ExecReload=/home/transang/startup.sh reload
  # ExecStop=/home/transang/startup.sh stop
  # TimeoutSec=30
  # Restart=on-failure
  # RestartSec=30
  # StartLimitInterval=350
  # StartLimitBurst=10

  [Install]
  WantedBy=multi-user.target
  ```

* start_newrl.sh contents
  ```sh
  #!/bin/bash 
  screen -dmS syn bash -c 'cd /home/asus19/newrl/;scripts/start.sh testnet;exec bash'
  ```
  
* Only, we can see results from log file. Stat_newrl.sh will show us log file tail.
  ```sh
  #!/bin/bash
  tail -f newrl/logs/newrl-node-log
  ```
  
  <p align="right">(<a href="#readme-top">back to top</a>)</p>
