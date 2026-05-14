# UDP Packet Capture with Nanosecond Timestamp Precision

## Install Required Packages

### RHEL / CentOS

```bash
yum install -y tcpdump wireshark

Ubuntu / Debian
apt update
apt install -y tcpdump tshark

Sender Server
Capture UDP Packets for 3 Minutes
timeout 3m tcpdump -i p3p2 udp port 8284 -w sender.pcap --time-stamp-precision=nano

watch -n 1 'TZ=Asia/Kolkata tshark -r sender.pcap -T fields -e ip.id -e frame.time_epoch 2>/dev/null | tail | while read id ts; do echo -n "$id "; date -d @"$ts" "+%d-%b-%Y %H:%M:%S.%N IST"; done'

Receiver Server
Capture UDP Packets for 3 Minutes
timeout 3m tcpdump -i ens256 udp port 8284 -w receiver.pcap --time-stamp-precision=nano

watch -n 1 'TZ=Asia/Kolkata tshark -r receiver.pcap -T fields -e ip.id -e frame.time_epoch 2>/dev/null | tail | while read id ts; do echo -n "$id "; date -d @"$ts" "+%d-%b-%Y %H:%M:%S.%N IST"; done'

