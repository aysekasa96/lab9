🚴Real-Time Bike Data Processing with Microsoft Fabric Eventstream

📌 Overzicht
Dit project laat zien hoe u realtime gegevens kunt opnemen, verwerken en opvragen met behulp van Microsoft Fabric Eventstream. We gebruiken voorbeeldgegevens van inzamelpunten voor fietsen in een deelfietssysteem en slaan de verwerkte gegevens op in een Eventhouse KQL-database.

🔗 Vereisten
Voordat u verdergaat, moet u ervoor zorgen dat u beschikt over:

Toegang tot een Microsoft Fabric-tenant.

Een werkruimte die is aangemaakt met Fabric-capaciteit (proefversie, premiumversie of Fabric).

🏗 Projectstappen
1️⃣ Een werkruimte aanmaken
Navigeer naar Microsoft Fabric en maak een nieuwe werkruimte aan met Fabric-capaciteit.

2️⃣ Een Eventhouse aanmaken
Selecteer in de werkruimte Nieuw item > Eventhouse.

Geef het een unieke naam.

Er wordt een KQL-database met dezelfde naam aangemaakt in Eventhouse.

3️⃣ Een eventstream aanmaken
Selecteer Gegevens ophalen in je KQL-database.

Kies Eventstream > Nieuwe eventstream en noem deze Bicycle-data.

4️⃣ Een gegevensbron toevoegen
Selecteer Voorbeeldgegevens gebruiken.

Noem de bron Bicycles en selecteer de voorbeelddataset Bicycles.

5️⃣ Een bestemming toevoegen
Selecteer Gebeurtenissen transformeren of voeg een bestemming toe > Eventhouse.

Gegevensinvoer instellen:

Bestemmingsnaam: bikes-table

KQL-database: Je aangemaakte database

Tabelnaam: bikes

Invoerformaat: JSON

Klik op Publiceren en wacht tot de gegevens zijn ingevoerd.

6️⃣ Vastgelegde gegevens opvragen
Voer de volgende KQL-query uit om records op te halen die in de afgelopen 24 uur zijn ingevoerd:

kql
bikes
| where ingestion_time() between (now(-1d) .. now())
7️⃣ Gebeurtenisgegevens transformeren
Bewerk de Eventstream en voeg een Group By-transformatie toe:

Bewerking: GroupByStreet

Aggregate: SUM(No_Bikes)

Group by: Street

Tijdsvenster: 5 seconden (tumbling window)

8️⃣ Getransformeerde gegevens opslaan
Maak een nieuwe Eventhouse-bestemming:

Bestemmingstabel: bikes-by-street

Invoerformaat: JSON

Klik op Publiceren om te activeren.

9️⃣ Query naar getransformeerde gegevens
Voer de volgende KQL-query uit:

kql
['bikes-by-street']
| summary TotalBikes = sum(tolong(SUM_No_Bikes)) by Window_End_Time, Street
| sort by Window_End_Time desc, Street asc
🔄 Resources opschonen
Verwijder de werkruimte na het testen.

🎯 Samenvatting
Dit project laat zien hoe Microsoft Fabric Eventstream realtime data kan vastleggen, transformeren en routeren. Door deze stappen te volgen, creëert u met succes een realtime datapijplijn voor een fietsdeelsysteem!

🤝 Bijdragen
Voel je vrij om te forken, aan te passen of verbeteringen voor te stellen!

📜 Licentie
Dit project valt onder de MIT-licentie.

# lab9
