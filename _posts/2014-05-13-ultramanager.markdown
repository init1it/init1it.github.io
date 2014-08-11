---
layout: post
status: publish
published: true
title: UltraManager
author:
  display_name: darkjoker
  login: darkjoker
  email: drkjoker@hotmail.it
  url: ''
author_login: darkjoker
author_email: drkjoker@hotmail.it
wordpress_id: 164
wordpress_url: http://blog.init1.it/?p=164
date: '2014-05-13 15:04:30 +0200'
date_gmt: '2014-05-13 13:04:30 +0200'
categories:
- Programmazione
- Sicurezza
tags:
- mifare
- ultralight
- nfc
- android
- tool
---
<p>I tag <a title="Mifare Ultralight" href="http://www.mifare.net/en/products/mifare-smartticket-ics/mifare_ultralight/" target="_blank">Mifare Ultralight</a> utilizzano la tecnologia NFC e servono per memorizzare piccole quantità di dati (16 word da 32 bit ciascuna) in maniera semplice ed economica.</p>
<p>Proprio per questo motivo sono stati adottati da diverse aziende di trasporti, organizzatori di eventi o di sistemi di autenticazione; il problema è che molto spesso questi sistemi sono implementati in maniera poco sicura.</p>
<p>Qualunque bravo smanettone inizierebbe subito a lavorarci sopra: per far ciò serve semplicemente un lettore/scrittore di tag NFC e i tool giusti. Molti smartphone android oramai hanno la tecnologia NFC, e con una spesa di circa 90€ se ne può acquistare uno senza problemi.</p>
<p>Avere i tool giusti, invece, è più difficile: le app per la lettura/scrittura di tag sono sì presenti su Google Play, ma molte di queste costano molto, o sono state progettate male, o entrambe le cose.</p>
<p>Per questo motivo ho deciso di scrivermi il mio tool di lettura/scrittura per Mifare Ultralight. Dato che poi l'app è diventata abbastanza completa da poter essere rilasciata, ho deciso di fare esattamente così e pubblicarla su Google Play.</p>
<p>UltraManager è disponibile in due versioni: Lite e Pro. La Lite ha le funzioni base per interagire con i Mifare Ultralight: può infatti leggerli, modificarli e scriverli.</p>
<p>Se invece vi interessano features più avanzate, esiste la versione Pro: questa consente anche di salvare sulla memoria del telefono tag letti o creati in precedenza; è inoltre anche possibile creare tag "nuovi", calcolare il checksum di un UID (utile se si hanno tag UID changeable), e utilizzare un convertitore Hex - Bin - Dec built-in.</p>
<p>Il prezzo della versione Pro è di € 0.99.</p>
<p>Le due app sono disponibili su Google Play:<br />
<a title="UltraManager Lite" href="https://play.google.com/store/apps/details?id=io.github.darkjoker.ultramanagerlite" target="_blank">UltraManager Lite</a><br />
<a title="UltraManager Pro" href="https://play.google.com/store/apps/details?id=io.github.darkjoker.ultramanagerpro" target="_blank">UltraManager Pro</a></p>
<p><strong>darkjoker</strong></p>
