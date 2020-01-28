---
path: '/osa-5/3-vuorojen-jakelu'
title: 'Lähetysvuorojen jakelu'
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata yleisellä tasolla, miten kanavan jakaminen ja lähetysvuorojen jakelu voidaan tehdä.
- Osaat esitellä erilaisia lähetysvuorojen jakeluun liittyviä periaatteellisia ongelmia sekä joitakin niihin liittyviä ratkaisuja
- Tunnistat mm. lyhenteet TDMA, FDMA, CDMA, CSMA/CD, CSMA/CA, jne.
- Osaat simuloida Ethernetin käyttämää lähetysvuorojen jakelumenetelmää erilaisissa tilanteissa.

</text-box>

## Yhteiskäyttöinen kanava ja lähetysvuorojen jakelu

Kanava, jolla lähettäjän ja vastaanottajan välinen viesti kulkee, voi olla joko oma tai ajettu. Omaa kanavaa kutsutaan kaksipisteyhteydeksi (engl. point-to-point), koska siinä on vain kaksi päätä lähettäjä ja vastaanottaja. Jos kanava on jaettu, niin silloin se on yleislähetysyhteys (engl. broadcast), jossa voi olla useita lähettäjiä ja vastaanottajia. Kaksipisteyhteys on esimerkiksi tähtiverkon yksi haara ei parikaapeli kytkimen ja isäntäkoneen (engl. host) välillä. Myös puhelinverkossa esimerkiksi kahden modeemin välinen yhteys on kaksipisteyhteys. [Point-to-Point Protocol (PPP)](https://fi.wikipedia.org/wiki/PPP_(tiedonsiirtoprotokolla)) on suunniteltu nimeomaan käytettäväksi tällaisilla yhteyksillä. Nykyisin ethernet on paljolti syrjäyttänyt sen, koska modeemien käyttö on vähentynyt ja etherner-pohjaisten laajakaistayhteyksien käyttö lisääntynyt.

Langaton verkko, väylätopologia ja usein myös tähtitopologia ovat yleislähetysyhteyksiä, koska kaikki voivat lähettää ja kaikki vastaaottajat kuulevat kaiken tai lähes kaiken kanavassa kulkeva liikenteen. Yleislähetysyhteydellä meillä on siis yksi jaettu kanava, jota kaikki kuuntelevat. Lähetys onnistuu virheettömäasti vain jos yksi lähettäjä lähettää kerrallaan. Jos useampi lähettäjä lähettää samanaikaisesti, niin syntyy yhteentörmäys, jolloin viestien signaalit sekoittuva ja vastaanottajalle saapuu epämääräistä "bittimössöä". Vastaanottaja ei siis pysty tästä mössöstä enää erottamaan yhtä virheetöntä viestiä, vaan kaikki törmänneet viestit tuhoutuvat ja ne täytyy lähettää uudelleen. Meidän täytyy siis rakentaa mekanismi, jolla varmistetaa, että vain yksi lähettäjä kerrallaan lähettää.

Kanavan lähetysvuorojen jakelusta käytetään useita erilaisia englanninkielisiä termejä, ainakin channel access method, multiple-access protocol ja media access control (eli MAC). Ne korostavat hiukan eri asioista tästä vuoronjako problematiikasta. Suomen kielen termmi 'lähetysvuorojen jakelu' on muuten kuvaava, mutta se ei valitettavsti ota kantaa siihen, että usein kyse on nimenomaan lähettäjällä tapahtuvasta päätöksenteosta. Englanninkieliset termit korostavat nimenomaan siitä, että kyse on lähettäjän tai viestin pääsystä (access) kanavalle.

Vuoronjakomenetelmää suuniteltaessa on siis päätettävä miten solmu päättelee voiko se lähettää ja kuinka solmun on toimittava törmäystilanteessa. Huomaa, että tietoliikenteessä meillä on käytettävissä vain tämä yksi kanava, jolloin kaikki tarvittava neuvottelu lähetysvuoroista käydään tässä yhdessä ja samassa kanavassa. Neuvotteluissakin lähetetään ja vastaanotetaan viestejä, joiden lähetysvuoroista pitäisi sopia. Tässä siis sekä kontrolli (engl. control) että data kulkevat samassa kanavassa. 

Yhteiskäyttöisen kanavan vuorojenjakomenetelmä valitaan siten, että saamme mahdollisimman suuren osa kanavasta hyötykäyttöön. Tällöin menetelmän aiheuttama yleisrasite on pieni ja lähetysvuoroista sopiminen tehdään kustannustehokkaasti. Menetelmä on tällöin yleensä myös yksinkertainen ja halpa toteuttaa.

Kun kanavassa on vain yksi lähettäjä kerrallaan, se pystyy hyödyntämään kanavan koko siirtonopeuden eli siirtämään tietoa kanavan 'maksiminopeudella' R bittiä sekunnissa. Pidemmällä aikavälillä tarkasteltuna, kun meillä on M lähettäjää, niin jokainen niistä saa keskimäärin saman osuuden linjan siirtonopeudesta eli R/M bittiä sekunnissa.

Vuoronjakomenetelmän keskeinen ongelma onkin se, että miten voimme varmistaa, että yksi lähettäjä ei voi kaapata kanavaa itselleen, vaan että kaikilla lähettäjillä on tasapuolisesti mahdollisuus saada kanava käyttöön ja lähettää viesti sitä pitkin. Haluamme siis välttää yksittäisen lähettäjän nälkiintymisen (engl. starvation). Tietojenkäsittelyteoriassa nälkiintyminen tarkoittaa, että kyseinen entiteetti (tässä sanoman lähettäjä) ei saaa koskaan tarvitsemaansa resurssia (tässä kanava). Jotta nälkiintymistä ei tapahdu, niin kanavanjako menetelmän täytyy olla tasapuolinen. Toisaalta myöskään lukkiutumista (engl. deadlock), eli että mikään lähettäjä ei voisi enää lähettää, ei saa esiintyä. Menetelmän mitää siis olla kaikilta ominaisuuksiltaan hyvin toimintavarma.


Lähetysvuorojen jakaminen yhdellä kanavalla voidaan tehdä usealla eri tavalla. Kanavanjakoprotokollilla (engl. channel partitioning protocols) voimme jakaa kanava osiin ja antaa yhden osan yhden lähettäjän käyttöön. Vuorotteluprotokollilla (engl. taking-turns protocols) voimme jakaa käyttövuorot ennalta sovitun periaatteen mukaisesti vuoronperään eri lähettäjille. Kilpailuprotokollilla (engl. random access protocols) lähettäjät joutuvat kilpailemaan lähetysvuorosta. Vuorot jakautuvat niille satunnaisemmin ja lähetysvuoron odottamiseen kuluva aika on vaikeammin ennustettava.

### Kanavanjakoprotokollat

Kanavan jaolla annetaan jokaiselle lähettäjälle oma pala kanavasta. Lähettäjä saa tässä palasessa lähettää omia viestejä ilman, että muiden lähettäjien viestien signaalit häiritsevät lähetystä. Näin kukin solmu saa oman viipaleensa. Voimme jakaa kanavan paloihin ajan tai taajuuden perusteella. Lisäksi on mahdollista käyttää erilaisia bittien koodaustapoja, jolloin samassa kanavassa voidaan samanaikaisesti lähettää useita eri viestejä, jotka vastaaottaja pystyy edelleen erottamaan toisistaan.

Tässä siis oleellista on se, että lähettäjän käyttäessä sille varattua osaa kanavasta, ei yhteentörmäyksiä tapahdu, jolloin kyseinen osa kanavasta on aina lähettäjän käytettävissä. Näin kyseinen osa kanavasta on varattuna tälle lähettäjälle silloinkin, kun se ei sitä tarvitse. 

Aikaviipaloidussa kanavassa, ns. [aikajakokanavointi](https://fi.wikipedia.org/wiki/TDMA) (engl. time-division multiple access, #TDMA#), jaetaan tietty aikajakso vakiokokoisiin aipaviipaleisiin siten, että aikaviipaleen pituus on yhden kehyksen lähetysaika ja jokainen lähettäjä saa (yhden) aikaviipaleen kerran tämän aikajakson kuluessa.

KUVA:  Kanava jaettu aikaviipaleisiin ja niissä lähettäjät. Viereen sama kanava, mutta myt taajuusjaoteltuna

[Taajuusjakokanavoinnissa](https://fi.wikipedia.org/wiki/FDMA) (eng. Frequency division multiple access, FDMA) jaetaan ajan sijasta käytettävää taajuusaluetta. Tämä muistuttaa esimerkiksi radio- tai tv-kanavien jakoperustetta, kun kyseessä on radiosignaalia käyttävä lähetys. Tässä jokaiselle lähettäjälle varataan kiinteä taajuusalue (fixed frequency band), joka on vain osa koko kanavan taajuusalueesta. Koska taajuusalue varataan yhdelle lähettäjälle, niin sitä eivät muut voi käyttää sinä aikana kun varaus on voimassa. Näin, jos lähetettävää ei kyseisellä lähettäjällä ole, niin se osa kanavan kapasiteetistä jää käyttämättä, vaikka muilla olisikin lähetettävää.

Sekä aikajaossa että taajuusjaossa lähettävä solmu (tai asema) saa käyttöönsä vain osan kanavan kapasiteetista. JOs kanavan kapasiteetti R bittiä sekunnissa jaetaan tasan kaikille M:lle lähettäjälle niin jokainen voi lähettää keskimäärin R/M bittiä sekunnissa, jos se käyttää kaikki sille varatut osat kanavasta.

[Koodijakokanavoinnissa](https://fi.wikipedia.org/wiki/CDMA) (engl. code division multiple access, CDMA) voidaan koko kanavan kapasiteetti antaa kaikkien lähettäjien käyttöön samanaikaisesti. Tätä käytetään lähinnä radiotiellä, jossa jokaisella asemalla on yksilöllinen (ja muiden kanssa ortogonaalinen) tapa koodata bitit 1 ja 0. Nyt kaikkien asemien lähettämät signaalit voivat vapaasti sekoittua radioaalloilla. Vastaanottajan pitää vain tietää, millä koodauksella sille tulevat viestit on koodattu ja se voi tämän avulla erotella saapuvasta yhteissigmaalista itselleen kuuluvat bitit.

 
### Vuorotteluprotokollat

Vuorotteluprotokollia (engl. taking-turns protocols) kutsutaan myös vuoronantoprotokolliksi, koska niille on tyypillistä se, että kanavan käyttövuorot jaetaan jollakin etukäteen sovitulla tavalla (kuten vuorokysely tai vuoromerkki), mutta vain niille, joilla on jotain lähetettävää. Näin vätetään kanavan pitäminen turhaan varattuna. Toisaalta, kun lähettäjällä on lähetysvuoro, niin kaikki tietävät sen, eikä viestien yhteentörmäyksiä kanavassa pääse syntymään.

Vuorokyselyssä erityinen isäntäsolmu kyselee vuorotellen (engl. polling) jokaiselta solmulta, onko sillä lähetettävää. Jos solmulla on lähetettävää se voi kyselyn saatuaan lähettää yhden viestin. Isäntä kuuntelee signaalia ja osaa päätellä milloin lähetys loppui. Sitten se kysyy seuraavalta vuorossa olevalta solmulta, onko sillä lähetettävää. Näin lähetys vuoro kiertää solmulta toiselle.

Vuoromerkkinä (engl. token) voidaan käyttää isäntäsolmun sijaan. Se soveltuu erityisesti rengastopologiaan. Kun solmulla on vuoromerkki, se saa lähettää vietin, jonka jälkeen se antaa vuoromerkin renkaassa seuraavana olevalle solmulle. Jos solmulla ei ole lähetettävää, kun se saa vuoromerkin, se antaa merkin välittömästä eteenpäin. Näin kanavan kapasiteetti tulee käyttöön tehokkaammin kuin kanavanjakoon perustuvissa menetelmissä.

Näistä on olemassa erilaisia versioita, joissa voidaan määritellä tarkemmin esimerkiksi miten kauan vuoromerkkiä saa pitää, kuinka monta kehystä yhdessä vuorossa voi lähettää, jne. Esimerkiksi [Token Ring](https://fi.wikipedia.org/wiki/Token_Ring) on vuorottelua käyttävä verkko.

Näissä vuorottelumenetelmissä on yksi iso ongelma, ns. 'single point of failure'. Menetelmä toimii hyvin vain niin kauan kuin vuoromerkki tai kyselyjä hoitava isäntäkone ei katoa.  Jos tällainen vikaantuminen kuitenkin tapahtuu, niin mikään solmu ei voi lähettää kanavasssa ja päädytään lukkiutuneeseen tilaan. Tästä voidaan toipua vain ensin havaitsemalla tilanne ja sitten joko valitsemalla uusi isäntäsolmu tai ottamalla käyttöön uusi vuoromerkki. Tällainen toipuminen on hidasta ja sen aikana viestit eivät kulje.



### Kilpailuprotokollat

Kilpailuprotokollissa (engl. random access protocols). Näisä käytetään englanniksi myös termiä multiple access protocols. Lähettäjiä on useita ja ne kilpailevat lähetysvuoroista. Lähettäessään niiden pitää noudattaa tiettyjä sääntöjä, mutta etukäteen ei ole tiedossa milloin ja missä järjestyksessä lähettäjät voivat toimia. Periaatteena on siis yksinkertaisesti "kuka ensin ehtii". 

Koska mitään keskistettyä jakoa tai vuorottelua ei ole, niin jokaisen solmun pitää siis tehdä päätös viestin lähettämisestä tai odottamisesta pelkästään sen hetkisen paikallisen tiedon varassa. Kun solmu haluaa lähettää, niin se ensin kuuntelee, onko joku muu solmu jo lähettämässä. Jos ei ole, niin solmu aloittaa oman lähetyksensä. Kaikki lähetykset tehdään aina kanavan täydellä nopeudella.

Koska lähettäjät kilpailevat kanavasta, niin on mahdollista, että kaksi lähettäjää yrittää lähettää samaanaikaan ja tapahtuu törmäys. Mikäli kahden lähettäjän viestit ovat kanavassa samaan aikaan, niin kumpikin viesti tuhoutuu, koska niiden signaaleja ei voi sekoittumisen jälkeen enää erottaa. Kun tällainen viestien törmäys tapahtuu, niin molemmat lähettäjät joutuvat lähettämään omat viestinsä myöhemmin uudelleen.

Solmun pitää tavalla tai toisella havaita törmäys. Tässä suhteessan eri protokollat toimmivat eri tavalla. Niissä on myös eroja siinä mien törmäyksestä toivutaan eli miten ja milloin uudelleenlähetys tapahtuu. Tyypillisesti törmäyksen jälkeen solmu odottaa satunnaisen (engl. random) ajan ja yrittää uudelleen. 


#### Aloha ja viipaloitu aloha

Aloitetaan historiasta ja ihan ensimmäisestä langattoman verkon läehtysvuorojen jakoprotokollasta, jonka nimi on [ALOHA](https://en.wikipedia.org/wiki/ALOHAnet). Vaikka ALOHA kehitettiin nimenomaan langatonta radioteitse tapahtuvaa viestintää varten 1971 Havaijilla, niin sitä käytettiin myös kaapeliyhteyksillä.

Alkuperäinen ALOHA ei ollut kovin tehokas, koska jokainen solmu sai lähettää viestin heti, kun sillä oli lähetettävää. Se ei siis  etukäteen kuunnellut onko radiotiellä jo viesti kulkemassa. Solmu kuunteli vasta lähetyksen jälkeen onnistuiko lähetys. Jos törmäys tapahtui, niin solmun piti odottaa satunnainen aika ja yrittää sitten uudelleen.

Koska solmut eivät kuunnelleet etukäteen ja koska viestien pituutta ei ollut rajoitettu, niin alkuperäisessä ALOHAssa törmäyksen todennäköisyys oli niin suuri, että vain noin 18% kanavan koko kapasiteetista saatiin käyttöön. Suurin osa kanavan kapasiteetista kului siis tärmäysten vuoksi hukkaan. Katso tuolta englanninkielisestä wikipediasta kuvia, joista selviää miksi yhteentörmäykset veivät noin suuren osan kanavan kapasiteetista.

ALOHAsta tehtiin paranneltu versio, jossa karsittiin osittain päällekkäiset yhteentörmäykset pois. Viipaloidussa ALOHAssa kanavan sovittiin, että siirrettävät kehykset ovat keskenään samankokoisia. Lisäksi kanava jaettiin aikaviipaleisiin, siten että yhdessä aikaviipaleessa voi lähettää yhden kehyksen. Nyt kaikki solmut aloittivat lähetyksensä aina aikaviipaleen alusta eikä kesken toisen solmun lähetystä. Tämä onnistui, kun solmut ja niiden kellot saatiin synkronoitua. Kaikki (lähettävät) solmut havaitsevat yhteentörmäykset.

Kun siirtokehys on valmis, niin solmu lähettää sen heti seuraavassa aikaviipaleessa. Jos ei tullut yhteenörmäystä, niin solmu voi lähettää seuraavan kehyksen heti seuraavassa aikaviipaleessa. Jos lähetyksessä tuli yhteentörmäys, niin solmu yrittää lähetystä uudelleen seuraavassa aikaviipaleessa jollakin todennäköisyydellä p.

Viipaloidulla ALOHAssa saadaan kanavan kapasiteetista käyttöön noin 37%, mikä on kaksinkertainen määrä alkuperäiseen ALOHAan verrattuna. Englanninkielisessä wikipediassa on hyvin kuvattu matemaattinen päättelyketju tämän tehokkuusluvun laskennassa. 

Viipaloitua ALOHAa käytetään edelleen tietyissä erikoistilanteissa, koska se on toteutukseltaan ja toiminnaltaan hyvin yksinkertainen ja koska se sallii yhden lähettäjän lähettää täydellä nopeudella, jos muita lähettäjiä ei samanaikaisesti ole.


TEHTÄVÖ:  Kuinka paljon 1 Gigabitiä sekunnissa kanavan kapasiteetista saadaan ylläolevien tehokkuus lukujen perusteella käyttöön a) ALOHAssa b) viipaloidussa ALOHAssa

Simuloi viipaloitua alohaa (Anna lähetettävän kehykset, todennäköisyydet yms, jolloin simulointi on kaikilla samanlaianen ja tee sitten kymys  mitä missäkin viipaleessa tapahtuu (tyhjä, yhteentörmäys, a,b,c,d,e)


### Lähetyskanavan kuuntelu CSMA

[Lähetyskanavan kuuntelu](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access) (engl. Carrier Sense Multiple Access, CSMA) on ALOHAn jälkeen kehitetty menetelmä, jossa keskeistä on, että lähettäjän täytyy ennen omaan lähetystään varmistaa, että kanava on tyhjä. Ajastus on siis että solmu kuuntelee kanavaa ennen kuin lähettää ja lähettää vain jos kanava on tyhjä. Jos kanava on tyhjä, niin voi lähettää samantien. Jos kanava on varattuna, niin solmun pitää odottaa hetki ja sitten voi yrittää uudelleen. Odotuksen kesto riippuu CSMAn toteutusvaihtoehdosta. Aika tavallista on odottaa satunnainen aika ja sitten tarkistaa onko kanava vapaa. Odottaminen voi myös olla aktiivista, jolloin solmu aktiivisesti kuuntelee kanavan vapautusta ja lähettää heti kun kanava on vapaa.

Etukäteen tapahtumvalla kanavan kuuntelulla ei kuitenkaan voi havaita kaikkia muiden lähetyksiä joko lainkaan tai riittävän nopeasti, joten yhteentörmäykset ovat edelleen mahdollisia. Esimerkiksi kaapelissa etenemisviiveen takia kaksi lähettäjää voivat molemmat havaita kanavan tyhjäksi ja aloittaa lähetyksen samaanaikaan. Langattomassa lähiverkossa lähettäjän ympäristön kuuntelu ei kerro, onko vastaanottaja saamassa sanomia muilta, joita lähettäjä ei voi etäisyyden takia kuulla. Vastaavasti satelliittikanavan kuuntelu ei paljasta, onko jonkin muu maa-asema jo aloittanut lähetyksen. Muistathan, että sateelliittien kautta tapahtuvassa viestinnässä etenemisviiveet voivat olla hyvinkin suuria.

Mikäli yhteentörmäys tapahtuu, niin pakettien lähetykset epännistuvat ja ne joudutaan lähettämään uudelleen. Tällöin paketin lähettämiseen käytetty aiak menee hukkaan. Solmujen väliset etäisyydet ja etenemisviiveet vaikuttavat yhteentörmäysten todennäköisyyteen, koska jos solmu ei voi havaita toisen solmun jo aloittamaan lähetystä, niin se voi aloittaa oman lähetyksensä samaan aikaan tuon toisen solmun kanssa.

 
    
Kalvo: CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

https://fi.wikipedia.org/wiki/CSMA/CD

Asema kuuntelee myös lähettämisen jälkeen
Langallinen LAN: törmäys => signaalin voimakkuus muuttuu 
Esim. Ethernet
Langaton LAN: hankalaa
Jos törmäys, keskeytä lähettäminen heti
ja yritä uudestaan satunnaisen ajan kuluttua
Näin törmäyksen aiheuttama hukka-aika pienenee

Kauanko kuunneltava?
2* maksimi etenemisviive solmujen välillä (A ei saa lopettaa lähetystä ennenkuin  törmäyssignaali olisi ehtinyt tulla!
)

CSMA/CA https://fi.wikipedia.org/wiki/CSMA/CA
