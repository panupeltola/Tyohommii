# Ohjelman käyttöohje

## Ohjelma

Ohjelman nimeää tiedostoja uudestaan käyttäen Infrakitin API rajapintaa.
Ohjelmaan tulee syöttää sellaisen käyttäjän tunnukset, jolla on riittävät käyttöoikeudet projektiin.
Ohjelman ajamista varten kannattaa luoda käyttäjä, jolla on oikeus vain haluttuun projektiin, jottei vahingossa muokkaa väärää projektia.

Huomio! Ohjelma ei ole ammattilaisen tekemä ja siinä voi olla bugeja. Ohjelman käyttö omalla vastuulla!

## Ennakkovaatimukset

Jotta voit ajaa ohjelman, sinulla tulee olla asennettuna Python versio 3.8-3.12, uusin 3.13 versio ei tue vaadittua pakettia.

Python version voi asentaa joko suoraan Pythonin sivuilta https://www.python.org/downloads/ tai Microsoft kaupasta

![image](https://github.com/user-attachments/assets/5b5204a4-9fe1-432a-8723-573b1d2de728)

Pythonin asentamisen jälkeen siihen tulee vielä asentaa "requests" paketti.

Sen saa tehtyä ajamalla komentorivillä komennon 'python -m pip install requests'

Lisätietoa requests paketista: https://pypi.org/project/requests/

Tämä paketti mahdollistaa verkkokutsujen tekemisen ohjelmalla.

Komentokehotteen saa auki monella tapaa ohjeena olevan artikkelin mukaisesti: https://www.ninjaone.com/blog/how-to-use-windows-command-prompt/

Helpoin tapa avata se Windows 11 ympäristössä on kirjoittaa "cmd" Windowsin hakukenttään.

![image](https://github.com/user-attachments/assets/fe365453-019e-4b0b-8242-ce204ef6e003)

## Kirjautumistietojen syöttäminen ohjelmaan

Kun olet saanut ennakkovaatimukset tehtyä on aika syöttää projekti ja kirjautumistiedot.

Saat ohjelman auki millä tahansa tekstieditorilla, esimerkiksi Windowsin Notepadilla.
Avataksesi ohjelman sillä, klikkaa tiedostoa oikealla painikkeella, valitse avaa ohjelmalla ja etsi oikea ohjelmisto avaamiseen.

![image](https://github.com/user-attachments/assets/8e626af8-a3d8-479f-8df0-1619a2a37994)


Ennen ohjelman ajamista, sinulla tulee olla tiedoston loppuun syötettynä käyttäjänimi, salasana ja project_uuid. Tiedot syötetään koodin lopussa olevaan kenttään omiin kohtiinsa. Syötä tiedot lainausmerkkien väliin.

Käyttäjänimi ja salasana ovat halutun käyttäjän kirjautumistiedot. Project_uuid löytyy projektin pääkäyttäjä puolen asetuksista.

![image](https://github.com/user-attachments/assets/623bf0e8-903d-4ab2-a602-6f5a8058c9c3)

Muokkausvalikko

![image](https://github.com/user-attachments/assets/e2369509-8d4a-47e9-a522-796ef907e450)

Project UUID

Kun olet määrittänyt asetukset, tallenna tiedosto.

Ohjelma tarkastaa ja ohittaa kaikki tiedostot, joissa on yleinen kuvan pääte. Mikäli haluat lisätä tai poistaa listalta, sen voi tehdä hakasulkujen välissä.

![image](https://github.com/user-attachments/assets/3b7541dd-8136-49b1-93b5-ce5460def41f)


Muista poistaa tiedot aina käytön jälkeen. Salasanojen ja käyttäjätietojen tallentaminen selkokielisenä on tietoturvariski.

## Ohjelman ajaminen

Ennen ohjelman ajamista on suositeltavaa tallentaa tiedostojen yhteenveto:

![image](https://github.com/user-attachments/assets/6ad6cc03-9c72-4cf7-b261-3996e4d5c7bb)

Kun ohjelma on valmis ajettavaksi avaa komentorivi sen ohjelman tiedoston sisältämässä kansiossa.

Tämän voi tehdä kahdella tapaa helposti:

- Klikkaa kansion sisällä oikealla hiirellä ja valitse "Open in Terminal" tai valitse kansion yläreunasta kansiopolku ja kirjoita sen tilalle "cmd" ja paina Enteriä

![image](https://github.com/user-attachments/assets/61fc5c82-0922-457d-a5a8-9e78fa1479ad)

![image](https://github.com/user-attachments/assets/2473fb0e-159b-44bb-ba35-9c109ea4ba7e)

Tiedoston ajamiseksi kirjoita python .\<Tiedoston nimi>

Tämän jälkeen ohjelma kysyy juurikansiota, johon juurikansion nimi on kopioitava.

![image](https://github.com/user-attachments/assets/ba517a68-469e-41f4-904f-854187cdbd9e)

Juurikansion uuid:n löydät menemällä Infrakit projektin kansion "Tiedostot" sivulla.

![image](https://github.com/user-attachments/assets/8bd664ea-2feb-4650-9838-7b51b3a5642f)

Ohjelmaa ajettaessa ohjelma listaa ensin kansiot, jotka se on käynyt läpi. Tällä ei ole ohjelman käytön kannalta merkitystä, se antaa vain tavan seurata, että ohjelma ei ole jumiutunut.

![image](https://github.com/user-attachments/assets/ec1cb15f-2b06-483b-a07d-194f8829224f)

Kun ohjelma on listannut kaikki kansiot se pyytää poistettavaa merkkijonoa. Lisää tähän mitä haluat poistettavan.

![image](https://github.com/user-attachments/assets/c181965f-d18f-4ee8-b5bf-a01bb06fbe6d)

Tämän jälkeen ohjelma käy kaikkien edellisessä vaiheessa listattujen tiedostot läpi, poistaa kaikki halutut merkkijonot ja ohittaa tiedostot, joissa on "ignore" listalla oleva merkkijono.

![image](https://github.com/user-attachments/assets/a823da7b-8fa3-4cc7-b9f1-f68393f10166)

Ohjelma tulostaa myös alkuperäisen ja uuden nimen.

Ohjelman ajon lopuksi se palaa tilaan, jossa näkyy sijainti kansiorakenteessa ja käyttäjä voi taas syöttää tekstiä.

![image](https://github.com/user-attachments/assets/2433ffc9-f1a2-464d-a532-e778d25003df)


## Lisäys

Lisäysohjelma toimii pitkälti samalla tapaa. Sen askeleet ovat:

1. Lisää käyttäjätunnus, salasana ja projekti uuid tiedostoon
2. Etsi haluamasi alkukansio
3. Aja ohjelma 'python .\<ohjelman nimi>
4. Kun ohjelma pyytää, lisää haluamasi teksti, joka tulee tiedoston alkuun (Muista lisätä alaviiva jos haluat sen tiedostoon)
5. Katso, kun tiedostot nimetään uudelleen




















