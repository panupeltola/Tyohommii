# Skriptitesti GRK koeaineisto

## Ympäristö

- Windows 11 Business 24H2
- AMD Ryzen 7 Pro @1,9 Ghz
- 32 GB RAM
- Integrated GPU
- Projekti: API Testit
- Käyttäjä: Testi_Panu
- Käyttäjän projektit: API Testit, Koulutus-41
- Rooli projektilla: Operaattori
- Aineisto: GRK koeaineisto (Karsittu ESKA AU1 laatuaineisto)

*08.04.2025, 7:20*

Testin tarkoituksena on tarkastaa toimiiko tehty "delete_string_select_folder.py" skripti halutusti

## Tavoite

Tavoite on käydä kaikki juurikansion materiaalit läpi ja poistaa jokaisesen tiedostoston nimestä, jossa sellainen on merkkijono "IR233797_"
Tiedostot, joiden nimessä ei päätettä ole, ohitetaan.

## Skriptin idea

Ohjelma käy rekursiivisesti ensin kaikki kansiot läpi API rajapinnan ja lisää ne listalle
Tämän jälkeen se hakee jokaisen kansion läpi niiden sisältämien tiedostojen osalta. Skripti ohittaa tiedostot, joissa on merkkijono erillisellä "ignore" listalla

Uudelleen nimeämisen jälkeen ohjelma lähettää viestin muutetusta nimestä konsoliin.
Nimen poistaminen tapahtuu "replace" funktiolla. Ei testattu poistaako vain yhden vai kaikki halutun merkkijonon esiintymät, oletettavasti kaikki.
Replace korvaa merkkijonon tyhjällä.

Kun uusi nimi on määritetty, dokumentti nimetään uudelleen API rajapinnan yli PUT komennolla.


    import requests
    import json
    import re


    class RenameImages:
    def __init__(self, base_url, username, password, project_uuid):
        self.bearer = None
        self.base_url = base_url
        self.username = username
        self.password = password
        self.project_uuid = project_uuid
        self.count = 0
        self.bearer = self.authenticate_via_api(self.base_url, self.username, self.password)
        self.headers = {'Authorization': f'Bearer {self.bearer}',
                        'Content-Type': 'application/x-www-form-urlencoded'}

    def authenticate_via_api(self, base_url, username, password):
        url = f'https://{base_url}/kuura/apilogin.json'
        headers = {'Content-Type': 'application/x-www-form-urlencoded'}
        data = {
            "username": username,
            "password": password
        }
        response = requests.post(url, data=data, headers=headers)
        response.raise_for_status()

        content = response.json()
        accesstoken = content.get("apiKey")
        print("Authenticating successful")
        return accesstoken

    def get_folders(self):
        userInputF = str(input("Input root folder uuid: "))
        folders = []  # List to store all folders
        self._fetch_subfolders_recursive(userInputF, folders)  # Call recursive helper function

        return folders

    def _fetch_subfolders_recursive(self, folder_uuid, folders):
        api_url = f'https://{self.base_url}/kuura/v1/folder/{folder_uuid}/folders?depth=100'
        headers = self.headers

        self.count += 1
        response = requests.get(api_url, headers=headers)
        status_code = response.status_code

        if status_code == 200:  # Proceed only if the response is successful
            data = response.json()
            folders.append(folder_uuid)  # Add the current folder to the list

            # Check if 'folders' key exists in the response
            if 'folders' in data:
                for folder in data['folders']:
                    subfolder_uuid = folder.get('uuid')
                    if subfolder_uuid not in folders:  # Avoid duplication
                        self._fetch_subfolders_recursive(subfolder_uuid, folders)
        else:
            print(f"Failed to fetch data for folder '{folder_uuid}'")

    def rename_folder_documents(self, folder_uuid, user_input):
        ignore = [".jpg", ".png", ".jpeg"]  # Extensions to ignore
        api_url = f'https://{self.base_url}/kuura/v1/folder/{folder_uuid}/documents'
        headers = self.headers

        response = requests.get(api_url, headers=headers)
        data = response.json()

        if 'documents' in data and len(data['documents']) > 0:
            for document in data['documents']:
                document_name = document.get('name')
                if any(ext in document_name for ext in ignore):
                    print(f"{document_name} skipped")
                    continue
                elif user_input in document_name:
                    self.rename_document(document, user_input)

    def rename_document(self, document, user_input):
          
        try:
            
            headers = self.headers

            name = document.get('name')
            originalName=name
               
            name=name.replace(user_input,"")  #Poistaa nimestä merkkijonon ja lisää tilalle tyhjän.
            name = name.replace('ä', 'a').replace('å', 'a').replace('ö', 'o').replace('Ä', 'A').replace('Å', 'A').replace('Ö', 'O')   
            sanitized_name = re.sub(r'[^a-zA-Z0-9\s._-]', '', name)
            new_name = f"{sanitized_name}" #Muotoilee ja tarkastaa

            api_url = "https://app.infrakit.com/kuura/v1/document/"+document.get('uuid')+"/name?name="+new_name  #API linkki jossa nimetään tiedosto

            print("Document "+originalName+" was renamed to "+new_name)


            requests.put(api_url, headers=headers)  #Kutsuu komennon ja vaihtaa nimet
        except:
            print("Failed")

        return True

    def main(self):
        folders = self.get_folders()
        if not folders:
            print("No folders found.")
            return
        userInputC = str(input("Input character string to delete: "))
        for folder in folders:
            self.rename_folder_documents(folder, userInputC)

    sync = RenameImages(base_url="app.infrakit.com", username="", password="", project_uuid="") #Tähän käyttäjä ja salasana, project uuid saa pääkäyttäjän projektien hallinnasta (ei siis projektin asetukset)
    sync.main()

## Ajo

*7:35*

Ennen komennon ajamista, tiedostojen nimet on otettu talteen Infrakitin "File summary" ominaisuudella.

Ajan ohjelman komennolla 'python .\delete_string_select_folder.py'

![image](https://github.com/user-attachments/assets/87d105ba-e97f-462d-981c-a5acd1156bab)

Käyttäjänimi, salasana ja project_uuid on säädetty skirptin loppuun


![image](https://github.com/user-attachments/assets/ac1ba615-ada2-4988-ba49-49a20d1145ae)

Ohjelman kysyessä syötettä, annan juurikansioksi b51cf46e-dbcd-48e2-b753-a4a8f073f882

![image](https://github.com/user-attachments/assets/54aa0d90-1dc0-4034-b38b-58c706c39c9a)

Kansion haku oli hidas, en seurannut aktiivisesti, mutt kesti noin 2-7 minuuttia ennen kuin kysyi seuraavaa promptia

![image](https://github.com/user-attachments/assets/a4069f71-4a44-4289-8847-89cfcbf237d2)

Annoin poistettavaksi 'IR233797_'

Noin 7 minuuttia myöhemmin lista oli käyty läpi. Rivinumeroiden mukaan 797 tiedostoa uudelleennimettiin.

Ennen:

![image](https://github.com/user-attachments/assets/30d1f33a-58b7-4ba1-a16b-d9d1b4604c87)

Jälkeen:

![image](https://github.com/user-attachments/assets/53b2d29a-2dd0-43c8-83b5-e13b89909d85)

Yksi tiedosto jäi poistamatta, syynä eri tavalla kirjoittaminen, voisi varmistaa muuttamalla syötteen ja tarkistuksen isolla kirjoitettuna.

Uudelleen nimeämisen ehtoihin voisi lisätä, että ohittaa tiedostot joissa nimeä ei ole, turhaan kutsuu funktiota muuten.

Kaikki kaikessa, skripti toimii.

## Lisää tiedostot kansioon

Ajan skriptin 'add_string_beginning_select_folder.py'

Kansiona käytän Kansiota "Keran_alue/4000_Rakennusteksiset_rakenneosat/Sillat/U-387_S10_Dreijaportin_AK_P" uuid: 526c55ba-c27d-48a9-b4f9-b543fde4b991

Haluan kaikkien tiedostojen nimien eteen "U-387_"

Ajoin komennon ja oikeat syötteet ilmestyivät oikeaan paikkaan.

![image](https://github.com/user-attachments/assets/11d16a68-9b1d-4d2e-bff8-22e758a7165b)

Voin siis todeta näiden ohjelmien toimivan.

Tehtäväksi jää:

- Ota kansio valokuvineen ulos ja testaa ajoa
- Lisää laskuri tiedostojen nimeämiseen
- Tee ohje


















