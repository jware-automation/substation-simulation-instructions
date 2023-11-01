#  IEC61850 Substation Simulation Manual
## Preparing the Environment

1. Download libiec61850-1.5.1-linux.zip and extract the zip folder
2. Open the terminal in ibiec61850-1.5.1-linux directory
3. Run the following command to prepare the simulation environment
    ```
    sudo ./make_sim_env.sh
    ```

## Start the Simulation
1. Run the following commmand to start the IEDs
    ```
    sudo ./start_simulation.sh
    ```
2. Run the following command to build the control app (This is required only once unless the control app code is modified.)
    ```
    sudo ./build_control_app.sh
    ```
3. Run `sudo ./examples/substation_simulation_control_app/substation_simulation_control_app` to start the control app
    - You may use an argumant to run the control app in a specific mode and exit automatically
      - Eg: `sudo ./examples/substation_simulation_control_app/substation_simulation_control_app 1` to launch normal mode

    | arg | mode    |
    |-----|---------|
    | 1   | nominal |
    | 2   | Transfomer overheat |
    | 3   | Abnormal Input Voltage |

4.  Run the following command to get the list of IP addresses of IEDs
    ```
    sudo docker network inspect -f '{{range .Containers}}{{.Name}}{{"\t"}}{{.IPv4Address}}{{"\n"}}{{end}}' libiec61850-151-linux_ied_net
    ```
```json
   #f03c15
```
    #### Notes
    - If you wish to change the default IP address or subnet, change the IP in docker compose file at  `./compose.yaml`. Keep the 4th digit of the IP address as shown in the table below:
      
        | IED | IP Last Byte | Example |
        |-----|---------|---------|
        | server_ied1   | 11 | 172.24.16.11 |
        | server_ied4   | 14 | 172.24.16.14 |
        | server_ied7   | 17 | 172.24.16.17 |
        | server_ied13   | 23 | 172.24.16.23 |
        | server_ied16   | 26 | 172.24.16.26 |
        | server_ied20   | 30 | 172.24.16.30 |

    - If you wish to use a different ethernet interface, you may specify the custom ethernet interface as `ETH_INTERFACE` of each IED in `./compose.yaml`. 

    - Note: if you reboot/restart the linux VM run `sudo ip link add mymacvlan70 link eth0 type macvlan mode bridge ; sudo ip addr add 172.24.16.20/24 dev mymacvlan70 ; sudo ifconfig mymacvlan70 up` to enable access to docker network IPs from your VM

## Configure OSHMI (on Windows Side)

1. Download `oshmi_setup_v.6.30.exe` at [https://sourceforge.net/projects/oshmiopensubstationhmi/files/](https://sourceforge.net/projects/oshmiopensubstationhmi/files/) and install OSHMI.
2. Replace the existing files in the following directories by the files given in the folder **OSHMI-Files for Replacing** provided.
   
    | File | Location    |
    |-----|---------|
    | point_list.txt   | `C:\oshmi\conf` |
    | iec61850_client.conf | `C:\oshmi\conf` |
    | IEC61850Client   | `C:\Users\<PC-Username>\Desktop\OSHMI` |
    | orr1.svg   | `C:\oshmi\svg` |
    | screen_list.js | `C:\oshmi\svg` |

    Set IED container IP addresses/hostnames (RED) and Ports (YELLOW) in `iec61850_client.conf` as shown below:
    ![iec61850_client_config](https://github.com/jware-automation/substation-simulation-instructions/assets/83968050/72bb8b4f-272c-4b54-93ac-0d94f17a2bd2)


4. Go the the OSHMI folder on your desktop
5. Start **WebServer**
6. Start **Screen Viewer**
7. Finally, Start **IEC61850Client**
