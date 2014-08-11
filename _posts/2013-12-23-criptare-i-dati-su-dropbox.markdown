---
layout: post
status: publish
published: true
title: Criptare i dati su DropBox
author:
  display_name: BrC
  login: brc
  email: rocco.sicilia@gmail.com
  url: http://www.roccosicilia.it
author_login: brc
author_email: rocco.sicilia@gmail.com
author_url: http://www.roccosicilia.it
wordpress_id: 32
wordpress_url: http://blog.init1.it/?p=32
date: '2013-12-23 18:00:09 +0100'
date_gmt: '2013-12-23 17:00:09 +0100'
categories:
- Sicurezza
tags:
- cloud
- dropbox
- truecrypt
---
<p><a href="http://blog.init1.it/wp-content/uploads/2013/12/icon-dropbox.png"><img class="alignleft size-full wp-image-46" alt="icon-dropbox" src="http://blog.init1.it/wp-content/uploads/2013/12/icon-dropbox.png" width="150" height="150" /></a>In questo post vorrei dare alcuni spunti per protegge i propri dati, personali o aziendali, nell'utilizzo dei servizi di Cloud Storage. E' evidente a tutti quanto siano comodi questi servizi di sincronizzazione dati e di fatto vengono utilizzati sia dall'utenza privata che dalle aziende per condividere informazioni e documenti. Uno dei più diffusi è DropBox il cui principio di funzionamento è molto semplice: si tratta di un agente che mantiene sincronizzata una directory locale con la propria area di storage remota; il sistema garantisce quindi di avere i propri files sempre "on-line" e potenzialmente accessibili da altri dispositivi.</p>
<p>E' comunque saggio chiedersi quanto siano sicuri questi sistemi. Hanno un buon livello di protezione? I dati sono realmente accessibili solo da chi ha gli accessi all'account? Il tutto si basa quindi sulla fiducia che diamo al gestore, fiducia che rischia poi di schiantarsi contro il prossimo datagate come quello scatenato da <a href="http://it.wikipedia.org/wiki/Edward_Snowden" target="_blank">Snowden</a> nel corso del 2013.</p>
<p>Proviamo allora ad imparare dalla storia ed a tutelarci anche quando non sembra strettamente necessario, proviamo a criptare i dati sia sul nostro computer che sulle aree di storage remote e fuori dal nostro controllo, proviamo ad usare <a href="http://www.truecrypt.org/" target="_blank">TrueCrypt</a>.</p>
<p>TrueCrypt è un simpatico tool free, open-source e multi piattaforma (ovvero esiste per Windows, per Linux e per Mac OS) che consente di creare un'area (o volume) locale nel proprio computer. Una volta creato il volume tramite TrueCrypt in esso potremo posizione le directory e files che vogliamo proteggere. Per proteggere anche i dati che sincronizziamo con il nostro account DropBox sarà sufficiente posizionare il volume criptato all'interno della directory di lavoro di DropBox stesso.</p>
<p>Lo scenario, così come presentato, ha doppia utilità di mantenere i dati sensibili protetti da un meccanismo di crittografia a prescindere dalla posizione grazie al fatto che il dato che l'agente di DropBox sposterà online sarà stato precedentemente criptato da TrueCrypt.</p>
<p>Vediamo come installare e configurare in pochi passi il nostro volume criptato (per gli <em>utenti</em> Windows).</p>
<ul>
<li>Scarichiamo il software dal sito <a href="http://www.truecrypt.org/downloads">http://www.truecrypt.org/downloads</a> ed eseguiamo l'installazione</li>
<li>Avviamo il programma e clicchiamo su "Create Volume"</li>
</ul>
<p><a href="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-createvol.jpg"><img class="aligncenter size-medium wp-image-35" alt="truecrypt-createvol" src="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-createvol-300x211.jpg" width="300" height="211" /></a></p>
<ul>
<li>Selezionare "Create TrueCrypt volume" e cliccare su "next"</li>
<li>Selezionare "Standard TrueCrypt volume" e cliccare su "next"</li>
<li>Selezionare la posizione in cui posizionare il file del volume (es: <em>E:\DiscoProtetto.disk</em>) e cliccare su "next"</li>
</ul>
<p><a href="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-selectvol.jpg"><img class="aligncenter size-medium wp-image-36" alt="truecrypt-selectvol" src="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-selectvol-300x182.jpg" width="300" height="182" /></a></p>
<ul>
<li>Selezionare il tipo di algoritmo di crypting e cliccare su "next"</li>
<li>Definire la dimensione del volume e premere "next"</li>
<li>Inserire e confermare la password di protezione dei dati e cliccare su "next"</li>
<li>Selezionare il tipo di File System (es: <em>NTFS</em>) e cliccare su "format" per avviare il processo di formattazione del volume</li>
</ul>
<p><a href="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-formatvol.jpg"><img class="aligncenter size-medium wp-image-37" alt="truecrypt-formatvol" src="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-formatvol-300x181.jpg" width="300" height="181" /></a></p>
<ul>
<li>Al termine della formattazione tornare alla schermata principale dove è necessario selezionare il volume appena creato, associarlo ad una Unità e cliccare su "mount"</li>
</ul>
<p><a href="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-mountvol.jpg"><img class="aligncenter size-medium wp-image-41" alt="truecrypt-mountvol" src="http://blog.init1.it/wp-content/uploads/2013/12/truecrypt-mountvol-300x255.jpg" width="300" height="255" /></a></p>
<ul>
<li>Verrà ovviamente chiesta la password per accedere al volume</li>
</ul>
<p>Una volta inserita la password, se corretta, comparirà una nuova unità disco identificata dalla lettera scelta poco prima. Questa unità disco è, di fatto, criptata e solo chi è in possesso della password potrà accedervi <em>montandola</em> a sua volta.</p>
<p>NOTA: visto l'interesse e le diverse opinioni che si sono sviluppate sul tema segnalo che è disponibile un<a title="Dropbox + TrueCrypt" href="https://init1.it/viewtopic.php?f=1066&amp;t=511588" target="_blank"> TOPIC sul forum di INIT1.IT</a> dove discuterne assieme.</p>
<hr />
<p>Edited by BrC [25/12/2013]: precisato utilizzo del volume criptato così come suggerito nei commenti</p>
