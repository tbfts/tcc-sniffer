### ATTENTION
This project is open so others can learn from it.
I do not feel it is ready for public use, so I don't sugggest using it in the current state.
Many things can change, since I'm still experimenting with ideas.

---

`tcc-sniffer` is a program that collects and decodes network packets relating to Albion Online.
This program does not collect or decode any data that would break the [`game rules`]().

The program will run a simple tcp server @ localhost:9999 by default.
You can find a detailed example using nodejs in [`/example/`]().
This example runs the sniffer in the background, connects to the server, and consumes the data.

[]: # If you want to consume the data as a third-party, please consider building your tool in the [`tcc-client`](). )
[]: # You can head over to [`tcc-extension-template`]() to learn how to do that.

---

## **What do we collect?**<br>
- Chat Messages
- ...

This list will continue to expand as I get to things...

## **How to build the executable?**
I have only used this on Windows. Linux/Mac users will have to figure things out themselves.

Requirements(Windows):
- Install [`NPCAP`]()
- Visual Studio (2019 or 2022)

1. Open the solution file in Visual Studio.
2. Select Build from the menubar, then select Publish.
3. Click the Publish button.

---

## **How do Albion's packets work?**
There are 3 types of packets; Events, Requests, and Responses.

Events are essentially actions that change something.
Requests are asking for something, or just sending something.
Responses are answering a request.

Packets are identified by a special code.
Events have there own Event Codes.
Requests and Responses share Operation Codes.

Every packet is packed like a dictionary or table, or more simple; key/value pairs.
The data in the packet has a key, which is a single byte.
The data itself is given to us as a plain object, which we must convert to the actual value type.

Every packet has a key of 252/253, which tells us the code that identifies the packet.
252 are events
253 are operations
So, if a packet has 252=63, this is the Chat Message event.

Requests and Responses have a special key, 255, that pairs a Response to a Request.
For example, the client sends a Request where 255 = 10.
Eventually the client will receive a Response where 255 = 10.
Also, they both will have the same code on the 253 key.

Finally, Albion uses the Photon Engine for networking. 
Luckily for us, people have already made libraries to decode Photon's packets.
If you're interested in seeing how that works, check out any of these:
- [`C# PhotonPacketParser`]()
- [`Go PhotonSpectator`]()
