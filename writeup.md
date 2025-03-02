# PwnMe 2025 CTF | MISC Writeup
## Mafia at the end of the block 1

**Category: MISC**

**Coins: 500 -> 50**

 **Blockchain** | **Network** | **Easy**

Flagged on: **March 1st, 2025** by **EVO7**

**CTF Problem:**

You're an agent, your unit recently intercepted a mob discussion about an event that's going to take place on August 8, 2024. You already know the location, though. A password for the event was mentioned. Your job is to find it and return it so that an agent can go to the scene and collect evidence.

**Note:** The contract is deployed on sepolia network

**Authors:** wepfen, teazer

**Flag format: PWNME{.........................}**

## Prerequisite

- **Wireshark**: Wireshark basics
- **Network**: Network basics
- **Forensic**: Forensic basics
- **OSINT**: Osint basics

## STEP 1:
First, let's start by downloading the [Mafia_at_the_end_of_the_block_1.zip](https://www.mediafire.com/file/243ehfu5yy6s2d1/Mafia_at_the_end_of_the_block_1.zip/file) which contains a **PCAP** file, and take a look.

![](https://imgur.com/fLj5Imn)

After looking at it, I notice a large number of packets, making it difficult to analyze them manually one by one, it will take too much time.













