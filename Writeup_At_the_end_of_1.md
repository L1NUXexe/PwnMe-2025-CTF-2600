# PwnMe 2025 CTF | MISC Writeup
## Mafia at the end of the block 1
![enter image description here](https://i.imgur.com/NMeGfPL.png)

**Category: MISC**

**Coins: 500 -> 50**

 **Blockchain** | **Network** | **Easy**

Flagged on: **March 1st, 2025** by **EVO7**

**CTF Problem:**

You're an agent, your unit recently intercepted a mob discussion about an event that's going to take place on August 8, 2024. You already know the location, though. A password for the event was mentioned. Your job is to find it and return it so that an agent can go to the scene and collect evidence.

>**Note:** The contract is deployed on sepolia network

**Authors:** **[wepfen](https://github.com/wepfen) , [teazer](https://github.com/LeTeazer)**

**Flag format: PWNME{.........................}**

## Prerequisite

- **Wireshark**: Wireshark basics
- **Network**: Network basics
- **Forensic**: Forensic basics
- **OSINT**: Osint basics

## STEP 1: Analyzing the PCAP
First, let's start by downloading the [Mafia_at_the_end_of_the_block_1.zip](https://www.mediafire.com/file/243ehfu5yy6s2d1/Mafia_at_the_end_of_the_block_1.zip/file) which contains a **PCAP** file, and take a look.

![](https://i.imgur.com/fLj5Imn.png)

After looking at it, I notice a large number of packets, making it difficult to analyze them manually one by one, it will take too much time.

Since the subject states that our unit has recently intercepted a mob discussion, I decide to filter the packets using **IRC (Internet Relay Chat)** to **identify and analyze the exchanged messages.** 

So I type: 
```
irc
```
in wireshark and here's what i get:

![](https://i.imgur.com/Lez4vxK.png)

As you can see, we have all of the **IRC protocol packets**. So i start taking a look at **PRIVMSG Responses**, until i find Something interesting:

![](https://i.imgur.com/8sfqKDA.png)

Here I can see a **message** sent by Bob:

![](https://i.imgur.com/UPMNbjj.png)

But nothing interesting here, so i keep looking at other packets and come across the **packet No. 452**, which seems to contain an interesting information:

![](https://i.imgur.com/r1cTbVM.png)

As you can see here, in the **packet No. 452**, we have a response from Bob42!

![](https://i.imgur.com/0A08WJi.png)

By analyzing the response, we can observe what's called a **"Telegram"** shortened link, which will be used for them to cummunicate.

To copy the message and the link:  **Right click on the response**, then **click on copy**, and finally **click on value**.

![](https://i.imgur.com/qpybEg2.png)

**This is what you will get:**
```
:Bob42!~Bob@47-252-8-177.pwn.unpawnables.me PRIVMSG #DarkNetMafia :Okay, so let's meet here in two days at the same time to finalize the strategy. Then we'll move on to Telegram, here if you missed it : https://shorturl.at/2O8nI
```
![](https://i.imgur.com/ekDHmql.png)

Opening  https://shorturl.at/2O8nI will redirect us to: 

https://www.swisstransfer.com/d/a4947af6-05c6-4011-958e-fd6b604587d1

![](https://i.imgur.com/gYubdnF.png)


There we will find a **[ChatExport.zip](https://www.mediafire.com/file/r0m4ktj5sama41i/ChatExport.zip/file)** folder.

## STEP 2: Alayzing ChatExport.zip

I download and unzip the **[ChatExport.zip](https://www.mediafire.com/file/r0m4ktj5sama41i/ChatExport.zip/file)** folder and take a look at it.

When opening it I can see that there are four folders and one **messages.html** file, so I click on it to see.

![](https://i.imgur.com/nffSBo6.png)

When opening **messages.html** I can observe some **conversations**, in which Bob Rajkwoski has sent a **DarknetMafia.abi** file:

![](https://i.imgur.com/p9lKrLE.png)

![](https://i.imgur.com/c51IPKE.png)

![](https://i.imgur.com/GA0iPJY.png)

So I open the **DarknetMafia.abi** file, and see the Following:

```
[
	{
		"inputs": [
			{
				"internalType": "string",
				"name": "_secret",
				"type": "string"
			}
		],
		"name": "set_secret",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [],
		"name": "ask_secret",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
]
```
I see some interesting things such as : **_secret** , **set_secret**, **ask_secret** but no visible flag.

> Remember that at the beginning, in the CTF's subject we have the
> following note : The contract is deployed on sepolia network.

So **I decide to look for some smart contract adresses in wireshark** by typing: 

```
frame contains "0x" and frame matches "(?i)0x[a-fA-F0-9]{40}"
```
![](https://i.imgur.com/AD2A2JI.png)

I can then see **three packets**, I click on the first packet and take a look:

![](https://i.imgur.com/Tm6X6oe.png)

I see a response from Bob42! that contains the address.

> Response from Bob:

```
:Bob42!~Bob@47-252-8-177.pwn.unpawnables.me PRIVMSG #DarkNetMafia :All right, no delay please. Here's the address : 0xCAB0b02864288a9cFbbd3d004570FEdE2faad8F8.
```
So the address is: **0xCAB0b02864288a9cFbbd3d004570FEdE2faad8F8**.

> I don't examine the other packets which also contain addresses because
> I am certain this is the packet that contains the address we need. In the
> chat we saw previously, Bob mentioned, "Alright, we intercepted some 
> key information regarding the ERC20 token we discussed. We have the 
> ABI of the token." He then sent the ABI file in the chat, confirming its relevance.

## STEP 3: Searching the address on Sepolia Network

What I do next is go to **[Sepolia Network](https://sepolia.etherscan.io/)** **Blockchain Explorer** and search the address: `0xCAB0b02864288a9cFbbd3d004570FEdE2faad8F8`

![](https://i.imgur.com/buEjixM.png)

I start scrolling to find some info:

![](https://i.imgur.com/i5UOXaC.png)

I can see **two transactions**:

![](https://i.imgur.com/9tUBUwh.png)

I click on the first transaction hash, and take a look:

![](https://i.imgur.com/ts5TR5T.png)

I then scroll down and click on show more:

![](https://i.imgur.com/aKoTaEQ.png)

![](https://i.imgur.com/YdDfG0H.png)

But as you can see I find Nothing interesting here.

So I look at the secoond transaction:

![](https://i.imgur.com/xQ3lqWM.png)

I scroll down and click on show more again:

![](https://i.imgur.com/ZgXquJv.png)

And here's where I see Something interesting:

![](https://i.imgur.com/O3ox4FQ.png)

At first glance, there didn't seem to be any flag, but when I switched the view to UTF-8 encoding, the flag became visible! :

![](https://i.imgur.com/fz7PaF5.png)

And there you go ! :

![](https://i.imgur.com/CrX5qXH.png)

**Here's the flag:**
```
FLAG: PWNME{1ls_0nt_t0vt_Sh4ke_dz8a4q6}
```
![](https://i.imgur.com/XJE3Tx2.png)























