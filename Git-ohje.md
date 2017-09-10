# Git-ohje
Ensin lyhyt yleinen kuvaus, sitten varsinaiset käytänteet ja käytettävät komennot.

Työskentelyn alkaessa muista aina `git checkout master` ja `git pull` -joka päivän aluksi! 

Kun teet pull requestia, muista laittaa base-kohtaan `develop` branch, EIKÄ `master`!

Lue tämän ohjeen pohjatiedoksi artikkeli [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/) -saatavilla osoitteesta: http://nvie.com/posts/a-successful-git-branching-model/

Lisäksi kannattaa lukea Githubin ohje [Git Style Guide](https://github.com/agis/git-style-guide) -saatavilla osoitteessa https://github.com/agis/git-style-guide 

## Yleinen kuvaus
Käytetään `master` branchia vain julkaisuille, `develop` branchia kehitykseen, `release-*` branchejä vain julkaisujen bugifiksaukseen sekä julkaisujen viemiseen `master` branchiin ja `hotfix-*` branchejä julkaistujen korjaamiseen.

`master`on siis vain julkaistuille versioille eli releaseille. Masteriin tuodaan vain testattuja versioita `release-*` branchien kautta. `master` branchista voidaan haarauttaa `hotfix-*`branchejä bugifiksausta varten.

`develop` branch on varsinaiselle työskentelylle, ja siitä haarautetaan kaikki toiminnallisuuksien/ominaisuuksien kehitysbranchit. `develop` branchistä haarautetaan myös `release-*` branchit julkaisuja varten.

`develop` branchistä haarautetaan kullekin työstettävälle toiminnolle/ominaisuudelle oma branch. Näitä kutsutaan **feature branch**eiksi. Nimetään feature branchit selkeästi toiminnallisuuden mukaan ja mielellään niin, että nimi on jollain tavoin yhtenäinen Trello-tiketin kanssa. Jos yhden toiminnallisuuden koodaamiseen tarvitaan useita branchejä esim. eri tekijöille, luodaan `develop` branchistä ensin toiminnallisuuden oma masteri. Masterin nimeäminen tehdään muodolla `toiminnallisuusx/master` ja tekijöiden tai osien branchit haarautetaan e.m. masterista ja nimetään ne muodolla `toiminnallisuusx/omanimi`. Vaihtoehtoisesti voi käyttää oman nimen sijaan osan nimeä muodolla `toiminnallisuusx/osay`. Välit korvataan väliviivoilla. Feature branch mergetään takaisin ´develop´ branchiin hyväksymikäytännön mukaan, eli oman testauksen jälkeen **pull request**, muiden tekemä testaus ja hyväksyntä sekä lopuksi viimeisen hyväksyjän tekemä merge `develop` branchiin. Ei masteriin!

`develop` branchistä haarautetaan samoin kullekin **release**lle oma `release-*` branch, jossa tehdään vain bugifiksausta ja testausta. Nimeäminen tehdään muodossa `release-julkaisunimi` esim. "release-demo02" tai "release-sprint04". Release branchissä tehtyjä bugifiksauksia voidaan jatkuvasti mergetä takaisin `develop` branchiin. Lopuksi testattu ja hyväksytty release mergetään sekä `master` että `develop` brancheihin ja merkitään masterissa versionumeron tagillä.

## TL;DR 
* Ole cool, käytä komentoriviä.
* Muista aina, liikenteessä aluksi `git checkout master` ja `git pull` 
* Luo feature branch `develop` branchistä: pullaa `master`, nimeä uusi branch kuvaavasti toiminnallisuuden nimellä
    * Jos useampia osia: tee “toiminnallisuusx/master“ branch ja siitä “toiminnallisuusx/omanimi” branch -—tai omanimen tilalle “osan-nimi”, välit väliviivoiksi
    * `git checkout develop` ja sitten
    * `git checkout -b toiminnallisuusx/omanimi develop`
* Koodaa ekaan committiin asti, tallenna, tarpeen mukaan:  `git add .` tai `git add -A` 
* Luo commit kuvaavalla viestillä: `git commit -m "Commit-viesti"`
* Branching **ensimmäinen push** komennolla `git push -u origin toiminnallisuusx/omanimi`, seuraavat `git push`
* Kun toiminnallisuus on valmis, testaa!
* Testattuasi tee Githubin sivulla pull request, aseta *base: develop* —EI MASTERIIN!
* Pull requesting testannut hyväksyjä tekee (feature branch -> merge -> develop branch)
`git checkout develop`  vaihdetaan tarkastetusta branchistä develop branchiin
`git merge --no-ff toiminnallisuus-x/osa-y -m "Merge-viesti"`  mergetään feature branch developiin pitäen mukana kaikki commitit
`git branch -d toiminnallisuus-x/osa-y`  deletoidaan paikallinen feature branch
`git push origin develop`  pushataan tulos Githubissa olevan repon develop branchiin
`git push origin :toiminnallisuus-x/osa-y` ja lopuksi deletoidaan feature branch Githubista.

Release branch tehdään lähes samalla tavalla, lue ohjeet alta! :)

## Käytänteet ja käytettävät komennot
### Aluksi
**Käytetään gitin kanssa komentoriviä.* IDEn omat tai muut graafiset työkalut saattavat käyttää komentoja, joista käyttäjä ei ole ihan kartalla ja pyritään välttämään niistä aiheutuvia omituisia ongelmia. Yhtäältä riski on pieni, toisaalta komentojen hallitseminen todennäköisesti antaa plussaa työhaastatteluissa ja helpottaa huomattavasti uusiin ympäristöihin sopeutumista. 

*Joka päivän alussa* tulee tehdä `git checkout master` ja `git pull` jotta päästään ajan tasalle.

### Branchit
**Käytetään branchejä** `master` *ja* `develop`, joista tehdään branchejä featureille, releaseille ja hotfixeille. Kussakin branch-tyypissä (feature, release, hotfix) on hieman erilaiset käytännöt, ks. alla. 

Käytetään toiminnallisuuksien tekoon feature branchejä ja developista tehdään release branchit. Release branchit mergetään aikanaan masteriin testauksen ja bugifiksauksen jälkeen. 

`master` branch on vain ja ainoastaan sellaiselle koodille, joka julkaistaan palvelimelle releasena. Tämä tuo selkeyttä projektin etenemiseen ja lisäksi useissa ympäristöissä `master` branchiin pushaaminen julkaisee automaattisesti uuden version tuotantoserverille.

**Ennen branchin luontia** tulee varmistaa, että ollaan tilanteen tasalla jotta uusi branchi saa pätevän pohjan.  Seuraavilla komennoilla otetaan käsittelyyn ensin master branch joka sitten pullataan kaikkine risuineen paikalliseen repoon. Masterin mukana tulee siis myös kaikki muutkin branchit, joita ei näin tarvitse pullata erikseen.
`git checkout master`
`git pull master`

#### Feature branch -käytännöt
Feature branches käytetään suojaamaan yhteistä kehitettävää projektia. Feature branchissä tehdään varsinainen työ, joka sovitulla tavalla testataan ja hyväksytään ennen mergaamista takaisin `develop` branchiin.

**Feature branchin nimeäminen** tulisi tehdä niin, ettei siinä käytetä varattuja sanoja “master”, “develop”, “release-“ tai “hotfix-“. Tämä on vain nimeämisen selkeyden vuoksi, mikään ei sinällään mene sekaisin vaikka niitä käyttäisikin. 

Jos yhdessä toiminnallisuudessa (feature) on useita samaan aikaan työstettäviä osia (module) tai useita tekijöitä, niin branch nimetään feature / module- tai feature / name -käytännöllä, eli työstettävän toiminnallisuuden nimi, kauttaviiva ja toiminnallisuuden osan tai koodarin oma nimi. Sanavälit korvataan väliviivoilla. Tällöin on hyvä olla `develop` brachistä ensin tehty yhteinen branch, esim. `toiminnallisuusx/master`, josta tehdään muut branchit, esim. `toiminnallisuusx/osay` tai `toiminnallisuusx/omanimi`.

Nämä nimet olisi hyvä olla yhdistettävissä siihen Trello-tikettiin, jonka pohjalta työskennellään ja  mielellään samoilla sanoilla jotta etsintään ei mene turhaan aikaa.

**Feature branchin luonti** kannattaa tehdä seuraavalla komennolla (muista pullata `master` branch ennen tätä). Tämä haaroittaa uuden branchin develop branchistä ja tässä esimerkissä tehdään branch nimeltä “toiminnallisuusx/osa3”.
 `git checkout -b toiminnallisuusx/osa3 develop`

**Commitit feature branchiin** voi tehdä komennolla
`git commit -m "Commit-viesti"`
Missä commit-viestistä tulisi selvitä alle 50 merkillä mistä commitissa on kyse.

Mutta vielä parempi olisi tehdä commit-viesti editorilla, mikä avautuu komennon `git commit` jälkeen. Viestin ekan rivin tulee olla lyhyt mutta ytimekäs: alle 50 merkkiä eikä pistettä perässä. Yksi rivi väliin ja sitten tarkempi kuvaus tarpeen mukaan, mihin kirjataan kuvaus miksi tehtiin, mitä tehtiin ja mitä sivuvaikutuksia voi ilmetä.. Tämä on aika raskasta, mutta ideana on tehdä selväksi viedä vuoden päästäkin asiaan palatessa mistä commitissa oli kyse. 

Kannattaa myös huomioida, että commit-viestit voivat näkyä myös asiakkaalle! Hyvin todennäköisesti niitä seurataan myös työnjohdossa. ;-)

**Feature branchin ensimmäinen PUSH** tulee tehdä komennolla
`git push -u origin toiminnallisuusx/osa3`
…minkä jälkeen voi kyseistä branchiä pushata tavalliseen tapaan `git push`-komennolla.

**Pull request** tulee tehdä Githubin sivuilla kun koodi on itse testattu toimivaksi. Kun koodi ja sen toiminta on testattu paikallisesti muiden kuin tekijän toimesta, voidaan branchi mergetä alkup. branchiin (yleensä develop) ja tätä kautta ottaa tehty ominaisuus mukaan releaseen menevään koodiin.

**Feature branchin mergaus develop-branchiin** tehdään vasta kun pull request on tarkastettu, testattu ja hyväksytty. Merge, samoin kuin testaus, tulee tehdä jonkun muun kuin koodin kirjoittajan toimesta. Näillä komennoilla: 
`git checkout develop`  vaihdetaan tarkastetusta branchistä develop branchiin
`git merge --no-ff toiminnallisuus-x/osa-y -m "Merge-viesti"`  mergetään feature branch developiin pitäen mukana kaikki commitit
`git branch -d toiminnallisuus-x/osa-y`  deletoidaan paikallinen feature branch
`git push origin develop`  pushataan tulos Githubissa olevan repon develop branchiin
`git push origin :toiminnallisuus-x/osa-y` ja lopuksi deletoidaan feature branch Githubista

#### Release branch -käytännöt
Release branchillä erotetaan työstettävä kehitysvaihe omaksi versiokseen, tehdään siihen vain tarvittavat bugfixaukset ja mergetään testattuna `master` branchiin, missä siitä muodostetaan release. Tällä vältetään samaan aikaan työstettävien featureiden ja bugfixien välisten konfliktien aiheuttamat viivytykset releasen julkaisulle ja/tai suojataan sprintin lopussa demottavaa versiota. 

**Release branch nimetään** muotoon `release-julkaisunimi`  ja se mergetään develop ja master brancheihin (bugfixit developiin, lopullinen tuotos masteriin). Release branchiin tehdään siis vain bugfixejä, ei uusia toiminnallisuuksia.

**Release branchin luonti** toteutetaan seuraavalla komennoilla luomalla ensin branch “release-julkaisunimi-X.Y” `develop` branchistä:
`git checkout -b release-julkaisunimi-X.Y develop`

**Commitit release branchiin** tehdään samalla tavalla kuin feature branchiin.

**Release branchin ensimmäinen PUSH** ja sitä seuraavat pushit tehdään samalla tavalla kuin feature branchiin.

**Release branchin mergaus master branchiin:** Kun bugfixit on tehty (ja bugfixejä voidaan jatkuvasti mergata takaisin `develop` branchiin), tehdään pull request, testaukset ja mergetään release branch `master` branchiin. Githubin sivuilla `master` branchistä muodostetaan release, joka voidaan julkaista.

Seuraavilla komennoilla:
`git checkout master`  otetaan käyttöön master branch
`git merge --no-ff release-julkaisunimi-X.Y -m "Merge-viesti"`  tehdään merge `master`iin
Jos tulee merge conflict, fiksataan ne ja testataan uudelleen. Tästä ei saa päästää läpi fiksausta joka ei todistetusti toimi myös muilla!
`git tag -a X.Y -m "Tag-viesti"`  annetaan commitille tag versionumeron mukaan
`git push origin master` julkaisuversion push masteriin

Jotta tehdyt bugfixit saadaan mukaan myös `develop` branchiin, otetaan käyttöön `develop` branch ja tehdään merge release branchistä:
`git checkout develop`
`git merge --no-ff release-julkaisunimi-X.Y -m "Merge-viesti bugfixeille"`
Tässä vaiheessa tulee todennäköisesti konflikteja, joiden selvittämisen jälkeen voidaan pushata muutokset `develop` branchiin:
`git push origin develop`
Ja release branch poistetaan:
`git branch -d releasenimi-X.Y`
ja lopuksi poistetaan myös Githubissa oleva release branch:
`git push origin :release-ekajulkaisu-0.1`

#### Hotfix branch -käytännöt
Nämä kirjoitetaan myöhemmin jos tarvetta tulee! Tarvittaessa voi katsoa mallia Vincent Driessenin /A successful Git branching model/ -artikkelista. :)
