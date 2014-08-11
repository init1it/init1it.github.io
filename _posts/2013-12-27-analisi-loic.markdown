---
layout: post
status: publish
published: true
title: Analisi LOIC
author:
  display_name: rom3ocrash
  login: rom3ocrash
  email: mungiu.romeo@gmail.com
  url: ''
author_login: rom3ocrash
author_email: mungiu.romeo@gmail.com
wordpress_id: 48
wordpress_url: http://blog.init1.it/?p=48
date: '2013-12-27 12:57:46 +0100'
date_gmt: '2013-12-27 11:57:46 +0100'
categories:
- Programmazione
- Sicurezza
tags:
- LOIC
- DDoS
- IRC
- Python
---
<p>Negli ultimi anni si è sempre piu sentito parlare di attacchi DDOS. Molti di questi attacchi sono stati collegati al gruppo di hacktivisti Anonymous, questi presunti hacker hanno scaricato e utilizzato il Low Orbit Ion Cannon un toolkit per il distributed denial of service.</p>
<p>LOIC sommerge un bersaglio con grandi quantità di traffico UDP e TCP. Vista la grande quantità di risorse che hanno i sistemi attuali una singola istanza di LOIC farebbe ben poco, tuttavia, quando centinaia di migliaia di persone usano LOIC contemporaneamente, si esauriscono rapidamente le risorse del bersaglio.</p>
<p>LOIC offre due modalità di funzionamento. Nel primo caso l’utente inserisce semplicemente l’indirizzo della vittima e fa partire l’attacco. Nella seconda modalità, soprannominata HIVEMIND, gli utenti connettono LOIC ad un server IRC in cui gli utenti possono assegnare un obiettivo che tutti gli altri utenti connessi attaccheranno automaticamente.</p>
<p>Vista la diffusione su larga scala di questo software tra gli utenti meno esperti sorse la domanda:”Si rischi di essere arrestati usando questo programma?” la risposta di Anonymous fu: “Le probabilità sono prossimi allo zero. Basta dare la colpa hai un virus.” affermazione con la quale sono molto in disaccordo ed in questo articolo vi mostrerò con poche righe di codice (python) come è possibile verificare se all'interno della propria rete qualcuno sta scaricando/utilizzando LOIC per scopi illeciti.</p>
<p>Come prima cosa il presunto hacker dovrà entrare in possesso del software. Sapendo che la fonte principale da cui viene scaricato LOIC è sourceforge, quindi tramite traffico HTTP, ci basterà un piccolo script in python che analizzi il traffico HTTP alla ricerca di HTTP GET per LOIC:</p>
<pre lang="Python" line="20">import dpkt
import socket

def findDownload(pcap):
    for (ts, buf) in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            tcp = ip.data
            http = dpkt.http.Request(tcp.data)
            if http.method == 'GET':
                uri = http.uri.lower()
                if '.zip' in uri and 'loic' in uri:
                    print '[!] ' + src + ' Downloaded LOIC.'
        except:
        pass
f = open()
pcap = dpkt.pcap.Reader(f)
findDownload(pcap)</pre>
<h6><em>Nell'esempio sovrastante e negli esempi che seguiranno viene utilizzata la libreria dpkt disponibile a questo <a href="https://code.google.com/p/dpkt/">link</a>.</em></h6>
<p>Questo può aiutare un amministratore intelligente a dimostrare che un utente ha scaricato LOIC invece di essere infettato da un virus ma a questo punto qualcuno dirà “Non è illegale scaricare un programma” e questo è vero ma connettersi al Anonymous HIVE e lanciare un attacco DDOS è illegale e visto che con LOIC non ce bisogno di nessuna autorizzazione e chiunque può lanciare un attacco con una semplice riga di codice non ci vorrà molto.</p>
<p>Ora il nostro caro amministratore dovrà trovare un modo per sapere se qualcuno ha lanciato un attacco. Per fare questo prima però analizziamo più il dettagli come funziona l’attacco. Per attaccare un bersaglio un membro Anonymous deve connettersi ad un server IRC ed richiedere un attacco con un commando tipo: "<em>!lazor targetip=123.456.78.90 message=i’m_pro_lamer port=80 method=tcp wait=false random=true start</em>" e tutti i membri Anonimous connessi al server IRC con LOIC in modalità HIVEMIND attaccheranno il bersaglio 123.234.56.78.</p>
<p>Quindi al nostro admin non resta che analizzare il traffico sulla porta 6667, che è la porta usata dai server irc, alla ricerca della stringa !lazor... e naturalmente può verificare sia se qualcuno sta lanciando l'attacco, verificando il traffico in uscita sia se ce qualche utente con LOIC in HIVEMIND verificando il traffico in entrata.</p>
<pre lang="Python" line="23">import dpkt
import socket

def findHivemind(pcap):
    for (ts, buf) in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            dst = socket.inet_ntoa(ip.dst)
            tcp = ip.data
            dport = tcp.dport
            sport = tcp.sport
            if dport == 6667:
                if '!lazor' in tcp.data.lower():
                    print '[!] DDoS Hivemind issued by: '+src
                    print '[+] Target CMD: ' + tcp.data
            if sport == 6667:
                if '!lazor' in tcp.data.lower():
                    print '[!] DDoS Hivemind issued to: '+src
                    print '[+] Target CMD: ' + tcp.data
        except:
           pass</pre>
<p>E a questo punto mi sembra chiaro che non si può dare la colpa ad un virus. Ma se il nostro admin non si accontenta è vuole sapere se in questo momento sta avvenendo un attacco DDoS? Niente di più facile basta ricordarsi che durante un attacco DDoS il nostro hacker spara una massiccia quantità di pacchetti TCP verso un bersaglio. Questi pacchetti, in combinazione con i pacchetti degli utenti connessi in HIVEMIND, in sostanza, esauriscono le risorse del bersaglio. Avviando tcpdump in uno di questi momenti ci troveremmo d'avanti una cosa di questo tipo:</p>
<p><code>analyst# tcpdump –i eth0 'port 80'<br />
06:39:26.090870 IP loic-attacker.1182 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
64240, length 12<br />
06:39:26.090976 IP loic-attacker.1186 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
64240, length 12<br />
06:39:26.090981 IP loic-attacker.1185 &gt;loic-target.www: Flags [P.], seq<br />
301:313, ack 1, win<br />
64240, length 12<br />
06:39:26.091036 IP loic-target.www &gt; loic-attacker.1185: Flags [.], ack<br />
313, win 14600, lengt<br />
h 0<br />
06:39:26.091134 IP loic-attacker.1189 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
64240, length 12<br />
06:39:26.091140 IP loic-attacker.1181 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
64240, length 12<br />
06:39:26.091142 IP loic-attacker.1180 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
64240, length 12<br />
06:39:26.091225 IP loic-attacker.1184 &gt;loic-target.www: Flags [P.], seq<br />
336:348, ack 1, win<br />
</code></p>
<p>Per chi non fosse molto pratico di tcpdump gli basta sapere che ogni 0.00005 secondi vengono inviati pacchetti TCP di piccole dimensione (appunto length 12) fino a quando l'attacco termina.</p>
<p>Per rilevare un attacco, dovremmo impostare una soglia di pacchetti. Se il numero di pacchetti inviati da un utente a un indirizzo specifico supera questa soglia, puo indicare qualcosa su cui indagare. Indagare perché comunque non si può essere sicuri che si tratti di un attacco DDoS, tuttavia, correlando ad un utente che ha scaricare LOIC, seguita da un accettazione di un comando HIVE, seguito dall'attacco effettivo, forniscono prove schiaccianti per dimostrare che un utente ha partecipato ad un attacco DDoS.</p>
<pre lang="Python" line="29">import dpkt
import socket
THRESH = 10000

def trovaattacco(pcap):
    pktCount = {}
    for (ts, buf) in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            dst = socket.inet_ntoa(ip.dst)
            tcp = ip.data
            dport = tcp.dport
            if dport == 80:
                stream = src + ':' + dst
                if pktCount.has_key(stream):
                    pktCount[stream] = pktCount[stream] + 1
                else:
                    pktCount[stream] = 1
        except:
        pass

for stream in pktCount:
    pktsSent = pktCount[stream]
    if pktsSent &gt; THRESH:
        src = stream.split(':')[0]
        dst = stream.split(':')[1]
        print +src+' attaccato '+dst+' con ' + str(pktsSent) + ' pachetti.'</pre>
<p>E questo è il codice. Facile vero? Spero vi sia stato utile l'articolo e per qualsiasi domanda non esitate a contattarmi (qui, sul forum, canale IRC, dove vi è piu comodo). Naturalmente se qualcuno ha una soluzione migliore della mia è il benvenuto a mostrarcela.</p>
