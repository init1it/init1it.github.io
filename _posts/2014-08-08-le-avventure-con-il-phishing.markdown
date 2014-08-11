---
layout: post
status: publish
published: true
title: Le avventure con il phishing
author:
  display_name: robot
  login: r0b0t
  email: r0b0t82@gmail.com
  url: ''
author_login: r0b0t
author_email: r0b0t82@gmail.com
wordpress_id: 193
wordpress_url: http://blog.init1.it/?p=193
date: '2014-08-08 18:51:49 +0200'
date_gmt: '2014-08-08 16:51:49 +0200'
categories:
- News
tags: []
---
Probabilmente per i guru dell'e-commerce questo articolo non sarà una novità. Per quelli che invece sono alle prime armi può essere di grande aiuto. Mi sono sempre trovato davanti alle solite email di phishing, soliti metodi da dieci o vent'anni: "ti stan rubando la carta", "controlla i tuoi dati", bla bla bla.

Ribaltando la situazione, mi trovo a dover aiutare un amico a *vendere degli oggetti su ebay*. Gli spiego come metterli online e nel giro di pochi giorni arriva un acquirente:
> hello friend,i just view your item and am very interested in it,kindly get back to me with your price in euro
Dopo una breve contrattazione il ragazzo sembra molto disponibile:
> Thanks for your response. am ok with the price you are willing to sell your item for me in euro,kindly get back to me with your paypal email  i will paying you
> directly into your PayPal account without any delay. Get back to me with
> the details below :
> Name:
> PayPal Email address
> Total Cost of the item.
> Or you can send me a paypal money request, so once i receive the details i
> will go ahead with the payment through PayPal and then i will contact my
> shipping company after you get the payment. I will need your home address
> for the Item to be Picked Up by the Shipping Company.
> Have a nice day.

Ok, l'inglese non è perfetto ma... who cares? Sembra tutto normale.
Rispondo fornendo i dati necessari e qualche ora dopo mi arriva una sua risposta:
> have just receive an alert from PayPal Customer Care with the
> confirmation email that the payment has been made,a total of 450euro
> was sent 200euro for the (GOODS) Purchased and the extra 200euro
> for the shipping charges and 50euro for western union charges and am
> sure you notice that,which you will be sending 200euro to the address
> below via western union.
> Name: Elizabeth Difuntorum
> Address:6218 beacon isles Dr apt.206 tampa
> State: Florida
> Zip Code:33615
> Country:U.S.A
> With the issue of my details,transferring the name of ownership and
> signing of all paperwork will be done by the pick up agent so you don't
> have to worry about that.My shipper would be coming around to your area to
> have the item  picked up once you have sent the shipping charges fee to
> them,as i need you to let me know what time you want them to come for the
> pick up . I advises you to check your Inbox or your Junk/Spam mail in case
> you did not get the confirmation email from PayPal yet.
> I will be waiting to hear from you once the money has been sent to my shipper.
> Thanks for the business.

Inoltre arrivano queste email, tutte e tre da questo strano mittente:

__member@paypal.com (server_customercare@mail2pay.com)__

(Domanda veloce per i lettori: qual è l'indirizzo *reale* delle email? :D )

![http://blog.init1.it/wp-content/uploads/2014/08/email21.jpg](http://blog.init1.it/wp-content/uploads/2014/08/email21-183x300.jpg) 

Prima Email di Phishing

![http://blog.init1.it/wp-content/uploads/2014/08/email.jpg](http://blog.init1.it/wp-content/uploads/2014/08/email-300x294.jpg)

Seconda Email di Phishing

![http://blog.init1.it/wp-content/uploads/2014/08/email3.jpg](http://blog.init1.it/wp-content/uploads/2014/08/email3-300x279.jpg)

Terza Email di Phishing


Ad un osservatore attento ed esperto, queste email si distinguono subito da una reale email di PayPal.

![http://blog.init1.it/wp-content/uploads/2014/08/reale1.jpg](http://blog.init1.it/wp-content/uploads/2014/08/reale1-300x249.jpg)

Questa è una vera email inviata da Paypal.

Personalmente uso un modo molto semplice per riconoscere se l'email è Phishing o no:

* È scritta sgrammaticata? *Allora è Phishing*
* È brutta esteticamente (scritte che si sovrappongono, loghi o immagini a caso)? *Allora è Phishing*
* È insensata? (C'entra qualcosa con me? Se io ho un conto sulla banca X e mi arriva un'email della banca Y qualcosa non va! -> ) *Allora è Phishing.*
* Il mittente ha qualcosa che non va? *Allora è Phishing.*
* L'email ti chiede di cliccare su qualche link che ti rimandi alla banca/sito/ecc? *Allora è Phishing.*

Se nessuno di questi quattro punti corrisponde, *può comunque essere Phishing!*

Bene, per un attimo, credendo fosse Libero a mostrarmi le email in modo esteticamente raccapricciante mi son comunque chiesto: "Saranno vere? Può essere un'azienda che lavora per PayPal e controlla che le transazioni vadano a buon fine?"

Bene, basta un po' di ragionamento per non essere fregati:

* L'email mi è arrivata da <em>member@paypal.com (server_customercare@mail2pay.com)</em>. Bene, i mittenti di solito sono Mario Rossi (mario.rossi@blabla.com), quindi in questo caso il simpatico ladro ha impostato come nome "member@paypal.com" ma l'email non è di PayPal. Ok, può essere un'altra azienda..ma perchè fingersi PayPal?
* Riguardando le varie email ci sono immagini a caso (tipo quella del telefono con il divieto nella seconda foto) e il testo è scritto con diversi font. *Orribile.*
* Perchè collegandomi a PayPal.com non leggo nulla a riguardo?

Ormai è quasi sicuro siano truffatori. Ma non sono ancora convinto, provo a googlare "mail2pay":

[Secondo risultato di Google](http://www.ebay.com/gds/Paypal-Scamming-SPOOF-E-MAILS-BEWARE-FORGEIN-BUYERS-/10000000012904401/g.html)

Inoltro le varie email a <spoof@paypal.com> e mi confermano che è una truffa.
Nice try, furboni! :D
&nbsp;
