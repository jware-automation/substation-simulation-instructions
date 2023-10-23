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
    sudo docker network inspect -f '{{range .Containers}}{{.Name}} {{.IPv4Address}}{{"\n"}}{{end}}' libiec61850-151-linux_ied_net
    ```

    - Note: if you reboot/restart the linux VM run `sudo ip link add mymacvlan70 link eth0 type macvlan mode bridge ; sudo ip addr add 172.24.16.20/24 dev mymacvlan70 ; sudo ifconfig mymacvlan70 up` to enable access to docker network IPs from your VM

## Configure OSHMI

1. Download OSHMI4Simulation.zip
2. Place the oshmi folder in C: drive
    - eg: `C:\oshmi`
3. Place the OSHMI-Shortcuts folder in your desktop
4. Go the the OSHMI-Shortcuts folder on your desktop
5. Start **WebServer**
6. Start **Screen Viewer**
7. Finally, Start **IEC61850Client**
