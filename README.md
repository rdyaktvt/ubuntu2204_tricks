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

* Edit edilecek dosya:
  ```sh
  /lib/systemd/system/getty@.service
  ```

* #ExecStart=-/sbin/agetty -o '-p -- \\u' --noclear %I $TERM değiştirilecek.
  ```sh
  ExecStart=-/sbin/agetty -a asus19 --noclear %I $TERM
  ```

### getting-started

* yeni bir service yazıyoruz. (/etc/systemd/system/start_newrl.service):
  ```sh
  /etc/systemd/system/start_newrl.service

  systemctl start my-service
  systemctl stop my-service
  systemctl enable my-service

  sudo systemctl status start_newrl

  ```

* start_newrl.service içinde bir bash script i çalıştırıyoruz.
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

* start_newrl.sh içeriği
  ```sh
  #!/bin/bash 
  screen -dmS syn bash -c 'cd /home/asus19/newrl/;scripts/start.sh testnet;exec bash'
  ```
  
* makina açıldığında bu script arka planda çalışıyor ve sonuçları ancak log üzerinden görebiliyoruz. bunun içinde stat_newrl.sh dosyası oluşturuldu
  ```sh
  #!/bin/bash
  tail -f newrl/logs/newrl-node-log
  ```
  
  <p align="right">(<a href="#readme-top">back to top</a>)</p>
