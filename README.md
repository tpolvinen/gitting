# Git-käyttöohje
## Softala 3 2017
Perustuu kurssimateriaaleissa annettuun Futuricen git-ohjeeseen /Branching workflow cheat sheet for git/, Vincent Driessenin /A successful Git branching model/ -artikkeliin, Githubin omaan /Git Style Guideen/ sekä kirjoittajan aikaisempiin kokemuksiin.

Perustellut muutokset ja parannukset ovat erittäin suotavia ja projektin tarpeet menevät yleisten käytäntöjen yli.

### TL;DR 
* ole cool, käytä komentoriviä
* kloonaa repo `git clone git@github.com:tpolvinen/cityzer.git`
* muista aina, liikenteessä aluksi `git checkout master` ja `git pull` 
* tee feature branch develop branchistä: pullaa masteri, nimeä uusi branch kuvaavasti toiminnallisuuden nimellä
    * jos useampia osia: tee “toiminnallisuus/master“ branch ja siitä “toiminnallisuusx/omanimi” —tai omanimen tilalle “osatoiminnallisuusY”, välit väliviivoiksi
    * `git checkout develop` ja sitten
    * `git checkout -b toiminnallisuusx/omanimi develop`
* koodaa mukaan ekaan committiin asti, tallenna, tarpeen mukaan:  `git add .` tai `git add -A` 
* luo commit kuvaavalla viestillä: `git commit -m "Commit-viesti"`
* branching eka push komennolla `git push -u origin toiminnallisuusx/omanimi`, seuraavat `git push`
* kun toiminnallisuus on valmis, testaa
* testattuasi tee Githubin sivulla pull request, aseta:*base: develop* —EI MASTERIIN!
* Pull requesting testannut hyväksyjä tekee (feature branch -> merge -> develop branch)
`git checkout develop`  vaihdetaan tarkastetusta branchistä develop branchiin
`git merge --no-ff toiminnallisuus-x/osa-y`  mergetään feature branch developiin pitäen mukana kaikki commitit
`git branch -d toiminnallisuus-x/osa-y`  deletoidaan paikallinen feature branch
`git push origin develop`  pushataan tulos Githubissa olevan repon develop branchiin
`git push origin :toiminnallisuus-x/osa-y` ja lopuksi deletoidaan feature branch Githubista.

* Release branch tehdään lähes samalla tavalla, lue ohjeet alta! :)

### Aluksi
*Käytetään gitin kanssa komentoriviä.* IDEn omat tai muut graafiset työkalut saattavat käyttää komentoja, joista käyttäjä ei ole ihan kartalla ja pyritään välttämään niistä aiheutuvia omituisia ongelmia. Yhtäältä riski on pieni, toisaalta komentojen hallitseminen todennäköisesti antaa plussaa työhaastatteluissa ja helpottaa huomattavasti uusiin ympäristöihin sopeutumista. 

*Kloonataan repo* komennolla
`git clone git@github.com:tpolvinen/cityzer.git`

Huom: Koska emme ole forkanneet repoa jostain meneillään olevasta projektista, ei upstream-määritystä tarvitse tehdä.

*Joka päivän alussa* tulee tehdä `git checkout master` ja `git pull` jotta päästään ajan tasalle.

### Branchit
*Käytetään branchejä* `master` *ja* `develop`, joista tehdään branchejä featureille, releaseille ja hotfixeille. Käytännössä hotfixejä tuskin tullaan tekemään, mutta mainitsen sen ohjeessa koska kategoria on yleinen ja siitä on hyvä olla tietoinen. Kussakin branch-tyypissä (feature, release, hotfix) on hieman erilaiset käytännöt, ks. alla. 

Käytetään toiminnallisuuksien tekoon feature branchejä ja developista tehdään release branchit. Release branchit mergetään aikanaan masteriin testauksen ja bugifiksauksen jälkeen. 

Master branch on vain ja ainoastaan sellaiselle koodille, joka julkaistaan palvelimelle releasena. Tämä tuo selkeyttä projektin etenemiseen ja lisäksi useissa ympäristöissä master branchiin pushaaminen julkaisee automaattisesti uuden version tuotantoserverille.

*Ennen branchin luontia* tulee varmistaa, että ollaan tilanteen tasalla jotta uusi branchi saa pätevän pohjan.  Seuraavilla komennoilla otetaan käsittelyyn ensin master branch joka sitten pullataan kaikkine risuineen paikalliseen repoon. Masterin mukana tulee siis myös kaikki muutkin branchit, joita ei näin tarvitse pullata erikseen.
`git checkout master`
`git pull master`

#### Feature branch -käytännöt
*Feature branchin nimeäminen* tulisi tehdä niin, ettei siinä käytetä varattuja sanoja “master”, “develop”, “release-“ tai “hotfix-“. Tämä on vain nimeämisen selkeyden vuoksi, mikään ei sinällään mene sekaisin vaikka niitä käyttäisikin. 

Jos yhdessä toiminnallisuudessa (feature) on useita samaan aikaan työstettäviä osia (module) tai useita tekijöitä, niin branch nimetään feature / module- tai feature / name -käytännöllä, eli työstettävän toiminnallisuuden nimi, kauttaviiva ja toiminnallisuuden osan tai koodarin oma nimi. Sanavälit korvataan väliviivoilla. Tällöin on hyvä olla develop brachistä ensin tehty yhteinen branch, esim. `feature-X/master`, josta tehdään muut branchit, esim. `feature-X/module-Y` tai `feature-X/etunimi`.

Nämä nimet olisi hyvä olla yhdistettävissä siihen Trello-tikettiin, jonka pohjalta työskennellään ja  mielellään samoilla sanoilla jotta etsintään ei mene turhaan aikaa.

*Feature branchin luonti* kannattaa tehdä seuraavalla komennolla (muista pullata masteri ennen tätä), joka haaroittaa uuden branchin develop branchistä. Tässä esimerkissä tehdään branch nimeltä “toiminnallisuus-x/osa-y”.
 `git checkout -b toiminnallisuus-x/osa-y develop`

*Commitit feature branchiin* voi tehdä komennolla
`git commit -m "Commit-viesti"`
Missä commit-viestistä tulisi selvitä alle 50 merkillä mistä commitissa on kyse.

Mutta vielä parempi olisi tehdä commit-viesti editorilla, mikä avautuu komennon `git commit` jälkeen. Viestin ekan rivin tulee olla lyhyt mutta ytimekäs: alle 50 merkkiä eikä pistettä perässä. Yksi rivi väliin ja sitten tarkempi kuvaus tarpeen mukaan, mihin kirjataan kuvaus miksi tehtiin, mitä tehtiin ja mitä sivuvaikutuksia voi ilmetä.. Tämä on aika raskasta, mutta ideana on tehdä selväksi viedä vuoden päästäkin asiaan palatessa mistä commitissa oli kyse. Käytännössä en kuitenkaan usko, että tässä projektissa tullaan kirjoittamaan muita kuin yhden rivin commit-viestejä.

Kannattaa myös huomioida, että commit-viestit voivat näkyä myös asiakkaalle! Hyvin todennäköisesti niitä seurataan myös työnjohdossa. ;-)

*Feature branchin ensimmäinen PUSH* tulee tehdä komennolla
`git push -u origin toiminnallisuus-x/osa-y`
…minkä jälkeen voi kyseistä branchiä pushata tavalliseen tapaan `git push`-komennolla.

*Pull request* tulee tehdä Githubin sivuilla kun koodi on itse testattu toimivaksi. Kun koodi ja sen toiminta on testattu paikallisesti muiden kuin tekijän toimesta, voidaan branchi mergetä alkup. branchiin (yleensä develop) ja tätä kautta ottaa tehty ominaisuus mukaan releaseen menevään koodiin.

*Feature branchin mergaus develop-branchiin* tehdään vasta kun pull request on tarkastettu, testattu ja hyväksytty. Merge, samoin kuin testaus, tulee tehdä jonkun muun kuin koodin kirjoittajan toimesta. Näillä komennoilla: 
`git checkout develop`  vaihdetaan tarkastetusta branchistä develop branchiin
`git merge --no-ff toiminnallisuus-x/osa-y`  mergetään feature branch developiin pitäen mukana kaikki commitit
`git branch -d toiminnallisuus-x/osa-y`  deletoidaan paikallinen feature branch
`git push origin develop`  pushataan tulos Githubissa olevan repon develop branchiin
`git push origin :toiminnallisuus-x/osa-y` ja lopuksi deletoidaan feature branch Githubista

#### Release branch -käytännöt
Release branchillä erotetaan työstettävä kehitysvaihe omaksi versiokseen, tehdään siihen vain tarvittavat bugfixaukset ja mergetään testattuna masteriin, missä siitä muodostetaan release. Tällä vältetään samaan aikaan työstettävien featureiden ja bugfixien välisten konfliktien aiheuttamat viivytykset releasen julkaisulle ja/tai suojataan sprintin lopussa demoversiota. 

*Release branch nimetään* muotoon `release-julkaisunimi`  ja se mergetään develop ja master brancheihin (bugfixit developiin, lopullinen tuotos masteriin). Release branchiin tehdään siis vain bugfixejä, ei uusia toiminnallisuuksia.

*Release branchin luonti* toteutetaan seuraavalla komennoilla luomalla ensin branch releasenimi-X.Y develop branchistä:
`git checkout -b release-julkaisunimi-X.Y develop`

*Commitit release branchiin* tehdään samalla tavalla kuin feature branchiin.

*Release branchin ensimmäinen PUSH* ja sitä seuraavat pushit tehdään samalla tavalla kuin feature branchiin.

*Release branchin mergaus master branchiin:* Kun bugfixit on tehty (ja bugfixejä voidaan jatkuvasti mergata takaisin develop branchiin), tehdään pull request, testaukset ja mergetään release branch masteriin. Githubin sivuilla master branchistä muodostetaan release, joka voidaan julkaista.

Seuraavilla komennoilla:
`git checkout master`  otetaan käyttöön master branch
`git merge --no-ff releasenimi-X.Y -m "Merge-viesti"`  tehdään merge masteriin
Jos tulee merge conflict, fiksataan ne ja testataan koko release uudelleen. Tästä ei saa päästää läpi fiksausta joka ei todistetusti toimi myös muilla!
`git tag -a X.Y -m "Tag-viesti"`  annetaan commitille tag versionumeron mukaan
`git push origin master` julkaisuversion push masteriin

Jotta tehdyt bugfixit saadaan mukaan myös develop branchiin, otetaan käyttöön develop branch ja tehdään merge release branchistä:
`git checkout develop`
`git merge --no-ff eleasenimi-X.Y -m "Merge-viesti bugfixeille"`
Tässä vaiheessa tulee todennäköisesti konflikteja, joiden selvittämisen jälkeen voidaan pushata muutokset developiin:
`git push origin develop`
Ja release branch poistaa:
`git branch -d releasenimi-X.Y`
ja lopuksi poistetaan myös Githubissa oleva release branch:
`git push origin :release-ekajulkaisu-0.1`

#### Hotfix branch -käytännöt
Nämä kirjoitetaan myöhemmin jos tarvetta tulee! Tarvittaessa voi katsoa mallia Vincent Driessenin /A successful Git branching model/ -artikkelista. :)
