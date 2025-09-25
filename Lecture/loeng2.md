# Loeng 2: Võrgu Tarkvara ja Protokollid

## 📚 Sissejuhatus

Eelmises loengus vaatasime võrgu riistvara komponente - ruutereid, kommutaatoreid, kaableid. Täna vaatame **tarkvara poolt** - kuidas andmed tegelikult liiguvad ja millised reeglid seda juhivad.

Võrgu riistvara ilma tarkvarata on nagu tühi maja ilma inimesteta - struktuur on olemas, aga midagi ei toimu. Tarkvara paneb võrgu elama.

```mermaid
graph LR
    A[Riistvara<br/>🖥️ Arvutid<br/>🔌 Kaablid<br/>📡 Ruuterid] 
    B[Tarkvara<br/>📜 Protokollid<br/>📦 Paketid<br/>🔄 Marsruutimine]
    C[Toimiv Võrk<br/>🌐 Internet<br/>📧 E-mail<br/>🎮 Mängud]
    
    A -->|+| B
    B -->|=| C
    
    style A fill:#ffebee
    style B fill:#e3f2fd
    style C fill:#e8f5e9
```

---

## 1️⃣ PROTOKOLLIDE HIERARHIA

### 1.1 Mis On Protokoll?

**Protokoll on formaalne reeglite kogum**, mis määrab täpselt, kuidas kaks või enam osapoolt omavahel suhtlevad. See pole lihtsalt tehniline detail - protokollid on kõikjal meie ümber. Diplomaatias on protokollid, mis määravad, kuidas riigipead kohtuvad. Ärimaailmas on protokollid koosolekute pidamiseks.

#### 📱 Näide: WhatsApp Kõne Protokoll

```mermaid
sequenceDiagram
    participant A as Alice 📱
    participant B as Bob 📱
    
    Note over A,B: 1. ÜHENDUSE LOOMINE
    A->>B: 📞 Helistan (INVITE)
    B-->>A: 📱 Telefon heliseb (RINGING)
    B-->>A: ✅ Võtan vastu (OK)
    
    Note over A,B: 2. ANDMETE EDASTUS
    A->>B: 🎤 Hääl (RTP paketid)
    B->>A: 🎤 Hääl (RTP paketid)
    A->>B: 📊 Kvaliteedi info
    
    Note over A,B: 3. ÜHENDUSE LÕPETAMINE
    A->>B: 👋 Lõpetan (BYE)
    B-->>A: ✅ OK (ACK)
```

Võrguprotokollid määravad **kolm kriitilist aspekti**:

| Aspekt | Kirjeldus | Näide |
|--------|-----------|-------|
| **Süntaks** | Andmete formaat ja struktuur | HTTP päring algab "GET" või "POST" |
| **Semantika** | Mida iga väli tähendab | Port 80 = veebiserver |
| **Ajastus** | Millal ja kui kaua | TCP timeout = 3 sekundit |

### 1.2 Miks Protokollid On Kihtidena?

Võrgu tarkvara koosneb **hierarhilistest kihtidest**, kus iga kiht pakub teenuseid ülemisele kihile ja kasutab alumise kihi teenuseid. See pole juhuslik valik - kihtidel on mitu olulist eelist.

```mermaid
graph TB
    subgraph "Kihtide Süsteem"
        A[📱 Rakendus<br/>Gmail, Chrome, Spotify]
        B[🔄 Transport<br/>TCP/UDP - usaldusväärsus]
        C[🌐 Võrk<br/>IP - marsruutimine]
        D[🔗 Andmelink<br/>Ethernet/WiFi - lokaalne]
        E[⚡ Füüsiline<br/>Kaablid, signaalid]
    end
    
    A -->|kasutab| B
    B -->|kasutab| C
    C -->|kasutab| D
    D -->|kasutab| E
    
    style A fill:#e1f5fe
    style E fill:#ffebee
```

**Neli põhilist eelist:**

1. **🎯 Keerukuse juhtimine** - suur probleem jagatakse väiksemateks osadeks
2. **🔒 Abstraheerimine** - iga kiht peidab oma sisemised detailid  
3. **🔄 Sõltumatus** - kihte saab muuta teisi mõjutamata
4. **📏 Standardiseerimine** - igale kihile oma standardid

### 1.3 Filosoofi Näide

Vaatame konkreetset näidet, kuidas kihid töötavad. Kaks filosoofi tahavad arutada, üks räägib prantsuse, teine hollandi keeles.

```mermaid
graph LR
    subgraph "Kenya"
        F1[Filosoof 🇫🇷]
        T1[Tõlk 🇬🇧]
        S1[Sekretär 📠]
    end
    
    subgraph "Indoneesia"
        F2[Filosoof 🇳🇱]
        T2[Tõlk 🇬🇧]
        S2[Sekretär 📠]
    end
    
    F1 -.->|"Ideed"| F2
    T1 -.->|"Inglise keel"| T2
    S1 -.->|"Faks"| S2
    
    F1 -->|prantsuse| T1
    T1 -->|inglise| S1
    S1 -->|bitid| S2
    S2 -->|inglise| T2
    T2 -->|hollandi| F2
```

Filosoof arvab, et räägib otse teise filosoofiga, aga tegelikult info liigub läbi mitme kihi. Iga kiht suhtleb **horisontaalselt** oma vastaskihiga, kuigi füüsiliselt liigub info **vertikaalselt**.

---

## 2️⃣ KIHTIDE DISAINI PÕHIMÕTTED

### 2.1 Kuus Fundamentaalset Probleemi

Võrgu kihtide loomisel tuleb lahendada kuus põhiprobleemi. Need on universaalsed - esinevad igas võrgus, igal kihil.

```mermaid
mindmap
  root((Disaini<br/>Probleemid))
    Adresseerimine
      MAC aadress
      IP aadress
      Port number
    Suund
      Simplex
      Half-duplex
      Full-duplex
    Vigade kontroll
      Avastamine
      Parandamine
      Kordamine
    Järjekord
      Nummerdamine
      Puhverdamine
      Sorteerimine
    Voo kontroll
      Stop-and-wait
      Sliding window
      Credit-based
    Multipleksimine
      TDM
      FDM
      Statistical
```

### 2.2 Adresseerimine Detailselt

Iga võrgukiht vajab oma **adresseerimissüsteemi**, sest igal kihil on erinev ulatus ja ülesanne.

| Kiht | Aadress | Näide | Ulatus | Muutuv? |
|------|---------|-------|--------|---------|
| **Transport** | Port | 443 (HTTPS) | Protsess | Jah |
| **Network** | IP | 192.168.1.100 | Globaalne | Jah |
| **Data Link** | MAC | AA:BB:CC:DD:EE:FF | Lokaalne | Ei |
| **Physical** | - | Broadcast kõigile | Kaabel | - |

```mermaid
graph TB
    subgraph "Aadressi Hierarhia"
        A[🌍 google.com<br/>DNS nimi]
        B[🔢 142.250.74.110<br/>IP aadress]
        C[🔌 00:1A:2B:3C:4D:5E<br/>MAC aadress]
        D[⚡ Elektrisignaalid<br/>Füüsiline]
    end
    
    A -->|DNS tõlge| B
    B -->|ARP tõlge| C
    C -->|Kodeerimine| D
```

### 2.3 Vigade Kontroll

Füüsilised sidevahendid pole kunagi täiuslikud. Vaskkaablites tekitab müra vigu. WiFi-s segavad teised seadmed. Vigade kontroll on kriitiline.

#### Vigade Avastamise Meetodid

```mermaid
graph LR
    A[Andmed: 1101001] --> B{Vigade<br/>Kontroll}
    B --> C[Paarsus<br/>+1 bit]
    B --> D[Checksum<br/>+16 bit]
    B --> E[CRC<br/>+32 bit]
    
    C --> F[Nõrk<br/>50%]
    D --> G[Keskmine<br/>99%]
    E --> H[Tugev<br/>99.99%]
    
    style C fill:#ffebee
    style D fill:#fff9c4
    style E fill:#e8f5e9
```

**Vigade parandamise strateegiad:**

| Meetod | Kirjeldus | Kasutus |
|--------|-----------|---------|
| **ARQ** | Automatic Repeat Request - küsi uuesti | TCP, WiFi |
| **FEC** | Forward Error Correction - paranda ise | Satelliit, CD |
| **Hybrid** | Kombineeri mõlemad | 5G, fiber |

---

## 3️⃣ ÜHENDUSEGA VS ÜHENDUSETA TEENUS

### 3.1 Connection-Oriented (TCP Mudel)

Connection-oriented teenus järgib **telefonikõne mudelit** - enne andmete saatmist luuakse ühendus, andmed saadetakse, siis ühendus suletakse.

```mermaid
stateDiagram-v2
    [*] --> CLOSED: Algus
    CLOSED --> LISTEN: Server start
    CLOSED --> SYN_SENT: Client connect()
    
    LISTEN --> SYN_RCVD: SYN tuleb
    SYN_SENT --> ESTABLISHED: SYN+ACK
    SYN_RCVD --> ESTABLISHED: ACK
    
    ESTABLISHED --> DATA_TRANSFER: Andmed
    DATA_TRANSFER --> DATA_TRANSFER: Send/Receive
    
    DATA_TRANSFER --> FIN_WAIT: Close()
    FIN_WAIT --> CLOSED: FIN+ACK
```

**TCP Three-Way Handshake:**

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    
    Note over C,S: 1. Ühenduse loomine
    C->>S: SYN (seq=100)
    Note right of S: "Keegi tahab ühenduda!"
    S->>C: SYN+ACK (seq=300, ack=101)
    Note left of C: "Server on valmis!"
    C->>S: ACK (ack=301)
    Note over C,S: ✅ Ühendus loodud!
    
    Note over C,S: 2. Andmete edastus
    C->>S: DATA (seq=101, len=50)
    S->>C: ACK (ack=151)
    S->>C: DATA (seq=301, len=30)
    C->>S: ACK (ack=331)
```

### 3.2 Connectionless (UDP Mudel)

Connectionless on nagu **postkaardi saatmine** - kirjutad, adresseerid, postitad. Pole eelnevat kokkulepet.

```mermaid
graph TB
    A[📝 Loo pakett] --> B[📮 Lisa aadress]
    B --> C[📤 Saada]
    C --> D{Jõudis<br/>kohale?}
    D -->|Jah| E[✅ Õnnestumine]
    D -->|Ei| F[❌ Kadumine]
    D -->|Vale järjekord| G[🔄 Segadus]
    
    style E fill:#e8f5e9
    style F fill:#ffebee
    style G fill:#fff9c4
```

### 3.3 Võrdlus

| Omadus | Connection-Oriented (TCP) | Connectionless (UDP) |
|--------|--------------------------|---------------------|
| **Analoogia** | 📞 Telefonikõne | 📮 Postkaart |
| **Seadistus** | Vajalik (3-way handshake) | Pole vaja |
| **Järjekord** | ✅ Garanteeritud | ❌ Pole garanteeritud |
| **Usaldusväärsus** | ✅ Kõrge | ❌ Madal |
| **Kiirus** | 🐢 Aeglasem | 🚀 Kiirem |
| **Overhead** | Suur (20+ baiti) | Väike (8 baiti) |
| **Kasutus** | Failid, veeb, e-mail | Video, mängud, DNS |

---

## 4️⃣ TEENUSE PRIMITIIVID

### 4.1 Viis Põhilist Operatsiooni

Teenuse primitiivid on elementaarsed operatsioonid, mida kasutajad saavad teenusega teha. Need on nagu LEGO klotsid - lihtsad põhiosad.

```mermaid
graph TB
    subgraph "Server Operations"
        L[LISTEN<br/>🎧 Oota ühendusi]
        A[ACCEPT<br/>✅ Võta vastu]
    end
    
    subgraph "Client Operations"
        C[CONNECT<br/>🔌 Loo ühendus]
    end
    
    subgraph "Both"
        S[SEND<br/>📤 Saada andmed]
        R[RECEIVE<br/>📥 Võta vastu]
        D[DISCONNECT<br/>🔌 Sulge]
    end
    
    L --> A
    C --> S
    S --> R
    R --> D
```

### 4.2 Berkeley Sockets

Berkeley sockets on de facto standard võrguprogrammeerimiseks. Loodud 1983 BSD Unix'is, nüüd kõikjal.

#### Server Voog

```python
# Pseudo-kood näide
server = socket()           # 1. Loo socket
server.bind(port=80)        # 2. Määra port
server.listen(queue=5)      # 3. Hakka kuulama
while True:
    client = server.accept()  # 4. Võta ühendus
    data = client.recv()      # 5. Loe andmed
    client.send(response)     # 6. Saada vastus
    client.close()           # 7. Sulge ühendus
```

---

## 5️⃣ OSI VÕRDLUSMUDEL

### 5.1 Seitse Kihti

OSI (Open Systems Interconnection) mudel loodi ISO poolt 1983. See on **teoreetiline mudel** võrkude mõistmiseks.

```mermaid
graph TB
    subgraph "OSI 7 Layers"
        A[7. Application<br/>📱 HTTP, FTP, SMTP]
        B[6. Presentation<br/>🔐 SSL, JPEG, ASCII]
        C[5. Session<br/>🔄 SQL, RPC]
        D[4. Transport<br/>📦 TCP, UDP]
        E[3. Network<br/>🌐 IP, ICMP, ARP]
        F[2. Data Link<br/>🔗 Ethernet, WiFi]
        G[1. Physical<br/>⚡ Kaablid, signaalid]
    end
    
    A --> B --> C --> D --> E --> F --> G
    
    style A fill:#e3f2fd
    style D fill:#fff9c4
    style G fill:#ffebee
```

### 5.2 Iga Kihi Ülesanded

| Kiht | Nimi | Põhiülesanne | Näide |
|------|------|--------------|-------|
| **7** | Application | Kasutaja teenused | Veebileht (HTTP) |
| **6** | Presentation | Andmete vormindamine | Krüpteerimine (SSL) |
| **5** | Session | Dialoogide haldamine | SQL seanss |
| **4** | Transport | End-to-end ühendus | TCP segment |
| **3** | Network | Marsruutimine | IP pakett |
| **2** | Data Link | Vigadeta edastus | Ethernet kaader |
| **1** | Physical | Bitid juhtmetel | Elektrisignaalid |

### 5.3 Kuidas Andmed Liiguvad

```mermaid
sequenceDiagram
    participant A as Application
    participant P as Presentation  
    participant S as Session
    participant T as Transport
    participant N as Network
    participant D as Data Link
    participant Ph as Physical
    
    Note over A: User data
    A->>P: DATA
    Note over P: +Encryption
    P->>S: Encrypted DATA
    Note over S: +Session ID
    S->>T: Session + DATA
    Note over T: +Port numbers
    T->>N: TCP segment
    Note over N: +IP addresses
    N->>D: IP packet
    Note over D: +MAC addresses
    D->>Ph: Frame
    Note over Ph: Convert to bits
    Ph-->>Ph: 01010101...
```

---

## 6️⃣ TCP/IP VÕRDLUSMUDEL

### 6.1 Praktiline 4-Kihiline Mudel

TCP/IP on **Internet'i tegelik mudel**. Loodud ARPANET'i jaoks, nüüd globaalne standard.

```mermaid
graph LR
    subgraph "OSI Model"
        direction TB
        O1[7. Application]
        O2[6. Presentation]
        O3[5. Session]
        O4[4. Transport]
        O5[3. Network]
        O6[2. Data Link]
        O7[1. Physical]
    end
    
    subgraph "TCP/IP Model"
        direction TB
        T1[4. Application]
        T2[3. Transport]
        T3[2. Internet]
        T4[1. Network Access]
    end
    
    O1 --> T1
    O2 --> T1
    O3 --> T1
    O4 --> T2
    O5 --> T3
    O6 --> T4
    O7 --> T4
    
    style T1 fill:#e3f2fd
    style T2 fill:#fff9c4
    style T3 fill:#e8f5e9
    style T4 fill:#ffebee
```

### 6.2 TCP/IP Protokollid

| Kiht | Protokollid | Funktsioon |
|------|------------|------------|
| **Application** | HTTP, FTP, SMTP, DNS | Kasutaja teenused |
| **Transport** | TCP, UDP | Usaldusväärsus |
| **Internet** | IP, ICMP, ARP | Marsruutimine |
| **Network Access** | Ethernet, WiFi, PPP | Füüsiline edastus |

### 6.3 OSI vs TCP/IP

```mermaid
graph TB
    subgraph "Võrdlus"
        A[OSI]
        B[TCP/IP]
    end
    
    A --> C[✅ Selge struktuur<br/>✅ Hea õppimiseks<br/>❌ Liiga keeruline<br/>❌ Vähe kasutust]
    B --> D[✅ Praktiline<br/>✅ Laialdaselt kasutusel<br/>❌ Hägused piirid<br/>❌ Raske õpetada]
```

---

## 7️⃣ NÄIDISVÕRGUD

### 7.1 Internet Struktuur

Internet on **võrkude võrk** - hierarhiline struktuur ilma keskse kontrolli.

```mermaid
graph TB
    subgraph "Internet Hierarhia"
        A[🏠 Kodu/Kontor]
        B[🏢 ISP<br/>Telia, Elisa]
        C[🌍 Regional Network]
        D[🌐 Tier-1 Backbone<br/>AT&T, NTT]
        E[☁️ Content<br/>Google, Netflix]
    end
    
    A -->|Last mile| B
    B -->|Regional| C
    C -->|National| D
    D -->|Peering| E
    
    style A fill:#e8f5e9
    style D fill:#fff9c4
```

### 7.2 Mobiilvõrkude Evolutsioon

```mermaid
timeline
    title Mobiilside Põlvkonnad
    
    1980 : 1G
         : Analoog hääl
         : 2.4 kbps
         
    1991 : 2G (GSM)
         : Digitaalne
         : SMS
         : 64 kbps
         
    2001 : 3G (UMTS)
         : Mobiilne internet
         : Video kõned
         : 2 Mbps
         
    2010 : 4G (LTE)
         : All-IP
         : HD video
         : 100 Mbps
         
    2020 : 5G
         : Ultra-kiire
         : IoT
         : 10 Gbps
```

### 7.3 WiFi Standardid

| Standard | Aasta | Sagedus | Max kiirus | Uus nimi |
|----------|-------|---------|------------|----------|
| 802.11b | 1999 | 2.4 GHz | 11 Mbps | - |
| 802.11g | 2003 | 2.4 GHz | 54 Mbps | - |
| 802.11n | 2009 | 2.4/5 GHz | 600 Mbps | WiFi 4 |
| 802.11ac | 2013 | 5 GHz | 3.5 Gbps | WiFi 5 |
| 802.11ax | 2019 | 2.4/5/6 GHz | 9.6 Gbps | WiFi 6 |
| 802.11be | 2024 | 2.4/5/6 GHz | 46 Gbps | WiFi 7 |

---

## 8️⃣ STANDARDISEERIMINE

### 8.1 Miks Standardid On Kriitilised

Ilma standarditeta poleks globaalset internetti. Iga tootja teeks oma protokollid. Seadmed ei ühilduks.

```mermaid
graph LR
    subgraph "Ilma Standarditeta"
        A1[Apple WiFi]
        A2[Samsung WiFi]
        A3[Huawei WiFi]
        A1 -.❌.- A2
        A2 -.❌.- A3
        A1 -.❌.- A3
    end
    
    subgraph "Standarditega (802.11)"
        B1[Apple WiFi]
        B2[Samsung WiFi]
        B3[Huawei WiFi]
        B1 -.✅.- B2
        B2 -.✅.- B3
        B1 -.✅.- B3
    end
```

### 8.2 Peamised Organisatsioonid

| Org | Nimi | Vastutus | Standardid |
|-----|------|----------|------------|
| **ITU** | International Telecom Union | Telekom | G.xxx, H.264 |
| **ISO** | Int'l Standards Organization | Üldine | OSI mudel |
| **IEEE** | Inst of Electrical Engineers | LAN/MAN | 802.3, 802.11 |
| **IETF** | Internet Engineering TF | Internet | RFC (TCP/IP) |
| **W3C** | World Wide Web Consortium | Veeb | HTML, CSS |

---

## 9️⃣ MÕÕTÜHIKUD

### 9.1 Biti vs Baidi Segadus

> ⚠️ **1 bait (B) = 8 bitti (b)**

```mermaid
graph TB
    subgraph "Võrgu Kiirus (bittides)"
        A[100 Mbps internet]
        B[= 100,000,000 bits/sec]
        C[= 12.5 MB/sec]
    end
    
    subgraph "Faili Suurus (baitides)"
        D[1 GB film]
        E[= 1,024 MB]
        F[= 8,192 Mb]
    end
    
    A --> B --> C
    D --> E --> F
    
    C -.->|"80 sekundit<br/>100 Mbps"| D
```

### 9.2 Praktiline Arvutus

**Näide: Kui kaua võtab 5 GB faili allalaadimine?**

```python
# Andmed
faili_suurus = 5 * 1024 * 1024 * 1024 * 8  # bits
kiirus = 100 * 1000 * 1000  # 100 Mbps

# Arvutus
aeg = faili_suurus / kiirus
# = 42,949,672,960 / 100,000,000
# = 429 sekundit
# = 7 minutit 9 sekundit

# Praktikas: lisa 10-15% overhead
tegelik_aeg = 429 * 1.15 = 493 sekundit
```

---

## 📊 Kokkuvõte

### Peamised Õpitud Kontseptsioonid

```mermaid
mindmap
  root((Võrgu<br/>Tarkvara))
    Protokollid
      Reeglid
      Kihtidena
      Standardid
    Mudelid
      OSI (7)
      TCP/IP (4)
      Praktiline (5)
    Teenused
      Connection-oriented
      Connectionless
      Primitiivid
    Võrgud
      Internet
      Mobiil
      WiFi
```

### 🎯 Mida Edasi?

- **Järgmine loeng:** Füüsiline kiht detailselt
- **Praktikum:** Packet Tracer labor
- **Kodutöö:** OSI vs TCP/IP võrdlus

### 📚 Lisalugemist

- RFC 793 - TCP Protocol
- IEEE 802.11 - WiFi Standard
- Tanenbaum Ch 2 - Physical Layer

---

## ✅ Kontrolli Oma Teadmisi

1. **Miks on protokollid kihtidena?**
2. **Mis vahe on TCP ja UDP vahel?**
3. **Mitu kihti on OSI mudelis? TCP/IP mudelis?**
4. **Millal kasutada connection-oriented teenust?**
5. **Kui suur on 1 Gbps MB/s-des?**

### 📺 Vaata Videot

[![OSI Model Explained](https://img.youtube.com/vi/vv4y_uOneC0/maxresdefault.jpg)](https://www.youtube.com/watch?v=vv4y_uOneC0)
*Kliki pildil, et vaadata OSI mudeli selgitust*
