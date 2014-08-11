---
layout: post
status: publish
published: true
title: Format string bugs
author:
  display_name: Flame_Alchemist
  login: Flame_Alchemist
  email: matteo.bana@gmail.com
  url: ''
author_login: Flame_Alchemist
author_email: matteo.bana@gmail.com
wordpress_id: 153
wordpress_url: http://blog.init1.it/?p=153
date: '2014-05-04 11:59:29 +0200'
date_gmt: '2014-05-04 09:59:29 +0200'
categories:
- News
tags: []
---
<h1>Format string bugs</h1>
<p>Un articolo un po' fuori dai miei usuali: parla infatti di exploit. Questo problema sorge dall'uso non controllato di printf e simili (fprintf, sprintf, ...) per stampare stringhe prese in input dall'utente.</p>
<h2>Da dove arrivano?</h2>
<p>I format string bugs sono una vulnerabilità, e come la maggior parte di esse proviene da:</p>
<ul>
<li>la pigrizia di noi programmatori</li>
<li> gli strumenti troppo potenti che possediamo.</li>
</ul>
<p>Da qualche parte c'è un programmatore C che sta scrivendo del codice per stampare una stringa, in C. Normalmente, dovrebbe scrivere questo:</p>
<pre>printf("%s", string);</pre>
<p>Per evitare affaticamenti, risparmiare tempo e 6 byte di memoria scrive qualcosa di piuù simile a questo:</p>
<pre>printf(string);</pre>
<p>Così facendo, il programmatore (cioè noi) ha aperto la strada ad una nuova classe di problemi di sicurezza, che permettono all'attaccante (cioè noi) di controllare il flusso di esecuzione del programma.</p>
<h2>printf -- quello che non dicono</h2>
<p>Prima di entrare nei sanguinolenti dettagli di come sfruttare una printf, vediamo che funzionalità ci offre.</p>
<p>Le solite caratteristiche che tornano utili per quello che vogliamo fare sono grossomodo queste:</p>
<ul>
<li>accetta un numero di argomenti variabile, dipendente dal numero di specificatori presenti nella stringa di formato passata.</li>
<li>stampa argomenti in base al formato fornito. %x esadecimale, %c carattere, %d intero e così via.</li>
<li>si può aggiungere un 'fill'. %5d stampa il numero, con alcuni caratteri a sinistra, in modo tale che la sua lunghezza sia 5.</li>
</ul>
<p>Quello che di solito non si dice è:</p>
<ul>
<li>%n permette di ottenere il numero di caratteri stampati (a 32 bit, esistono altri specificatori per 16 bit o singolo byte).</li>
</ul>
<pre>printf("%d %n%d", 10, &amp;pos, 5);</pre>
<p>Mette nella variabile pos il numero 3 (la lunghezza di 10 + 1 per lo spazio).</p>
<ul>
<li>%n permette di ottenere il numero di caratteri stampati, anche se il buffer su cui si stampa è troppo corto.</li>
</ul>
<pre>char buf[20];
snprintf(buf, 20, "%.100d%n", 0, &amp;pos);</pre>
<p>Salva in pos 100, non 20.</p>
<ul>
<li>%1$X permette di riferirsi in maniera posizionale agli argomenti forniti.</li>
</ul>
<pre>printf("%s %s %1$s", "a", "b");</pre>
<p>Stampa "a b a".</p>
<h2>Un esempio</h2>
<p>Piuttosto che continuare a parlare in astratto, vediamo un esempio.</p>
<pre>int main(int argc, char *argv[]) {
    int x;
    char buf[100];
    x = 1;

    if (argc &gt; 1) {
        snprintf(buf, 100, argv[1]);
        buf[99] = '';
        printf("buffer = %s\n", buf);
        printf("x = %d (0x%x)\n", x, &amp;x);
    }
}</pre>
<p>Un paio di note:</p>
<ul>
<li>ricordandoci di 'smashing the stack for fun and profit', abbiamo fatto attenzione ad usare funzioni "sicure" per copiare un buffer, imponendo un limite di 100 caratteri (che è la dimenzione del buffer).</li>
<li>stampiamo il buffer e x. x sarà l'obiettivo nel nostro attacco. Per adesso, si nota che è <strong>sempre</strong> 1.</li>
<li>Questo esempio è stato provato su arch linux (versione non troppo importante), x86 a 32 bit: è una macchina little endian (questo vuol dire che 0xBAFF1 è salvato come F1 AF 0B)</li>
<li>In generale, si dovrebbe portare ad altre architetture, con un po' di sforzo. 64 bit potrebbe diventare significativamente più complicato, per ragioni che magari diventeranno chiare più avanti.</li>
<li>compiliamo con gcc (di nuovo, versione non troppo importante), aggiungendo come opzioni `-static -zexecstack -fno-stack-protector -mpreferred-stack-boundary=2`.<br />
Dalle mie prove, l'unica utile per questo articolo è `-mpreferred-stack-boundary=2`, che dice al compilatore di salvare gruppi di 4 byte sullo stack (e non 16, come di default). Le altre possono tornarvi utili se deciderete di portare avanti i concetti spiegati qui (per esempio, se voleste mettere del codice eseguibile sullo stack).</li>
<li>potrebbe essere utile disattivare la randomizzazione dello stack. Questo è dipendente dal SO che usate, ma una ricerca google dovrebbe portarvi molto vicino.</li>
</ul>
<h2>La vulnerabilità</h2>
<p>Adesso possiamo metterci i nostri cappelli e cercare di trovare la vulnerabilità. Nel caso più semplice possibile, diamo in ingresso una stringa.</p>
<pre>$ ./p "hello"
buffer = hello
x = 1 (0xbffffa40)</pre>
<p>Esattamente come atteso. Al contrario di quanto succederebbe se questo fosse un articolo sullo stack smashing, usando stringhe più lunghe di 100 caratteri non succede niente di particolare. Usiamo ora qualche format string.</p>
<pre>$ ./p "%x %x %x %x"
buffer = 1 2031 ff203133 33303266
x = 1 (0xbffffa40)</pre>
<p>Questo risultato è decisamente più interessante. Siccome ho disattivato la randomizzazione dello stack, x rimane nello stesso indirizzo. Se non l'avessi fatto, l'indirizzo sarebbe cambiato e tutto sarebbe più rognoso.</p>
<p>Prendo un paio di righe per ricordare due cose sulle convenzioni di chiamata delle funzioni nel x86: i parametri delle funzioni vengono messi sullo stack, in ordine inverso (questo vuol dire che il primo parametro della funzione è <strong>in cima</strong>). Questo non è più vero in x64, perché si usano principalmente i registri, ma non ci importa (vuol dire che x64 non è vulnerabile? NO).<br />
snprintf si trova infatti i parametri passati (che non si vedono qua, siccome vengono gestiti in maniera corretta), ma la stringa passata contiene dei '%x': quindi continua a "togliere" (NOTA: dico toglie, ma non toglie veramente, i valori rimangono lì) valori dallo stack e si trova qualcosa che appartiene alla funzione 'main', cioè:</p>
<ol>
<li>La variabile x</li>
<li>3 byte presi dalla variabile 'buf' (che non è inizializzata, quindi grossomodo random)</li>
</ol>
<h3>Epifania, parte 0</h3>
<p>Inserendo tante '%x' come input possiamo salire molto nello stack, e vedere<br />
cose messe lì da altre funzioni. Questo è interessante come concetto (ha alcuni problemi -- per esempio, possiamo inserire %x solo finchè la stringa ha spazio --, risolvibile, ma io non approfondirò qui), e pone un certo rischio, infatti possiamo utilizzarlo per vedere dati che dovrebbero essere nascosti.<br />
Non è proprio la nostra idea di vulnerabilità (in fondo stiamo solo leggendo, no?), ma non è da sottovalutare.</p>
<h3>Epifania, parte 1.</h3>
<p>Noi controlliamo la variabile buf. E infatti, un semplice test:</p>
<pre>$ ./p "abcd %x %x %x"
buffer = abcd 1 64636261 36203120
x = 1 (0xbffffa40)</pre>
<p>0x64636261 sono i 4 byte che corrispondono alle lettere abcd (a = 0x61 in ASCII). A questo punto, se non avessimo usato snprintf prima, saremmo a posto, potremmo sovrascrivere una zona di memoria, aprirci una birra e andare a casa soddisfatti.<br />
Ma, come già ricordato, abbiamo letto Smashing the stack for fun and profit, quindi non siamo degli sprovveduti, ed abbiamo una ulteriore epifania, ma al contrario di Matrix, la terza è la migliore.</p>
<h2>Epifania, parte 2</h2>
<p>Terminiamo l'attacco passivo, e iniziamo a fare veri danni. La variabile x sembra essere l'obiettivo perfetto: proviamo a cambiare il suo valore. Ricordate `%n'?<br />
Riecco un esempio:</p>
<pre>int pos;
printf("string%n", &amp;pos);</pre>
<p>Dobbiamo caricare sullo stack l'indirizzo di 'x', e mettere '%n' nella nostra format string. In altre parole, dobbiamo emulare questo:</p>
<pre>    snprintf(buf, 100, "string %n", &amp;x);
              ^    ^       ^         ^~~~~~~~ un po' più problematica
              |    |       |
           già presenti    +~~ molto semplice</pre>
<p>Lo stack che abbiamo assomiglia a questo, che e' un po' diverso da quello che vorremmo:</p>
<pre>
         &lt;-------- 32 bit --------&gt;

         +------------------------+
     L   |    indirizzo di buf    | &lt;---- punta alla cella sotto
         +------------------------+
         |          100           |
         +------------------------+
         |  indirizzo di argv[1]  |
         +------------------------+
         |  variabile x           |
         +------------------------+
     H   |  variabile buf         | &lt;---- ce ne sono 25
         +------------------------+</pre>
<p>Quella x lì sopra ci rovina un po' i nostri piani (certo, si potrebbe andare indietro e invertire le righe che definiscono x e buf nel programma C...). In definitiva, per il momento ci accontentiamo, facciamo finta di volere stampare anche x, per fare in modo di toglierlo dalla pila senza grossi problemi.</p>
<p>Inserire l'indirizzo di x sullo stack è leggermente più problematico perché:</p>
<ul>
<li>bisogna ricordarsi l'endianness.</li>
<li>sapere l'indirizzo di x (==&gt; l'abbiamo; non è sempre così semplice)</li>
<li>convertire i byte dell'indirizzo in caratteri</li>
</ul>
<p>Allora siamo a posto. Per comodità salviamo la stringa da dare in ingresso al programma su file (e poi useremo `cat`).</p>
<pre>$ echo -e '\x40\xfa\xff\xbf%d%n' &gt; in
$ ./p `cat in`
buffer = ... 
x = 5 (0xbffffa40)</pre>
<p>Bingo! Non ci piace tanto il fatto che dobbiamo aggiungere il %d, perché ci toglie un po' di controllo sulla stringa. Infatti ora lo elimineremo.</p>
<p>Ritorna $. Tramite $ possiamo stabilire che valore prendere dallo stack: aiutandoci con lo schema di sopra, vediamo che in questo caso serve il secondo.<br />
Purtroppo il terminale non funziona molto bene quando entrano $ nella stringa, quindi dobbiamo usare una altra strada, e apriamo python.  (Nota, neanche usare `perl -e` o `python -c` funziona)</p>
<pre>$ python
Python 2.7.3 [...]
&gt;&gt;&gt; f = open('in', 'wb')
&gt;&gt;&gt; f.write('\x40\xfa\xff\xbf%2$n')
&gt;&gt;&gt; f.close()
&gt;&gt;&gt; quit()
$ ./p `cat in`
buffer = ...
x = 4 (0xbffffa40)</pre>
<p>Molto più elegante. Ok, ci sono un paio di problemi nel nostro approccio:</p>
<ul>
<li>L'indirizzo non deve contenere il byte 0 (e gli indirizzi su 64 bit contengono tanti 0).</li>
<li>Scriviamo il numero di caratteri stampati, e non un generico intero. Questo può essere risolto. Ma vi lascio solo un indizio: ricordate che prima parlavo di 'fill'?</li>
</ul>
<h2>E allora?</h2>
<p>Effettivamente l'esempio fatto è piuttosto noioso, ma vengono subito in mente due utilizzi più interessanti:</p>
<ul>
<li>Modificare delle variabili utilizzate piu' avanti nel codice.</li>
<li>Sovrascrivere un indirizzo di ritorno, per farlo puntare ad uno shellcode (o ad altra zona di memoria che controlliamo)</li>
</ul>
<h2>Finale</h2>
<p>Cosa abbiamo imparato oggi:</p>
<ul>
<li>La pigrizia non paga</li>
<li>Con un po' di tempo libero e una stringa potete trasformare un semplice errore di un programmatore in una notizia da prima pagina.</li>
<li>Quando avete problemi, gdb spesso fornisce una soluzione.</li>
<li>Aggiungete sempre varie opzioni del vostro compilatore: sempre meglio abbondare, non si sa mai.</li>
<li>E, abilitate sempre i warning: almeno -Wformat, -Wformat-security, -Wformat-nonliteral, oltre al classico -Wall</li>
</ul>
<p style="text-align: right;"><em>Much learning doth make thee mad.</em><br />
<em>    Acts, XXVI. 24.</em></p>
