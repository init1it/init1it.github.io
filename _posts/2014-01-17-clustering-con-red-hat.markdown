---
layout: post
status: publish
published: true
title: Clustering con Red Hat
author:
  display_name: wozniak
  login: wozniak
  email: caporalcleg@gmail.com
  url: http://init1.it
author_login: wozniak
author_email: caporalcleg@gmail.com
author_url: http://init1.it
wordpress_id: 102
wordpress_url: http://blog.init1.it/?p=102
date: '2014-01-17 12:39:40 +0100'
date_gmt: '2014-01-17 11:39:40 +0100'
categories:
- News
tags: []
---
<p>In questo articolo verra' brevemente illustrato come creare un cluster con Red Hat.</p>
<p>Gli step da eseguire sono:</p>
<p>1-Preparare le macchine che faranno parte del cluster (nel nostro caso ho creato due macchine con CentOS 6.4 i386)</p>
<p>2-Installare e configurare il demone ricci</p>
<p>3-Installare e configurare conga e luci</p>
<p>4-creare il cluster</p>
<p>Procediamo con l'installazione del demone ricci su ogni nodo del cluster.</p>
<blockquote><p>yum install ricci</p></blockquote>
<p>Appena e' terminata l'installazione, creiamo una password per il demone, che ci servira' dopo quando andremo a creare il cluster</p>
<blockquote><p>passwd ricci</p></blockquote>
<p>Ora e' il momento di installare i software che si occuperanno della gestione del cluster. Questi software possono essere installati anche su uno dei nodi del cluste, ma consiglio di tenere la gestione su una macchina separata. Digitiamo quindi:</p>
<blockquote><p>yum install clusterlib lvm2-cluster modcluster luci</p></blockquote>
<p>i primi tre sono il core del sitema, mentre luci ci permette la gestione tramite la comoda interfaccia web. Impostiamo gli hostname per ogni macchina, andiamo a inserire nel file host tutti i nodi del cluster con relativo indirizzo ip (se non abbiamo un servizio dns)</p>
<p>Impostiamo l'hostname nel file di configurazione di cman (/etc/sysconfig/cman -&gt; NODENAME)</p>
<p>Impostiamo il clustername nel file di configurazione di cman (/etc/sysconfig/cman -&gt; CLUSTERNAME)</p>
<p>Avviamo i servizi ricci (su ogni nodo) e luci (sulla macchina di gestione). Colleghiamoci all'indirizzo della macchina di gestione, https://indirizzo_della_macchina_con_luci:8084, logghiamoci con le credenziali di root della macchina e aggiungiamo i nodi che ci interessano, utilizzando hostname e la password precedentemente creata per il demone ricci (vedi sopra).</p>
<p>Questa breve guida ha il solo scopo di chiarire i passi base per la creazione di un cluster, non vuole rappresentare nulla di piu'. Per maggiori informazioni si puo' fare riferimento alla guida ufficiale (<a href="https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html-single/Cluster_Administration/index.html#s1-clust-config-basics-CA">https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html-single/Cluster_Administration/index.html#s1-clust-config-basics-CA</a>) nel quale sara' possibile approfondire tutti gli aspetti.</p>
