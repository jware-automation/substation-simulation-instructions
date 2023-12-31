#  IEC61850 Substation Simulation Manual
## Preparing the Environment

1. Download libiec61850-1.5.1-linux.zip and extract the zip folder
2. Open the terminal in ibiec61850-1.5.1-linux directory
3. Run the following command to prepare the simulation environment
    ```
    sudo ./make_sim_env.sh
    ```
4. Run the following command.
   ```
   sudo nano /etc/environment
   ```
6. Append the following variables with IP addresses of each IED in the `etc/environment` file.
   ```
   ETH_INTERFACE=eth0
   SERVER_IED1="172.24.16.2"
   SERVER_IED4="172.24.16.3"
   SERVER_IED7="172.24.16.4"
   SERVER_IED13="172.24.16.5"
   SERVER_IED16="172.24.16.6"
   SERVER_IED20="172.24.16.7"
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
3. Run the following command to start the control app
   ```
   sudo ./examples/substation_simulation_control_app/substation_simulation_control_app
   ```
    - You may use an argumant to run the control app in a specific mode and exit automatically
      - Eg: `sudo ./examples/substation_simulation_control_app/substation_simulation_control_app 1` to launch normal mode

    | arg | mode    |
    |-----|---------|
    | 1   | nominal |
    | 2   | Transfomer overheat |
    | 3   | Abnormal Input Voltage |

5.  Run the following command to get the list of IP addresses of IEDs
    ```
    sudo docker network inspect -f '{{range .Containers}}{{.Name}}{{"\t"}}{{.IPv4Address}}{{"\n"}}{{end}}' libiec61850-151-linux_ied_net
    ```
    
    #### Notes
    - If you wish to change the default subnet, change the IP in docker compose file at  `./compose.yaml`. 

    - If you wish to use a different ethernet interface, you may specify the custom ethernet interface as `ETH_INTERFACE` in the `etc/environment` file. 

    - If you reboot/restart the linux VM run `sudo ip link add mymacvlan70 link eth0 type macvlan mode bridge ; sudo ip addr add 172.24.16.20/24 dev mymacvlan70 ; sudo ifconfig mymacvlan70 up` to enable access to docker network IPs from your VM

## Configure OSHMI (on Windows Side)

1. Download `oshmi_setup_v.6.30.exe` at [https://sourceforge.net/projects/oshmiopensubstationhmi/files/](https://sourceforge.net/projects/oshmiopensubstationhmi/files/) and install OSHMI.
2. Replace the existing files in the following directories by the files given in the folder **OSHMI-Files for Replacing** provided.
   
    | File | Location    |
    |-----|---------|
    | point_list.txt   | `C:\oshmi\conf` |
    | point_calc.txt   | `C:\oshmi\conf` |
    | iec61850_client.conf | `C:\oshmi\conf` |
    | start_hmi.bat | `C:\oshmi\bin` |
    | IEC61850Client   | `C:\Users\<PC-Username>\Desktop\OSHMI` |
    | orr1.svg   | `C:\oshmi\svg` |
    | screen_list.js | `C:\oshmi\svg` |

    Set IED container IP addresses/hostnames (RED) and Ports (YELLOW) in `iec61850_client.conf` as shown below:
    ![iec61850_client_config](https://github.com/jware-automation/substation-simulation-instructions/assets/83968050/72bb8b4f-272c-4b54-93ac-0d94f17a2bd2)


4. Go the the OSHMI folder on your desktop
5. Run **_Start_OSHMI** in the OSHMI folder on your desktop.
6. Finally, Start **IEC61850Client**
