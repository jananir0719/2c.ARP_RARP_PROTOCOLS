# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
## CLIENT
```
import socket

s = socket.socket()
s.connect(('localhost', 8000))

reader = s.makefile('r')

while True:
    print("\n1. ARP")
    print("2. RARP")
    print("3. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        ip = input("Enter IP Address: ")
        s.sendall(("ARP|" + ip + "\n").encode())

    elif choice == "2":
        mac = input("Enter MAC Address: ")
        s.sendall(("RARP|" + mac + "\n").encode())

    elif choice == "3":
        break

    else:
        print("Invalid choice")
        continue

    result = reader.readline().strip()
    print("Result:", result)

s.close()
```
## SERVER
```
import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(1)

print("Server is waiting for connection...")

conn, addr = s.accept()
print("Connected:", addr)

ip_mac = {
    "192.168.1.1": "AA:BB:CC:DD:EE:01",
    "192.168.1.2": "AA:BB:CC:DD:EE:02",
    "192.168.1.3": "AA:BB:CC:DD:EE:03"
}

while True:
    data = conn.makefile('r').readline().strip()

    if not data:
        break

    choice, value = data.split("|", 1)

    if choice == "ARP":
        result = ip_mac.get(value, "IP Address not found")
        print("ARP Request:", value)
        print("MAC Address:", result)

    elif choice == "RARP":
        result = "MAC Address not found"

        for ip, mac in ip_mac.items():
            if mac == value:
                result = ip
                break

        print("RARP Request:", value)
        print("IP Address:", result)

    conn.sendall((result + "\n").encode())

conn.close()
s.close()
```

## OUPUT - ARP
<img width="630" height="157" alt="Screenshot 2026-11-02 140102" src="https://github.com/user-attachments/assets/ea8c6611-55e6-4b4f-9902-4caa277db7fc" />

## OUPUT -RARP
<img width="748" height="268" alt="Screenshot 2026-11-02 135751" src="https://github.com/user-attachments/assets/b24aace4-81ae-41df-bd3d-dfb27c1ef616" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
