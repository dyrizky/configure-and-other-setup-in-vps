# Make Custom Script to Run Automatically During Boot

|Souce | [thegeekdiary.com](https://www.thegeekdiary.com/centos-rhel-7-how-to-make-custom-script-to-run-automatically-during-boot/)|
| ---- | ---|
## Creating the custom script

1.  Let us first create a sample custom script to be run at system boot automatically.

    ```sh
    #vi /var/tmp/test_script.sh
    #!/bin/bash
    echo "This is a sample script to test auto run during boot" > /var/tmp/script.out
    echo "The time the script run was -->  `date`" >> /var/tmp/script.out
    ```
    
2.  Check and verify the file permission.
 
    ```sh
    # ls -lrt /usr/local/sbin/myscript.sh
    ```

3.  Add execute permission(if it’s not already set).
    ```sh
    # chmod +x /var/tmp/test_script.sh
    ```

## Creating new systemd service unit

1.  Create a new service unit file at /etc/systemd/system/sample.service with below content. The name of the service unit is user defined and can be any name of your choice.

    ```sh
    # vi /etc/systemd/system/sample.service
    [Unit]
    Description=Description for sample script goes here
    After=network.target

    [Service]
    Type=simple
    ExecStart=/var/tmp/test_script.sh
    TimeoutStartSec=0

    [Install]
    WantedBy=default.target
    ```
      INFO
    ```sh
    After= : If the script needs any other system facilities (networking, etc), modify the [Unit] section to include appropriate After=, Wants=, or Requires= directives.
    Type= : Switch Type=simple for Type=idle in the [Service] section to delay execution of the script until all other jobs are dispatched
    WantedBy= : target to run the sample script in
    ```

# configure-and-other-setup-in-vps
## Enable the systemd service unit

1.  Reload the systemd process to consider newly created sample.service OR every time when sample.service gets modified.
    ```sh
    # systemctl daemon-reload
    ```
2.  Enable this service to start after reboot automatically.
    ```sh
    # systemctl enable sample.service
    ```
3. Start the service.
    ```sh
    # systemctl start sample.service
    ```
4. Reboot the host to verify whether the scripts are starting as expected during system boot.
    ```sh
    # systemctl reboot
    ```