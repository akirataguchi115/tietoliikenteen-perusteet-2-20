---
path: '/osa-2/3-sahkoposti'
title: 'Sähköposti'
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata sähköpostipalvelun.
- Osaat kuvata SMTP protokollan toimintaa ja sen käyttöä sähköpostipalvelussa.
- Osaat perustella miksi lähettäjällä ja vastaanottajalla on käytössään eri protokollat.

</text-box>

<quiz id="38dcffe8-2431-4357-ba9c-1d1405abff5d"></quiz>

## Sähköpostipalvelu

Kaikki me käytämme sähköpostipalvelua, mutta kuinka moni meistä osaa kertoa mitä sähköpostinjärjestelmässä oikeasti tapahtuu. Toki myös lähetämme tavallisia kirjeitä ja postikortteja, emmekä silloinkaan osa tarkasti kertoa miten kortti kulkee lähetyspaikasta vastaanottajalle. Tulevia tietotekniikka-alan osaajina tämän kurssin osallistujien pitää kuitenkin tietää edes yleisperiaatteet sähköpostipalvelun toiminnasta.

Sähköpostipalvelu, kuten useimmat muutkin internetin palvelut, perustuu erillisten sähköpostipalvelimien käyttöön. Nykyään käyttäjät käyttävät näitä palvelimia erityisten asiakasohjelmistojen kautta. Käyttäjän omalla koneella (tai kännykällä) voi olla erillinen sähköpostiohjelma (kuten Thunderbird, Outlook) tai www-selaimessa toimiva sähköpostipalvelu (kuten gmail, operaattorien sähköpostipalvelut). Tällä kurssille oletetaan, että käytetään erillistä sähköpostiohjelmaa. WWW-palvelimen voi ajatella vastaavaan rooliin sähköpostipalvelimien suhteen.

Sähköpostipalvelimet välittävät viestejä keskenään käyttäen yksinkertaista sähköpostin välitysprotokollaa (Simple Mail Transfer Protocol, SMTP) tai siitä edelleen kehitettyä laajempaa protokollaa (Extended SMTP, ESMTP).

Asiakasohjelma lähettää viestit sähköpostipalvelimelle käyttäen samaa SMTP protokolla kuin sähköpostipalvelin, kun se lähettää sähköpostin toiselle sähköpostipalvelimelle. SMTP-protokolla käytetään siis vain viestin lähettämiseen. Siinä lähettäjä ottaa yhteyden vastaanottajaan ja siirtää sähköpostin vastaanottajalle. Tällaisia protokollia, joissa yhteyden muodostanut lähettäjä 'työntää' viestin vastaanottajalle, kutsutaan englanninkielisen nimen mukaan PUSH-protokolliksi. Valtaosa tällä kurssilla käsiteltävistä protokollista on kuitenkin PULL-protokollia. Niissä yhteyden muudostajaa 'vetää' tietoa itselleen. Myös asiakasohjelman käyttämät protokolla POP3 ja IMAP oat PULL-tyyppisiä protokollia. Niiden avulla asiakasohjelma noutaa käyttäjän sähköpostilaatikon sisällön luettavaksi käyttäjän koneelle.

Käyttäjällä on siis sähköpostiosoite, kuten tiina.niklander@helsinki.fi, jota muut käyttäjät voivat käyttää vastaanottajan osoitteena, kun he haluavat lähettää sähköpostia kyseiselle käyttäjälle. Sähköpostiosoitteessa on kolme elementtiä: käyttäjän tunniste (tiina.niklander), ät-merkki ja postipalvelimen tunniste (helsinki.fi). Ät-merkin käytöstä voit lukeaa lisää kielitoimiston ohjepankista
http://www.kielitoimistonohjepankki.fi/ohje/20.

Käyttäjän tunnisteet yksilöivät käyttäjät vain yhden sähköpostipalvelimen sisällä. Sama käyttäjätunniste voi olla käytössä useammassa eri sähköpostipalvelimessa. Niitä voi hallinnoida yksi ja sama henkilö tai ne voivat kuulua useammalle eri henkilölle.

Sähköpostipalvelimen tunniste helsinki.fi ei ole sähköpostipalvelimen oma verkkonimi, vaan sähköpostijärjestelmässä käytettävä tunniste. Sähköpostipalvelimen verkkonimen voi selvittää nimipalvelun avulla. Nimipalvelun resurssitietueessa MX tieto siitä, mikä sähköpostipalvelin vastaa tietyn verkkoalueen (kuten helsinki.fi) sähköpostipalvelusta. Sähköpostiosoitteet sitoutuvat siis verkkonimiin siten, että tuo sähköpostipalvelimen tunniste on yleensä samalla verkkoalueen nimi.

QUIZZ:  MIkä on helsinki.fi osoitteiden sähköpostipalvelin  (Tee siis nimipavelukysely, kuten edellisessä osassa)

KUVA: Sähköpostijärjestelmästä, jossa useampi sähköpostipalvelin ja niillä pari postilaatikkoa.

HUOM:  Kuvaan liittyvä tarina yhden sähköpostin lähettämisestä ja vastaanottamisesta.



## SMTP

SMTP (Simple Mail Transfer Protocol) on sähköpostijärjestelmässä käytetty siirtoprotokolla, jolla sähköposti siirretään vastaanottajan sähköpostipalvelimelle. Sitä voidaan käyttää myös sähköpostiviestin siirtämiseen viestiä lähettävän käyttäjän sähköpostiohjelmasta lähettäjän omalle sähköpostipalvelimelle viestin edelleenlähetystä varten. SMTP protokolla on suunniteltu vain viestin siirtämiseen lähettäjältä vastaanottajalle.  Siksi sitä ei voida käyttää viestin vastaanottavan käyttäjän sähköpostiohjelmassa viestin noutamiseen lukemista varten. Viestien noutamiseen onkin myöhemmin kehitetty erillisiä protokollia kuten POP3 ja IMAP.

Nykyisin käytössä on lähinnä SMTP:stä edellen kehitetty ESMTP (Extended SMTP).  Tällä kurssilla ei paneuduta tähän laajennukseen, vaan opiskellaan sähköpsotijärjestelmän toimintaperiaate tuon alkuperäisen SMTP:n kanssa, vaikka nykyään postipavelimien suositellaan käyttävän aina tuota laajennosta. Jos jotakuta laajennus kuitenkin kiinnostaa, niin suomenkielinen wikipedian sivu https://fi.wikipedia.org/wiki/ESMTP on hyvä lähtökohta asian itsenäiselle tutkimiselle.

SMTP protokollassa on määritelty monivaiheinen viestien vaihto lähettäjän ja vastaanottajan välillä. Niiden täytyy ylläpitää tilatietoa, jotta ne pysyvät kärryillä, missä vaiheessa viestien vaihto on menossa. Tässä SMTP poikkeaa HTTP-protokollasta. HTTP-prptokolla kuten muistamme oli tilaton, jolloin lähettäjän ja vastaanottajan ei tarvinnut tilatietoa ylläpitää.

Oheinen yhteysesimerkki, eli yhden sähköpostiviestin siirto lähettävältä palvelimelta vastaanottavalla postipalvelimelle, on mukaeltu Visa Hännisen insinöörityössä  [https://www.theseus.fi/bitstream/handle/10024/23284/Hanninen_Visa.pdf?sequence=1&isAllowed=y]{"SMTP-releointipalvelun valvonta ja raportointi"}, Metropolia 2010, olleesta esimerkistä.

A (lahettava.palvelin.com) avaa TCP-yhteyden B:lle (email.vastaanottaja.com)
A:   HELO lahettava.palvelin.com
B:   250 email.vaastaanottaja.com Helo lahettava.palvelin.com, nice to interact with you
A:   MAIL FROM: <joku123@ lahettava.palvelin.com>
B:   250 sender <joku123@ lahettava.palvelin > OK
A:   RCPT TO: <kuka123@postipalvelin.com>
B:   250 recipient <kuka123@ postipalvelin.com > OK
A:   DATA
B:   250 OK; enter text, end with .
A:   Tämä on viestin sisältö.
A:   .
B:   250 postipalvelin.com OK; message accepted for delivery
A:   QUIT

Tästä esimerkistä käy hyvin ilmi monivaiheinen viestien vaihto. Tässä esimerkissä sähköpostin lähettäminen onnistuu. Protokollassa on määritelty tarkasti myös erilaiset virhetilanteet ja niihin liittyvät viestit. SMTP on HTTP-protokollan tavoin tekstipohjainen, jolloin ihmisen on mahdollista itsekin ottaa TELNET yhteys palvelimeen ja kirjoittaa nuo lähettävän palvelimen viestit ihan suoraan näppäimistöltä. 

Vastaanottajan tieto toimitetaan palvelimelle oikeasti kahteen kertaan. Se on tuolla RCPT TO viestissä, ja sen mukaisesti viesti sijoitetaan vastaanottajan postilaatikkoon. Viestin mukana näkyvä vastaanottaja tieto tulee kuitenkin erikseen osana viestin sisältöä. Näin toimitaan myös lähettäjän osoitteen kanssa.

XXXX: Tehtävä mitä hyötyjä / haittoja tästä tietojen kahteen kertaan toimittamisesta on?



HUOM: SMTP:hen käytetään paljon aikaa ja tilaa tällä kurssilla!!!!


## Viestien rakenne


## Sähköpostin haasteita


https://fi.wikipedia.org/wiki/Musta_lista_(s%C3%A4hk%C3%B6posti)

##  Salaus


## Allekirjoitus