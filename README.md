from PIL import Image
img = Image.open("download.jpg").convert("RGB")
pixels = img.load()

message = input("enter the message to be hidden")
msg = message + "#"  

bits = ''.join(format(ord(c), '08b') for c in msg)

i = 0
for y in range(img.height):
    for x in range(img.width):
        if i < len(bits):
            r, g, b = pixels[x, y]
            pixels[x, y] = ((r & ~1) | int(bits[i]), g, b)
            i += 1

img.save("stego.png")
print("Message hidden!")
-------------------------------------------------------------------------------------
img = Image.open("stego.png")
pixels = img.load()

bits = ""
for y in range(img.height):
    for x in range(img.width):
        bits += str(pixels[x, y][0] & 1)

msg = ""
for i in range(0, len(bits), 8):
    msg += chr(int(bits[i:i+8], 2))
    if msg.endswith("#"):
        break

print("Hidden message:", msg[:-1])



## TCPDUMP
sudo tcpdump -D
sudo tcpdump -i enp0s3
sudo tcpdump -i enp0s3 host <ip or dns>
sudo tcpdump -i enp0s3 port 80 
sudo tcpdump -i enp0s3 port 443
sudo tcpdump -i enp0s3 -w file.pcap
sudo tcpdump -i enp0s3 -a -r file.pcap
sudo tcpdump -i enp0s3 -xx -r file.pcap

sudo tcpdump -i enp0s3 icmp
sudo tcpdump -i enp0s3 tcp
sudo tcpdump -i enp0s3 'tcp[tcpflags] & tcp-syn != 0' -w file1.pcap
sudo tcpdump -a -r file.pcap
sudo tcpdump -i enp0s3 'tcp[tcpflags] & tcp-ack != 0' -w file1.pcap
sudo tcpdump -a -r file.pcap
sudo tcpdump -i enp0s3 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-syn != 0' -w file1.pcap
sudo tcpdump -a -r file.pcap

Use 2 terminal 1 for ping and other to capture packets
(IMP note:To capture http and http use curl http://example.com and https://example.com respectively.Also for tcp 3 way handshake use curl google.com or curl http://example.com )
( For icmp and others use ping google.com)

for pure flags use 

sudo tcpdump -i enp0s3 'tcp[tcpflags] == tcp-ack' -w file1.pcap
-----------------------------------------------------------

## Text encryption and decryption using cbc and ecb

touch plain.txt 
nano plain.txt
openssl enc -aes-128-cbc -e -in plain.txt -out cipher1.bin -K 00112233445566778899aabbccddeeff -iv 0102030405060708090a0b0c0d0e0f 
openssl enc -aes-128-cbc -d -in cipher1.bin -out output1.txt -K 00112233445566778899aabbccddeeff -iv 0102030405060708090a0b0c0d0e0f 
xxd cipher1.bin 
cat output1.txt 

openssl enc -aes-128-ecb -e -in plain.txt -out cipher2.bin -K 00112233445566778899aabbccddeeff 
openssl enc -aes-128-ecb -d -in cipher2.bin -out output2.txt -K 00112233445566778899aabbccddeeff 
xxd cipher2.bin 
cat output2.txt

-----------------------------------------------------------

## Image(Bitmap or bmp) encryption using ecb and cbc

openssl enc -aes-128-ecb -e -in pic_original.bmp -out pic_ecb.bmp -K 0123456789abcdeffedcba9876543210 
openssl enc -aes-128-cbc -e -in pic_original.bmp -out pic_cbc.bmp -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 

head -c 54 pic_original.bmp > header 

tail -c +55 pic_ecb.bmp > body_ecb 
cat header body_ecb > new_ecb.bmp 
tail -c +55 pic_cbc.bmp > body_cbc 
cat header body_cbc > new_cbc.bmp 
eog new_ecb.bmp 
eog new_cbc.bmp 

---------------------------------------------------------------

## Padding experiment 

echo -n 12345 > f1.txt 
echo -n 1234567890 > f2.txt 
echo -n 1234567890abcdef > f3.txt 

ls -l f*.txt 

openssl enc -aes-128-cbc -e -in f1.txt -out f1.bin -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f1.bin -out f1_dec.txt -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f1.bin -out f1_nopad.txt -nopad -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 

openssl enc -aes-128-cbc -e -in f2.txt -out f2.bin -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f2.bin -out f2_dec.txt -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f2.bin -out f2_nopad.txt -nopad -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 

openssl enc -aes-128-cbc -e -in f3.txt -out f3.bin -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f3.bin -out f3_dec.txt -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 
openssl enc -aes-128-cbc -d -in f3.bin -out f3_nopad.txt -nopad -K 0123456789abcdeffedcba9876543210 -iv 00112233445566778899aabbccddeeff 

ls -l f*.bin 

xxd f1.txt 
xxd f1_nopad.txt 
xxd f2.txt
xxd f2_nopad.txt
xxd f3.txt
xxd f3_nopad.txt

---------------------------------------------------------------

## Error propagation

touch plain.txt 
nano plain.txt
ls -l plain.txt
 
openssl enc -aes-128-ecb -e -in plain.txt -out cipher1.bin -k 00112233445566778899aabbccddeeff 
ghex cipher1.bin 
openssl enc -aes-128-ecb -d -in cipher1.bin -out cipher1.txt -k 00112233445566778899aabbccddeeff 
gedit cipher2.txt
 
openssl enc -aes-128-cbc -e -in plain.txt -out cipher2.bin -k 00112233445566778899aabbccddeeff -iv 0102030405060708090a0b0c0d0e0f 
ghex cipher2.bin 
openssl enc -aes-128-cbc -d -in cipher2.bin -out cipher2.txt -k 00112233445566778899aabbccddeeff -iv 0102030405060708090a0b0c0d0e0f 
gedit cipher2.txt

if u get bad magic number or bad decrypt use 

openssl enc -aes-128-cbc -e -in plain.txt -out cipher.bin -K 0011223344556677889900aabbccddeeff -iv 0102030405060708090a0b0c0d0e0f10 -nosalt

----------------------------------------------------------------

## RSA

openssl genrsa -out key.pri 2048 
cat key.pri 

openssl rsa -in key.pri -noout -text 
openssl rsa -in key.pri -out key.pub -pubout 
cat key.pub 

touch Earth.txt 
gedit Earth.txt 
openssl rsautl -encrypt -inkey key.pub -pubin -in Earth.txt -out Earth.enc 
openssl rsautl -decrypt -inkey key.pri -in Earth.enc -out Earth.dec 
cat Earth.dec 


openssl rand -hex 32 > encryption.key 
openssl rsautl -encrypt -inkey key.pub -pubin -in encryption.key -out encryption.key.enc 
openssl rsautl -decrypt -inkey key.pri -in encryption.key.enc -out encryption.key.dec 
cat encryption.key 
cat encryption.key.dec 

============================================================================================================================
5b)
from scapy.all import * 
packet=IP()/TCP() 
e_pack=Ether()/packet 
e_pack.show()

6b)
from scapy.all import * 
packets=sniff(count=10) 
packets.summary() 
print(packets[0].show()) 
print(packets[1].show())

7A)
from scapy.all import * 
result,unanswered=traceroute(["www.example.com","www.google.com"],maxttl=20) 
result.show()


8A)
import scapy.all as scapy
request = scapy.ARP()
request.pdst = "172.29.240.0/28"

broadcast = scapy.Ether()
broadcast.dst = "ff:ff:ff:ff:ff:ff"

request_broadcast = broadcast / request

clients = scapy.srp(request_broadcast, timeout=2)[0]
for element in clients:
    print(element[1].psrc + " - " + element[1].hwsrc)

11a)
from scapy.all import * 
packet=IP(dst="8.8.8.8")/ICMP() 
send(packet)


12a) 
from scapy.all import *
target = "www.google.com"
for port in range(70, 85):
    packet = IP(dst=target) / TCP(dport=port, flags="S")
    response = sr1(packet, timeout=1, verbose=0)
    if response:
        print("Port", port, "is open")


14a)
from scapy.all import *
def callback(packet):
    if packet.haslayer(ICMP):
        print("ICMP Packet Detected")
        packet.show()

sniff(filter="icmp", prn=callback, count=10)




15a)
import scapy.all as scapy
request = scapy.ARP()
request.pdst = "192.168.5.0/24"
broadcast = scapy.Ether()
broadcast.dst = "ff:ff:ff:ff:ff:ff"
request_broadcast = broadcast / request
clients = scapy.srp(request_broadcast, timeout=1)[0]
for element in clients:
    print(element[1].psrc + " - " + element[1].hwsrc)


16a)
from scapy.all import * result,unanswered=traceroute(["www.example.com","www.google.com"],maxttl=20) 
result.show()
=================================================================================

C)
xx' IN(itemnum,sdesc,ldesc) UNION SELECT email,passwd,123,123 FROM userdb LIMIT 2 --

''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
c)
x' union select count(itemnum),count(itemnum),count(itemnum),count(itemnum) from itemdb -- 

===============================================================================
5c)
code-

#!/bin/bash
read -p "Enter the starting port number: " start_port
read -p "Enter the ending port number: " end_port
for (( port=start_port; port<=end_port; port++ ))
do
        nc -zv localhost $port
done

terminal---
nano port_scan.sh
chmod +x port_scan.sh 
./port_scan.sh
-----------------------------------------
6c)
nc -vlp 1234                            nc localhost 1234
nc -vlp 1234				nc <ip addr> 1234
---------------------------------------------------------------
7b)
nc www.example.com 80			
HEAD / HTTP/1.1
Host: example.com

nc www.example.com 80
GET / HTTP/1.1
Host: example.com
-----------------------------------------------------------------
8b)
nc -l -p 5000
browser-- localhost:5000 return to cmd
HTTP/1.1 200 OK
Content-Type: text/html;
(double enter)
hloooo bnm
browse for output
-----------------------------------------------------------------
11b)
nc -l -p 5000
browser-- localhost:5000 return to cmd
HTTP/1.1 200 OK
Content-Type: text/html;
(double enter)
<!doctype>
<title>appp</title>
<h1>apppp</h1>
<img src="image location">
(double enter)
ctrl c referesh browser

====================================================================================================================
