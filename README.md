# 2019-06-party-history

## Who did Swiss municipalities vote for?

### Preliminary Remarks

This document describes the pre-processing and exploratory analysis of the data set that is the basis of article [So haben die Schweizer Gemeinden in den letzten 40 Jahren gewählt](https://www.srf.ch/news/schweiz/wahlen-2019/wahlen-2019-so-haben-die-schweizer-gemeinden-in-den-letzten-40-jahren-gewaehlt) published on srf.ch.

SRF Data attaches importance to the fact that the data pre-processing and analysis can be reproduced and checked. SRF Data believes in the principle of open data, but also open and comprehensible methods. On the other hand, it should be possible for third parties to build on this preparatory work and thus generate further evaluations or applications.  

### R-Script & processed data

The preprocessing and analysis of the data was conducted in the [R project for statistical computing](https://www.r-project.org/). The RMarkdown script used to generate this document and all the resulting data can be downloaded [under this link](http://srfdata.github.io/2019-06-party-history/rscript.zip). Through executing `main.Rmd`, the herein described process can be reproduced and this document can be generated. In the course of this, data from the folder `input` will be processed and results will be written to `output`.

SRF Data uses Timo Grossenbacher's [rddj-template](https://github.com/grssnbchr/rddj-template) as the basis for its R scripts. If you have problems executing this script, it may help to study the instructions from the [rddj-template](https://github.com/grssnbchr/rddj-template). 


### GitHub

The code for the herein described process can also be freely downloaded from [https://github.com/srfdata/2019-06-party-history](https://github.com/srfdata/2019-06-party-history).


### License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Dataset" property="dct:title" rel="dct:type">2019-06-party-history</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/srfdata/2019-06-party-history" property="cc:attributionName" rel="cc:attributionURL">SRF Data</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Namensnennung - Attribution ShareAlike 4.0 International License</a>.


### Other projects

Code and data by [SRF Data](https://srf.ch/data) are available on [https://srfdata.github.io](https://srfdata.github.io).


### Disclaimer

The published information has been carefully compiled, but does not claim to be up-to-date, complete or correct. No liability is assumed for damages arising from the use of this script or the information drawn from it. This also applies to contents of third parties which are accessible via this offer.


## Data description of output files

The following sections describe the results of the data preprocessing as stored in the `output` folder. 

#### `output/municipalities.csv`

Contains metadata for all 2'240 municipalities as of April 2nd, 2017.

| Attribute | Type | Description |
|-------|------|-----------------------------------------------------------------------------|
| id | Numeric | Official BFS-number |
| name | String | Official name of the municipality |
| name_de | String | Contains the "common" German translation if it exists |
| name_fr | String | Contains the "common" French translation if it exists |
| name_it | String | Contains the "common" Italian translation if it exists |


#### `output/parties.csv`

Contains party classifications made by SRF Data with the help of political scientists, used throughout all projects related to elections.

| Attribute | Type | Description | 
|-------|------|---------------------------------------------------------------------|
| abbr | String | Abbreviations in D/F |
| group_id | Numeric | Unique ID for bigger parties and non-unique identifier for groups |
| bloc | Enum | Either "left", "center" or "right" classifying the political views of this party |
| group_name | String | Description for groups (corresponding to group_id) |


#### `output/dominant_party.csv`

Party strengths 1975-2015 with only the "dominant" party per year-municipality combination (dominating = the one with the highest support)

| Attribute | Type | Description | 
|-------|------|---------------------------------------------------------------------|
| year | Numeric | Election year |
| support | Double | Party strength in fraction of 1  (e.g. 0.2 = 20%) |
| municipality | Numeric | BFS ID of municipality in question |
| canton | Numeric | Cantonal abbreviation (two letters) |
| party | Numeric | Party or party grouping, referencing ID in `output/parties.csv` |


#### `output/national.csv`

Party strengths 1971-2011 on the national level.

Note: Might equal to zero if party did not have any candidates that year.

| Attribute | Type | Description | 
|-------|------|---------------------------------------------------------------------|
| year | Numeric | Election year |
| party | Numeric | Party or party grouping, referencing ID in `output/parties.csv` |
| support | Numeric | Party strength in fraction of 1 (e.g. 0.2 = 20%) |


#### `output/by_municipality/municipality_{id}.csv`

Party strengths 1971-2011 in municipality with BFS-number *id*. 

Note: Only those year-party combinations are contained where the party actually had candidates. 

| Attribute | Type | Description | 
|-------|------|---------------------------------------------------------------------|
| year | Numeric | Election year |
| party | Numeric | Party or party grouping, referencing ID in `output/parties.csv` |
| support | Numeric | Party strength in fraction of 1 (e.g. 0.2 = 20%) |


#### `output/by_party/party_{id}.csv`

Party strengths 1971-2011 in all municipalities for party with *id* as defined in `output/parties.csv`. 

Note: Only those year-municipality combinations are contained where the party actually had candidates. 

| Attribute | Type | Description | 
|-------|------|---------------------------------------------------------------------|
| municipality | Numeric | Unique identifier, referencing ID in `output/municipalities.csv` |
| year | Numeric | Election year |
| party | Numeric | Party or party grouping, referencing ID in `output/parties.csv` |
| support | Numeric | Party strength in fraction of 1 (e.g. 0.2 = 20%) |


### Original data source

#### Party Strengths

##### Per Canton and national

We configured the following data cubes from the Federal Statistics Office and downloaded the result as a comma separated file without head:

- `input/px-x-1702020000_104.csv`

From: <https://www.pxweb.bfs.admin.ch/pxweb/de/px-x-1702020000_104/px-x-1702020000_104/px-x-1702020000_104.px/>


##### Per District and Municipality

- `input/px-x-1702020000_105.csv`

From: <https://www.pxweb.bfs.admin.ch/pxweb/de/px-x-1702020000_105/-/px-x-1702020000_105.px>

The following meta information was provided by the Statistics Office:
Letzte Änderungen (12.01.2018): neuer Gemeindestand
Erhebungsstichtag: Wahldatum
Stand der Datenbank: 2015
Raumbezug: Gemeinden per 02.04.2017, inkl. Spezialgemeinden der politischen Statistik
Erhebung: Statistik der Wahlen und Abstimmungen 
In verschiedenen Kantonen werden für gewisse Wahlen Spezialgemeinden /-bezirke («extraterritoriale» Gemeinden/Bezirke) ausgewiesen. Es handelt sich dabei vor allem um Auslandschweizer (Ausland-CH), die von mehreren Kantonen separat ausgewiesen werden und um (einzelne) Stimmen die vom jeweiligen Kanton keiner offiziellen Gemeinde zugeordnet wurden (Andere, Korrespondenzweg).
Gemeinden mit so genannt «gemeinsamer Urne»: Dabei werden zwei oder mehrere Gemeinden zusammengefasst, deren Stimmzettel gemeinsam ausgezählt werden. 
2009: Fusion von FDP und LPS auf nationaler Ebene unter der Bezeichnung «FDP.Die Liberalen».
In der Waadt fusionierten FDP und LP im Jahr 2012, in Basel-Stadt haben FDP und LP nicht fusioniert. Da die LP-BS Mitglied der «FDP.Die Liberalen Schweiz» ist, wird sie zur FDP gezählt. 
Verwendete Zeichen:
'...' : Zahl unbekannt, weil (noch) nicht erhoben oder (noch) nicht berechnet, d.h. keine Kandidatur, stille Wahl oder Gemeinden mit gemeinsamen Urnen.


#### Per language region

-> `input/su-d-17.02.02.03.02_SPR_SRF-Zehr.xlsx`

We downloaded the table from the [BFS website](https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/tabellen.assetdetail.217244.html) but realized that the values for 2015 were only temporary, so we asked `poku@bfs.admin.ch` for the updated data that we got per mail in mid June 2019.

It contained the following meta information:
1) Die Ergebnisse nach Sprachregion werden aufgrund der Gemeindeergebnisse bzw. der Zugehörigkeit der Gemeinden zu einer Sprachregion 
berechnet. Bis 2000 diente die Volkszählung als Grundlage der Zuteilung einer Gemeinde zu einem Sprachgebiet, danach wird die (kumulierte) Strukturerhebung verwendet.
2) 2009: Fusion von FDP und LPS auf nationaler Ebene unter der Bezeichnung "FDP.Die Liberalen". Fusion von FDP und LP im Kanton Genf im Jahr 2010 und im Kanton Waadt im Jahr 2012. 
Im Kanton Basel-Stadt haben FDP und LP nicht fusioniert. Da die LP-BS Mitglied der „FDP.Die Liberalen Schweiz“ ist, wird die LP-BS auf gesamtschweizerischer Ebene der FDP zugeteilt.


#### Classification per language region

-> `input/Raumgliederungen.xlsx`

The classification of each municipality to a language region was extracted from the [Application of Swiss Municipalities](https://www.agvchapp.bfs.admin.ch/de/typologies/results?SnapshotDate=02.04.2017&SelectedTypologies%5B0%5D=HR_SPRGEB2016). The status of the data is the same as in the election data: 2nd of April 2017.


#### Geographical data

-> `input/gd-b-00.03-875-gg17`

The shapefiles for municipalities, cantons etc. we downloaded from [Generalisierte Gemeindegrenzen](https://www.bfs.admin.ch/bfs/de/home/dienstleistungen/geostat/geodaten-bundesstatistik/administrative-grenzen/generalisierte-gemeindegrenzen.assetdetail.1902553.html). We worked with the files in the folder `ggg_2017vz` which contain the status of 31st of December 2017 (which is the same as it was on the 2nd of April).