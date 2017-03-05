#Git

Distribuovaný systém správy verzí - poznámky k přednášce

#Dokumentace

Všechny příkazy i popis práce s větvemi je popsána v knize
[Pro Git](https://git-scm.com/book/en/v2), tuto knihu si můžete
přečíst i v [češtině](https://git-scm.com/book/cs/v1). Pro práci
vývojáře jsou důležité první tři kapitoly. Pokud se chcete
dozvědět něco o kořenech Gitu tak mrkněte na
[tuhle  přednášku](https://youtu.be/4XpnKHJAok8).

#Historie

Git vytvořil Linus Torvalds v roce 2005, vznikl jako nástroj pro
správu zdrojových kódů linuxového jádra, protože neexistoval
ve své době vhodný open-source nástroj pro tento účel. Aktuálně
git vyvíjí [Junio Hamano](https://en.wikipedia.org/wiki/Junio_Hamano).

Základ Gitu byl napsán během několika týdnů a přitom výkon a
použitelnost byla nesrovnatelně lepší oproti tehdejším open source
variantám verzovacích systémů (CVS a SVN).

Je to příklad open source projektu, který je nesrovnatelně lepší
než běžně používané komerční varianty. Důkazem je, že git je dnes
široce používaný i ve velkých  softwarových firmách a investice
do učení tohoto nástroje se rozhodně vyplatí.

#Diff & patch

Git je postavený na changesetech. Které vznikají na základě
"patchů".

```
diff -u original.c new.c > original.patch
patch original.c < original.patch
```

Diff a patch je dvojice klasickych unixových utilit pro práci s textovými
soubory, Diff vygeneruje soubor, který popisuje rozdíl mezi dvěma soubory
nebo sadou souborů a patch je schopen tento rozdílový soubor zpětně aplikovat.
Asi Všechny verzovací systémy dneška stojí v základech na těchto dvou
utilitách. Pro mě bylo klíčové tyto dvě utility pochopit abych věděl, jak vůbec
verzovací systémy a konkrétně Git fungují. Než se člověk pustí do studia gitu
je fajn se podívat jak [diff a patch fungují](http://www.thegeekstuff.com/2014/12/patch-command-examples/).

#Kde je soubor v repozitáři uložen

Git ukládá postupně změny v souborech patche do databáze. Neukládá tedy
jednotlivé verze souborů ale jen jejich změny *changesets*.


```
Soubor na lokalním disku
       |
+------v----+
| git add   |
+------v----+
       |
Stage - příprava k zapsání
       |
+------v----+
| git commit|
+-----------+
       |
Lokální databáze gitu
       |
+------v----+
| git push  |
+-----------+
       |
Vzdálená databáze gitu
```

Toto je zjednoušený popis jak postupně změny v souborech nahrát do lokálního úložiště a do
vzdáleného repozitáře. Podrobný popis je [tady](https://git-scm.com/book/cs/v2/Z%C3%A1klady-pr%C3%A1ce-se-syst%C3%A9mem-Git-Nahr%C3%A1v%C3%A1n%C3%AD-zm%C4%9Bn-do-repozit%C3%A1%C5%99e)

#Git je distribuovaný

Všechny operace nutné pro vývoj probíhají na proti lokální databázi. To znamená, že nahrání
do změn do repozitáře, práce s větvemi a práce s historií jsou velmi rychlé.

Není žádný centrální repozitář, všechny repozitáře jsou na stejné úrovni a obsahují celou historii.
To znamená, že sice klonování vzdáleného repozitáře trvá dlouho, ale následná práce na projektu
je již velmi rychlá.

## Důležité příkazy pro nahrávání změn

   * [`git add`](https://git-scm.com/docs/git-add) - přidávavá změněné a nové soubory a do
     [stage](http://softwareengineering.stackexchange.com/questions/119782/what-does-stage-mean-in-git).
	 Pomocí `git add -p` lze poslat do stage jen některé změny a ne celé soubory.
   * [`git status`](https://git-scm.com/docs/git-status) - vypíše aktuální stav lokálního repozitáře a lokální kopie
     souborů. Změněné soubory, soubory které nejsou kontrolované gitem, nenahrané změny ze větví ve vzdálených
     repozítářů.
   * [`git commit`](https://git-scm.com/docs/git-commit) - posílá změny ze stage do lokálního repozitáře. `git add`
     a `git commit` lze spoujit dohromady pomocí parametrů `git commit -a` a `git commit <soubor pro commit>`.
   * [`git log`](https://git-scm.com/docs/git-log) - zobrazuje seznam commitu
   * [`git reset`](https://git-scm.com/docs/git-reset) - vrací změny ze stage do lokální kopie. Lze použít i pro
     likvidaci starších commitů `git reset HEAD~X`, kde X je číslo, které určuje počet odstraňovaných commitů.
   * [`git diff`](https://git-scm.com/docs/git-diff) - bez parametrů lze vypíše rozdíl mezi lokální
     kopií a posledním commitem resp. stavem ve stage. Dobré je když vyvyíjíte ve větví, tak před jejím
     merge zobrazit rozdíl vůči větvi do které chcete merge udělat a podívat se co jste měnili. Při odeslání
     do větve master je to  `git diff master`.
   * [`git stash`](https://git-scm.com/docs/git-stash) - schová vám změny v lokálních souborech do speciální
     oblasti a zruší změny v lokální kopii souborů. Pozor, když to špatně použijete můžete přijít o to co jste udělali.

## Důležité příkazy pro práci s větvemi

   * [`git checkout`](https://git-scm.com/docs/git-checkout) - tento příkaz používám ke třem věcem.
      1. k přepínání mezi větvem `git checkout moje_branch`
      1. k vytvoření  nové větve `git checkout -b nazev_nove_vetve nazev_vetve_ze_ktere_vychazim`
      1. ke zničení změn v lokální kopii souborů. Zničí všechny v aktálním adresáři a jeho podadresářích
	  `git checkout .`
   * [`git merge`](https://git-scm.com/docs/git-merge) - tento příkaz aplikuje změny z větve na jinou větev.
     Aplikování změn z větve master na aktuální větev `git merge master`.
   * [`git rebase`](https://git-scm.com/docs/git-merge) - tento příkaz taky aplikuje změny z větve na jinou větev.
     Ale dělá to tak, že postupně aplikuje změny z cilové větve na zdrojovou.
   * [`git cherry-pick`](https://git-scm.com/docs/git-cherry-pick) - vezme commit z identifikovaný SHA1 a použije ho
     na aktuální větvi.

## Důležité příkazy pro práci se vzdálenými repozitáři

   * [`git push`](https://git-scm.com/docs/git-push) - odesílá změny z aktuální větve resp. ze všech větví, záleží
     na konfiguraci do vzdáleného repozitáře.
   * [`git fetch`](https://git-scm.com/docs/git-fetch) - tento příkaz stahuje změny ze vzdáleného repozitáře do
     vašeho lokálního repozitáře. Jde jen o stažení do vašich lokálních zrcadel vzdálených repozitářů. Nedocházi
	 přitom k aplikování změn ze vzdálených repozitářů do lokálních větví.
   * [`git remote`](https://git-scm.com/docs/git-remote) - tento příkaz nastavuje vzdálené repozitáře

#Uživatelské rozhraní

Git obsahuje CLI klenta, což je to sada příkazů `git [command]` Tento klient je oficiálním  klientem gitu
a rozhodně stojí za to se ho naučit. Pokud se jej načíte tak naplno využijete možnosti gitu a budete mít
přístup ke všem jeho možnostem. Oproti některým GUI klientům máte vždy plnou vládu nad tím co s se s vaším
kódem děje.

Git obsahuje i webové klienty, což jsou více než klienti rozhraní pro repozitáře uložené na serveru. Naprosto
výsadní postavení mezi ními má [github](https://github.com).

[Tady](https://git.wiki.kernel.org/index.php/InterfacesFrontendsAndTools) najdete seznam všech možných
nástrojů a knohove, které nějak s gitem souvisí.

#Internals aneb kde je git repozitář

Když půjdete do adresáře s git repozitářem, tak tam najdete adresář .git, který obsahuje databázi
gitu pro daný projekt a jeho případné konfigurace

  * .git/objects databaze changesetu - kazdy changeset je identifikovan SHA-1 hashem Ukaž git log/show
  * .git/config - definice vzdálených repozitářů a lokalni konfigurace
  * .git/hooks - skripty, programy spouštěné při různých akcích a client side (pre-commit, commit, pre/post-merge/rebase ...,) server side - pre/post-receive, update
  * .git/info/exclude - lze bez vytváření souborů .gitignore ignorovat některé souboru hodí, se když upravujete cizí kód a nechcete měnit .gitignore nebo to chcete tajit
  * .git/modules - definice submodulů, submodul, je něco jako podřízený repozítář
  * v ~/.gitconfig aliasy, definice vzdálených repozitářů,vase nacionale, GPG klic, nastaveni jednotlivych prikazu ...

Git si můžete rozšířit i o vlastní příkazy, pokud si do `PATH` uložíte skript, který se jmenuje
git-command, kde command je libovolný název příkazu, který s gitem spouštět jako příkaz gitu.
