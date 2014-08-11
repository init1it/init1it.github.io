---
layout: post
status: publish
published: true
title: SDDC fundamentals [prima parte]
author:
  display_name: BrC
  login: brc
  email: rocco.sicilia@gmail.com
  url: http://www.roccosicilia.it
author_login: brc
author_email: rocco.sicilia@gmail.com
author_url: http://www.roccosicilia.it
wordpress_id: 184
wordpress_url: http://blog.init1.it/?p=184
date: '2014-08-06 16:28:50 +0200'
date_gmt: '2014-08-06 14:28:50 +0200'
categories:
- ICT
- Cloud
tags: []
---
<p>In questo primo appuntamento proviamo a spiegare in poche parole a cosa ci si riferisce con l'acronimo che svetta in cima a questo post: S.D.D.C.</p>
<p>Con <strong>Software Defined Data Center</strong> ci si riferisce (quanto meno <em>NOI</em> tecnici) alla prospettiva di poter estendere i concetti della Server Virtualizzation all'intera infrastruttura Data Center così da beneficiare dei vantaggi tipici della virtualizzazione anche in altre aree dell'IT.</p>
<p>Quando si parla di virtualizzazione il primo pensiero va al concetto di Virtual Machine, ovvero alla possibilità di virtualizzare e consolidare un elevato numero di sistemi in pochi server sotto forma di macchine <em>guest</em>. La tipica Server Farm si compone quindi di server con il ruolo di Hosts che erogano CPU e RAM, di Storage System che erogano le LUN sulle quali poggeranno i vDISK delle macchine virtuali, di Network Appliance quali switch e router per la gestione delle reti. Questi elementi sono tipicamente in gestione a chi amministra la Farm, agli utenti - per quanto evoluti - resta l'onere di eseguire il provisioning di nuovi ambienti all'interno della Virtual Farm utilizzando gli "oggetti" che il Cloud Proveider mette a disposizione.</p>
<p>Estendendo il concetto della virtualizzazione agli altri elementi del Data Center si vuole raggiungere l'obbiettivo di dare la libertà agli utenti di gestire l'intera infrastruttura IT con una logica "a servizio". In linguaggio un po' più commerciale si parla infatti di ITaaS (<em>IT as a Services</em>) nel tentativo di far trasparire le potenzialità di questo paradigma. L'effetto finale è che l'utilizzatore ha la possibilità di eseguire il provisioning di potenza (vCPU e vRAM) come di spazio storage ed ha la possibilità di gestire le proprie reti, pubbliche e private, all'interno del proprio Virtual Data Center con degli oggetti di fatto virtuali ma facenti le funzioni di switch, router e firewall.</p>
<p>Il tutto deve ovviamente essere orchestrato da un software centralizzato in grado di esporre queste funzionalità all'utente "tipo" che, in questo contesto, è un professionista IT con le esigenze di chi amministra un'infrastruttura IT, ma con la sostanziale differenze di lavorare su un'infrastruttura completamente a servizio in ogni sua parte: server, storage, networking e security.</p>
<p>In questo momento storico probabilmente le soluzione del colosso VMware Inc. sono quelle che meglio rispondono a queste esigenze e non a caso riporto di seguito un video illustrativo delle soluzione <em>made in vmware</em> per condire le mie poche parole con il riporto (utile anche se propagandistico) degli addetti ai lavori:</p>
<p><iframe src="//www.youtube.com/embed/Jf0KdjpxgCI" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>
<p>Ma VMware non è l'unico attore in questa avventura, anzi. I risultati arrivano grazie all'integrazione di diverse tecnologie in ambito networking e storage, temi che inevitabilmente escono dal recinto di VMware per finire in casa Cisco, EMC, NetApp, ecc.</p>
<p>Con le prossime occasioni vedremo da vicino cosa significa, nella pratica, poter disporre di questo tipo di servizi appoggiandoci, per comodità, su un'infrastruttura vCloud Director di VMware.</p>
