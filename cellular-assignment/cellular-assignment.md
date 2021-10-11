# Cellular Assignment

In this assignment, we will use srsLTE (now [srsRAN](https://github.com/srsran/srsRAN)) to instantiate a softwarized cellular network on Colosseum and exchange traffic between the base station and the user(s).


## Make a reservation on Colosseum

1. Connect to Colosseum VPN (instructions [here](https://colosseumneu.freshdesk.com/support/solutions/articles/61000285824-cisco-anyconnect-remote-vpn-access) and login to [Colosseum website](https://experiments.colosseum.net)).
2. Make a reservation with two SRNs with the `srslte-20-04` image (see instructions on [Making a Reservation](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253463-making-a-reservation-interactive-and-batch-mode-)). Call the reservation in a meaningful way (e.g., your name). Two hours should suffice.
3. In the reservation page, you can find the assigned SRNs/nodes and their hostnames by hovering over nodes. At your scheduled reservation time, open four terminals (two for each node). As a convention, the SRN with the lowest ID will be used as a base station; the other SRN as User Equipment (UE). Now, ssh into the assigned Colosseum SRNs[^1]<sup>,</sup>[^2] 
(see instructions on [Logging into an SRN](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253366-logging-into-an-srn)): `ssh <srn-hostname>`
4. In one of the three terminals, run the following command to start a Colosseum Radio-frequency (RF) scenario through the Colosseum CLI API (see instructions [here](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253397-colosseum-cli)): `colosseumcli rf start 1009 -c`. When the scenario starts, an output similar to the following is returned (time is in UTC):

```
Scenario Start Time is 22:30:45
```
5. This will engage the Colosseum RF Channel Emulator and make the necessary connections between the USRPs of the reserved nodes based on the parameters set in the specific RF scenario (see the [Scenario Summary List](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000276224-scenarios-summary-list)). In this assignment we will use the [Test Scenario All Paths 0 dB (1009)](https://colosseumneu.freshdesk.com/support/solutions/articles/61000277641-test-scenario-all-paths-0-db-1009). You can check if the RF scenario is active and running by executing the following command: `colosseumcli rf info`

[^1]: You need to setup your ssh config files by following the instructions in [SSH Proxy Setup](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253369-ssh-proxy-setup).
[^2]: The password for the `srslte-20-04` container is `ChangeMe`.


## Configure the cellular nodes

1. Configure the base station:
    - Configure the RF parameters of the base station to match the following parameters. Downlink (DL) and uplink (UL) transmission frequencies need to be compatible with the Colosseum scenario in use.
    - `vim /root/radio_code/srslte_config/ue.conf`:<br/><br/>

    ```
    [rf]
    dl_earfcn = 3400
    ul_freq = 1005000000
    dl_freq = 995000000
    tx_gain = 20
    rx_gain = 20
    
    time_adv_nsamples = 100
    ```

2. Configure the UE:
    - On the base station node, read the database configuration of the EPC: `cat /root/radio_code/srslte_config/user_db.csv`
    - You will see different entries for different UEs (one per line), for instance:<br/><br/>

    ```
    ue1,xor,001010123456789,00112233445566778899aabbccddeeff,opc,63bfa50ee6523365ff14c1f45f88737d,8000,0000000012b8,7,dynamic
    ue2,mil,001010123456780,00112233445566778899aabbccddeeff,opc,63bfa50ee6523365ff14c1f45f88737d,8000,000000001255,7,dynamic
    ```

    - On the UE node, modify the configuration of the UE. `vim /root/radio_code/srslte_config/ue.conf`:
        - Scroll down to the `USIM configuration` section
        - Configure the fields to match the configuration of one of the UEs reported in the EPC database. Note that you may need to uncomment the `opc` field. Also, the `imei` field is not registered in the EPC database, feel free to leave it as is. As an example:<br/><br/>

        ```
        [usim]
        mode = soft
        algo = mil
        opc  = 63bfa50ee6523365ff14c1f45f88737d
        k    = 00112233445566778899aabbccddeeff
        imsi = 001010123456780
        imei = 353490069873310
        ```

        - Check that the APN (found in the `[nas]` section in `ue.conf` file) matches with the one in the `epc.conf` file (`/root/radio_code/srslte_config/epc.conf`), for example:<br/><br/>

        ```
        apn = colosseum
        ```

        - Configure the RF parameters. DL and UL transmission frequencies need to match those used by the base station (see previous step) and they need to be compatible with the Colosseum scenario in use:<br/><br/>

        ```
        [rf]
        dl_earfcn = 3400
        ul_freq = 1005000000
        dl_freq = 995000000
        freq_offset = 0
        tx_gain = 20
        rx_gain = 20
        
        time_adv_nsamples = 100
        ```


## Start the cellular nodes

1. Start the EPC:
    - In the first terminal of the base station/EPC node:
    - Move into the directory with the EPC configuration files: `cd /root/radio_code/srslte_config`
    - Start the EPC application `srsepc epc.conf`
    - The following should appear upon startup of the EPC:<br/><br/>

    ```
    ---  Software Radio Systems EPC  ---
    
    Reading configuration file epc.conf...
    HSS Initialized.
    MME S11 Initialized
    MME GTP-C Initialized
    MME Initialized. MCC: 0xf001, MNC: 0xff01
    SPGW GTP-U Initialized.
    SPGW S11 Initialized.
    SP-GW Initialized.
    ```
2. Start the base station:
    - In the second terminal of the base station/EPC node:
    - Move into the directory with the base station configuration files: `cd /root/radio_code/srslte_config`
    - Start the base station application: `srsenb enb.conf`
    - Wait a few moments for the base station to start. In general, the following should be displayed upon startup:<br/><br/>

    ```
    Setting manual TX/RX offset to 100 samples
    Setting frequency: DL=995.0 Mhz, UL=1005.0 MHz for cc_idx=0
    
    ==== eNodeB started ===
    Type <t> to view trace
    ```
3. Start the UE:
    - In the terminal of the UE node:
    - Move into the directory with the UE configuration files: `cd /root/radio_code/srslte_config`
    - Start the UE application: `srsue ue.conf`. Feel free to ignore the following error:<br/><br/>

    ```
    Waiting PHY to initialize ... /root/radio_code/srsLTE/srsue/src/phy/sync.cc.632: Error stopping AGC: not implemented
    ```

    - Wait a few moments for the UE to start and connect to the base station. An IP address should be returned if the connection is successful. In the remaining of this assignment, we will assume the UE IP is `172.16.0.2`
    - The following should appear upon startup of the UE:<br/><br/>

    ```
    Attaching UE...
    Setting manual TX/RX offset to 100 samples
    .
    Found Cell:  Mode=FDD, PCI=1, PRB=15, Ports=1, CFO=-0.3 KHz
    Setting manual TX/RX offset to 100 samples
    Found PLMN:  Id=00101, TAC=7
    Random Access Transmission: seq=51, ra-rnti=0x2
    Random Access Complete.     c-rnti=0x46, ta=52
    RRC Connected
    Network attach successful. IP: 172.16.0.2
    Software Radio Systems LTE (srsLTE)
    ```


## Exchange traffic between base station and UE

Now that we have instantiated a cellular network on Colosseum, we will exchange traffic between our base station and user.

1. We will use the `ping` command to confirm reachability of base station and UE:
    - Open an additional terminal on the base station and ping the IP address of the UE: `ping 172.16.0.2`
    - Optionally open a new terminal on the UE and ping the IP address of the base station: `ping 172.16.0.1`
2. We will use the `iperf3` tool to exchange TCP/UDP traffic between base station and UE:
    - On the UE terminal, start an Iperf3 server: `iperf3 -s`
    - On the base station terminal, start a TCP client. This client should connect to the UE: `iperf3 -c 172.16.0.2`
    - You should see TCP packets being exchanged for 10 seconds. Compare the transmit and receive rates at the base station and UE
    - Now do the same but with UDP (parameter `-u` in `iperf3`) and with different source rates (parameter `-b` in `iperf3`) and compare the transmit/receive throughput and packet losses:
        - 0.5 Mbps: `iperf3 -c 172.16.0.2 -u -b 0.5M`
        - 2 Mbps: `iperf3 -c 172.16.0.2 -u -b 2M`
        - 5 Mbps: `iperf3 -c 172.16.0.2 -u -b 5M`
    - Compare your findings with the case in which Iperf3 was run in TCP mode.


## Terminate the running processes and the Colosseum scenario

1. In all terminals with running processes on Colosseum, `Ctrl+C` to stop such processes
2. In one of the terminals, stop the Colosseum scenario: `colosseumcli rf stop`
3. Close all open terminals on Colosseum nodes.


## Optional

- Create a new reservation with three nodes and repeat the previous steps. This time connect 2 UEs to the base station and try exchanging traffic between them.
- Create a reservation with two cellular nodes and two Wi-Fi nodes (see [wifi-assignment](../wifi-assignment/wifi-assignment.md)). Configure the Wi-Fi nodes to exchange traffic on the downlink center frequency used by the cellular nodes, while the base station transmits downlink iperf3 traffic to the users. Observe how Wi-Fi and cellular traffic are impacted by the coexistance of these two technologies.


## Extra

Exemplary configurations for base station, EPC and UE can be found in the [example-configurations](example-configurations) directory of this repository.
