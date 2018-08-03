[Takaisin alkuun](README.md)

:movie_camera: [Opetusvideona](https://youtu.be/HGjGKKEzC04) :movie_camera:

# Pandoc, binääritiedostot, .gitignore ja draw.io

## Pandoc ja etunimi_sukunimi_opiskelijanumero.md

[Pandoc (- a universal document converter)](https://pandoc.org/index.html) on mukava ohjelmisto kääntelemään tiedostoja formaatista toiseen.

## Asennus

Helpoin tapa saada Pandoc toimimaan Windows ympäristössä ... on [asentaa Ubuntu Windows Subsystemin](https://docs.microsoft.com/en-us/windows/wsl/install-win10) kautta. Asia ei ole niin vaikea kuin kuulostaa, mutta vaatii tietokoneen uudelleen käynnistyksen.

Avaa Powershell Administrator käyttäjällä alapuolisen mukaisesti

![powershell](src/vko/vko04/powershell.png "powershell")

Syötä alapuolinen käsky powershelliin

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

Windows kysyy uudelleen käynnistystä, että asetukset saadaan käyttöön.

![powershell2](src/vko/vko04/powershell2.png "powershell2")

Uudelleen käynnistyksen jälkeen etsitään Windows Store:sta Ubuntu.
![store](src/vko/vko04/store.png "store")
Painetaan Get
![store2](src/vko/vko04/store2.png "store2")
Latauksen ja asennuksen jälkeen painetaan Launch.
![store3](src/vko/vko04/store3.png "store3")

Asennus kestää hetken ja kysyy käyttäjätunnusta ja salasanaa. Aseta ne alapuolisen mukaisesti.

```
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: user
Enter new UNIX password: xxxxxx
Retype new UNIX password: xxxxxx
passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

Päivitä asennustiedostot ja asenna ne.

```
user@PC:~$ sudo apt update
user@PC:~$ sudo apt upgrade
```

Asenna pandoc

```
user@PC:~$ sudo apt install pandoc
```

Alapuolinen komento kääntää .md -tiedoston .docx -tiedostoksi.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ pandoc -s README.md -o README.docx
```

Alapuolinen komento kääntää .md -tiedoston .html -tiedostoksi.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ pandoc -s README.md -o README.html
```

Lisää tiedostot repositorioon.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.docx
        README.html
        README.pdf

nothing added to commit but untracked files present (use "git add" to track)

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git add *

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git commit -m "binaaritiedostoja incoming"
[master 1f196fd] binaaritiedostoja incoming
 3 files changed, 134 insertions(+)
 create mode 100644 README.docx
 create mode 100644 README.html
 create mode 100644 README.pdf

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git push
Enter passphrase for key '/c/Users/sahka/.ssh/id_rsa':
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 118.65 KiB | 14.83 MiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To gitlab.labranet.jamk.fi:sahka/gitlab-opintojakso.git
   a6b0f52..1f196fd  master -> master
```

Voit avata .docx -tiedoston wordiin, .pdf tiedoston readeriin ja .html -tiedoston selaimeen. Kaikkien pitäisi toimia.

## Binääritiedosto

Commit sisällöstä näkee, että gitlab ei osaa esittää .docx -dokumenttia vaan tekstisisällön kohdalla lukee "File added".

![image](src/vko/vko04/binary1.png)

Ongelma tiedostossa on, että sen sisältö on binäärisessä muodossa eikä esim. ASCII tai UTF-8 -muodossa. Tämä tarkoittaa, että esim. **tail** -komento näyttää kohtalaisen erikoiselta .docx -tiedoston kohdalla.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ tail README.docx
▒▒▒▒ښp|Ā92▒\▒k|▒
                r▒▒w▒▒I▒▒▒hj,=▒▒▒▒j▒7▒▒▒
                                        ▒▒▒▒▒R)▒▒b▒▒▒n▒k9▒s2▒▒{PnJ▒▒▒e?▒▒5I3▒▒▒▒u▒}▒Qe▒▒▒▒TS8N!!
                ▒▒YJ▒▒▒&n▒ԍX▒P▒▒▒▒▒▒z#j▒▒X▒▒▒▒▒t▒t`▒▒@7▒Upm▒g0A▒▒▒▒f▒▒z▒_I▒▒v▒▒Ъ▒6▒NS▒=٥▒}▒▒ϵ▒▒▒▒Ы▒▒A;▒▒'&f▒x▒ט▒X$▒.ʃ▒▒j▒U▒h▒N▒▒▒h▒▒ڑ@▒▒▒h▒.▒V▒?▒S▒&▒▒▒t▒q▒▒?▒MqE▒Zj▒R▒/K▒▒*▒▒9▒F▒▒1▒▒
```

Tämä binäärimuoto rikkoo versionhallintaa git:ssä. Git on tarkoitettu tekstimuotoisen tiedostoformaatin versionhallintaan (lue: lähdekoodin). Git pystyy toimimaan binäärimuotoisen tiedon kanssa, mutta käyttäjän pitää tiedostaa mitä rajoitteita versionhallinta niille asettaa ja olla varovainen.

Avaamalla README.docx ja lisäämällä tekstiä nähdään että git joutuu työskentelemään .docx -tiedoston kanssa hieman erillailla (rewrite).

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.docx
		
no changes added to commit (use "git add" and/or "git commit -a")

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git add *

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git commit -m "binäärimuutoksia"
[master dc490d9] binäärimuutoksia
 3 files changed, 24 insertions(+), 1 deletion(-)
 rewrite README.docx (98%)
 create mode 100644 src/vko/vko04/binary1.png
 
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git push
Enter passphrase for key '/c/Users/sahka/.ssh/id_rsa':
Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (8/8), 81.56 KiB | 10.19 MiB/s, done.
Total 8 (delta 4), reused 0 (delta 0)
To gitlab.labranet.jamk.fi:sahka/gitlab-opintojakso.git
   626ff1a..dc490d9  master -> master
```

Binäärinen tiedosto toimii vielä repositoriosta, mutta versionhallintaan ei ole enää mitään taetta. Satunnaisesti myös binäärisissä tiedostoissa on havaittu epävakautta. Käyttöön kohdistuu siis hieman riskiä.

## .gitignore

.gitignore -tiedoston voi lisätä repositorioon, jos haluaa 100% varmasti poistaa tietyt tiedostotyypit repositorion seurannasta.

Github:lla on [esimerkki .gitignore:sta](https://gist.github.com/octocat/9257657), jossa selitetään miksei joitain tiedostoformaatteja haluta seurata. Sama tulosteena tähän.

```
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
```

Meidän tapauksessamme haluamme ehkä tästä repositoriosta poistaa .docx ja .pdf -tiedostot. Luodaan .gitignore -tiedosto repositorion juureen.

![image](src/vko/vko04/gitignore.png)

.gitignore -tiedostossa on pieniä kikkoja Windows -käyttöjärjestelmällä. Ohje [Stack Overflow:sta](https://stackoverflow.com/questions/10744305/how-to-create-gitignore-file):

```
If you're using Windows it will not let you create a file without a filename in Windows Explorer. It will give you the error "You must type a file name" if you try to rename a text file as .gitignore

To get around this I used the following steps

    Create the text file gitignore.txt
    Open it in a text editor and add your rules, then save and close
    Hold SHIFT, right click the folder you're in, then select Open command window here
    Then rename the file in the command line, with ren gitignore.txt .gitignore

You can get around this Windows Explorer error by appending a dot to the filename without extension: .gitignore. will be automatically changed to .gitignore
```

Tämän jälkeen voi lisätä .gitignore -tiedoston repositorioon.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git add .gitignore

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   .gitignore


sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git commit -m ".gitignore"
[master 51afcd3] .gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git push
Enter passphrase for key '/c/Users/sahka/.ssh/id_rsa':
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 301 bytes | 301.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To gitlab.labranet.jamk.fi:sahka/gitlab-opintojakso.git
   6560f6e..51afcd3  master -> master
```

Nyt kun .gitignore -tiedosto on lisätty. Voidaan poistaa README.docx -tiedosto git:n seurannasta.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git rm README.docx
rm 'README.docx'
```

Poistaminen on myös versiohistorian vaihe. Tämä poisto pitää ilmoittaa keskitettyyn versionhallintaan.

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    README.docx


sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git commit -m "deleted README.docx"
[master ba74a0a] deleted README.docx
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 README.docx

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git push
Enter passphrase for key '/c/Users/sahka/.ssh/id_rsa':
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 236 bytes | 236.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To gitlab.labranet.jamk.fi:sahka/gitlab-opintojakso.git
   8251993..ba74a0a  master -> master
   
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status

The following paths are ignored by one of your .gitignore files:
lueminut.docx
Use -f if you really want to add them.
```

Tämän jälkeen voimme lisätä .docx -tiedoston repositorioon ja huomata, että se ei tule seurantaan.

![image](src/vko/vko04/gitignore2.png)

## Kuviot

https://draw.io on mukava avoimen lähdekoodin työkalu piirtämiseen. Pääkohde korvaamiselle on esim. [Microsoft Visio]. Etuna git -maailmaan on, että piirretyt kuviot tallentuvat .xml -tiedostona, eli tekstimuotoisena.

Sivustolle mentäessä valitaan, että tallennetaan paikalliselle koneelle.

![image](src/vko/vko04/drawio.png)

Tämän jälkeen avataan uusi diagrammi. Myöhemmin voi avata tallentamansa tiedoston levyltä.

![image](src/vko/vko04/drawio2.png)

Piirsin alapuolisen kuvion ja käydään lävitse tallennukset. 

![image](src/vko/vko04/drawio3.png)

- File -> Save as... -> Filename: piirros.xml -> Download
   1. Avaa selaimen tallennusikkunan, jonka voit sitten sisällyttää esim. repositoriosi juureen.
   2. tallentaa .xml -formaatissa
- File -> Export as -> PNG -> Export -> Filename: piirros.png -> Download
   1. Avaa selaimen tallennusikkunan, jonka voit sitten sisällyttää esim. repositoriosi juureen.
   2. tallentaa .png -formaatissa
   
Tämän jälkeen kuvio ja sen png on lisättävissä repositorioon

```
sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        piirros.png
        piirros.xml

no changes added to commit (use "git add" and/or "git commit -a")

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git add *
The following paths are ignored by one of your .gitignore files:
lueminut.docx
Use -f if you really want to add them.

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git commit -m "piirros lisäys"
[master 0347635] piirros lisäys
 4 files changed, 9 insertions(+), 1 deletion(-)
 create mode 100644 piirros.png
 create mode 100644 piirros.xml

sahka@SAHKA-PC MINGW64 ~/Desktop/git/gitlab-opintojakso (master)
$ git push
Enter passphrase for key '/c/Users/sahka/.ssh/id_rsa':
Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 5.35 KiB | 5.35 MiB/s, done.
Total 8 (delta 4), reused 0 (delta 0)
To gitlab.labranet.jamk.fi:sahka/gitlab-opintojakso.git
   d188dff..0347635  master -> master
```

Nyt repositoriossa on versionhallinnassa
- piirros.xml
- piirros.png

Xml on tekstimuotoinen, mutta png on binäärimuotoinen (ks. alapuolinen kuva). Gitlab osaa kuitenkin visualisoida .png -formaatissa olevan tiedoston sisällön kuvaksi.

![image](src/vko/vko04/drawio4.png)

# Tee harjoitustehtävä 4

Linkki harjoitustehtävään: [Harjoitustehtävä 4](src/vko/vko04_harj.md)