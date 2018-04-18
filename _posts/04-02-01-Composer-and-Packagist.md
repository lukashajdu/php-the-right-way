---
isChild: true
anchor:  composer_and_packagist
---

## Composer a Packagist {#composer_and_packagist_title}

Composer je **geniálny** správca závislostí pre PHP. Vymenúva závislosti vášho projektu v súbore `composer.json` a s niekoľkými jednoduchými príkazmi za vás dokáže stiahnuť projektové závislosti a nastaviť samozavádzanie. Composer je analogický ku NPM vo svete node.js alebo analogický ku Bundler vo svete Ruby.

Dnes existuje už mnoho PHP knižníc, ktoré sú kompatibilné s Composerom a teda sú pripravené pre použitie vo vašom projekte. Tieto „balíčky“ sú uvedené v oficiálnom repozitári [Packagist] pre Composer-kompatibilné PHP knižnice.

### Ako nainštalovať Composer

Composer je možné nainštalovať lokálne (vo vašom aktuálnom pracovnom adresári) alebo globálne (napr. /usr/local/bin - odporúčané).
Predpokladajme, že chcete nainštalovať Composer globálne:

{% highlight console %}
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
{% endhighlight %}

**Poznámka:** Ak vyššie uvedené riadky zlyhajú kvôli právam, opätovne spustite riadok `mv` začínajúci so `sudo`.

Vyššie uvedené riadky stiahnu `composer.phar` (PHP binárny archív). Pre správu vašich závislostí ho môžete spúšťať s `php`.

**Bezpečnostná poznámka:** Ak chcete vložiť uvedený kód priamo do interpretera, najprv si ho tu prečítajte aby ste sa presvedčili, že je bezpečný.

#### Inštalácia na systéme Windows

Ak ste používateľ systému Windows tak najjednoduchším spôsobom inštalácie je využitie inštalátora [ComposerSetup], ktorý vykoná globálnu inštaláciu a nastaví `$PATH` takým spôsobom, že budete môcť zavolať `composer` z akéhokoľvek adresára vo vašom príkazovom riadku.

### Ako nainštalovať Composer (manuálne)

Manuálna inštalácia Composer je pokročilá technika. Napriek tomu, existujú rôzne dôvody prečo by vývojár uprednostnil túto metódu voči použitiu interaktívnej inštalačnej rutiny. Interaktívna inštalácia kontroluje vašu PHP inštaláciu aby sa presvedčila, že:

- používa sa dostatočná veria PHP
- `.phar` súbory sú spúšťané správne
- oprávnenia určitých adresárov sú dostatočné
- určité problematické rozšírenia nie sú povolené
- určité nastavenia (`php.ini`) sú nastavené 

Keďže manuálna inštalácia nevykonáva žiadne kontroly, musíte sa rozhodnúť či tento kompromis je pre vás výhodný. Nižšie je uvedené ako je možné získať Composer manuálne:

{% highlight console %}
curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
chmod +x $HOME/local/bin/composer
{% endhighlight %}

Cesta `$HOME/local/bin` (alebo adresár vášho výberu) by mala byť uvedená vo vašej premennej prostredia `$PATH`. Toto vyústi do dostupnosti príkazu `composer`.

Keď natrafíte v dokumentácii na príkaz `php composer.phar install`, môžete namiesto neho použiť aj príkaz:

{% highlight console %}
composer install
{% endhighlight %}

Táto sekcia predpokladá, že ste nainštalovali Composer globálne.

### Ako definovať a nainštalovať závislosti

Composer udržuje zoznam projektových závislostí v súbore `composer.json`. Tento súbor môžete upravovať ručne alebo využívať priamo Composer. Príkaz `composer require` pridáva projektovú závislosť a ak súbor `composer.json` neexistuje, vytvorí ho. Tu je príklad, ktorý pridáva [Twig] ako závislosť vášho projektu:

{% highlight console %}
composer require twig/twig:~1.8
{% endhighlight %}

Alternatívne, príkaz `composer init` vás bude sprevádzať vytvorením celého súboru `composer.json` pre váš projekt. V každom prípade, keď už raz vytvoríte súbor `composer.json`, môžete povedať Composer-u aby stiahol a nainštaloval vaše závislosti do adresára `vendor/`. Toto platí aj pre projekty, ktoré ste stiahli a už obsahujú súbor `composer.json`:

{% highlight console %}
composer install
{% endhighlight %}

Ďalej, pridajte tento riadok do primárneho PHP súboru vašej aplikácie - toto povie PHP, aby použil Composer samozavádzanie pre vaše projektové závislosti:

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

Teraz môžete používať vaše projektové závislosti - budú automaticky načítané na vyžiadanie.

### Aktualizácia vašich závislostí

Composer vytvára súbor zvaný `composer.lock`, ktorý obsahuje informáciu o presnej verzii každého balíka, ktorý bol stiahnutý keď ste prvý krát spustili príkaz `composer install`. Ak zdielate váš projekt s ostatnými programátormi a súbor `composer.lock` je súčasťou vašej distribúcie, tak keď títo programátori spustia príkaz `composer install`, dostanú rovnaké verzie balíkov ako vy. 
Pre aktualizovanie vašich závislostí spustite príkaz `composer update`.

Toto je najviac užitočné, keď definujete vaše flexibilné požiadavky na verzie. Napríklad, požiadavka na verziu `~1.8` znamená „čokoľvek novšie ako `1.8.0` ale staršie ako `2.0.x-dev`“. Taktiež môžete využiť náhradný znak `*`, teda `1.8.*`. Príkaz `composer update` teraz zaktualizuje všetky vaše závislosti na najnovšiu verziu ktorá spĺňa definované obmedzenia. 

### Aktualizácia notifikácii

Pre prijímanie notifikácii o vydaniach nových verzií sa môžete registrovať na [VersionEye], webovej službe, ktorá monitoruje a kontroluje `composer.json` vo vašom GitHub a BitBucket účte a pri vydaní novšej verzie balíka posiela emailové notifikácie. 

### Kontrolovanie vašich závislostí pre bezpečnostné problémy

[Security Advisories Checker] je webová služba a nástroj príkazového riadka - obe vyšetria váš súbor `composer.lock` a povedia vám či potrebujete aktualizovať nejakú z vašich závislostí.

### Zaobchádzanie s globálnymi závislosťami

Composer môže taktiež spracovávať globálne závislosti a ich binárne súbory. Použitie je priame - všetko čo potrebujete je pridať slovo `global`. Napríklad, ak by ste chceli nainštalovať balík PHPUnit a mať ho dostupný globálne, spustili by ste tento príkaz:

{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}

Toto vytvorí adresár `~/.composer`, kde budú sídliť vaše globálne závislosti. Aby boli vaše nainštalované balíky dostupné všade, musíte ešte pridať cestu `~/.composer/vendor/bin` do premennej `$PATH`.

* [Learn about Composer]

[Packagist]: http://packagist.org/
[Twig]: http://twig.sensiolabs.org
[VersionEye]: https://www.versioneye.com/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Learn about Composer]: http://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe
