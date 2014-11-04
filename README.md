Symfony2

Struktura katalogów archiwum Symfony:
app/ - pliki konfiguracyjne aplikacji,
bin/ - polecenia wsadowe pakietów dodatkowych
src/ - kod źródłowy aplikacji
vendor/ - pakiety dodatkowe – np. doctrine, twig, swiftmailer
web/ - folder zawierający główny kontroler aplikacji – skrypt app.php, CSS, pliki graficzne, javascript. Jedyny dostępny publicznie katalog za pomocą HTTP.

Assets – w dokumentacji Symfony2 określane są style.css, .js, pliki graficzne .jpg .gif.

Symfony2 jest zaimplementowane obiektowo z wykorzystaniem przestrzeni nazw (namespace). 
Aplikacja tworzona w Symfony2 jest podzielona na:
	pakiety (bundle),
	kontrolery (controller),
	akcje (action)
	widoki (view).
	
Przestrzenie nazw umożliwiają stosowanie  wieloczłonowych nazw klas – struktura drzewa np.:
Symfony\Component\ClassLoader\UniversalClassLoader
Przestrzenie nazw tworzone poprzez umieszczanie klasy w osobnych katalogach i dołączaniu deklaracji namespace.
Pakiety są niezależnymi fragmentami aplikacji. Pakiety są niezależne od konkretnej aplikacji. Nazwa pakietu musi kończyć się przyroskiem Bundle np.: Slomka/HelloworldBundle
Kontroler odpowiada za przetworzenie żądania HTTP i wygenerowaniu odpowiedzi.
Widoki to pliki, które zawierają kod HTML oraz specjalne instrukcje umieszczające w kodzie HTML wartości zmiennych. Twig jest domyślnym językiem przetwarzania - .html.twig.
Biała strona podczas uruchamiani aplikacji:
1. czyszczenie cache,
2. restart apache,
3. sprawdzenie web/config.php.
czyszczenie cache:
php app/console cache:clear
tworzenie pakietu – bundla:
php app/console gernerate:bundle
sprawdzanie poprawności instalacji PHP:
php app/check.php
edycja pliku np. php.ini:
gksudo gedit /etc/php5/apache2/php.ini
restart serwera apache:
sudo /etc/init.d/apache2 restart
Adnotacje są specjalnymi komentarzami umieszczanymi w kodzie PHP, któ©e konfigurują pakiet. Zapisywane są w plikach kontrolerów. Kod kontrolera+opcje dostępne są w jednym miejscu. W adnotacjach stosujemy cudzysłowa.
Generowanie bundla (pakietu) w sposób wsadowy (bez trybu interaktywnego):
php app/console generate:bundle -–namespace=MyHelloWorldBundle --dir=src -–no-interaction
.htaccess->deny from all->blokuje dostęp do zawartości folderu xxx/src/ za pomocą protokołu HTTP (nie da się zajrzeć za pomocą przeglądarki).
Web Debug Toolbar – pasek narzędzi developerskich – wyświetlany jest tylko w trybie deweloperskim i gdy strona zawiera znaczniki <html> i <body>.
Środowisko jest zestawem opcji konfiguracyjnych ustalających parametry pracy aplikacji. 
app.php – front kontroler uruchamiający aplikację w środowisku prod;
app_dev.php - front kontroler uruchamiający aplikację w środowisku dev.
W pliku tw app.php i app_dev.php torzony jest obiekt $kernel poprzez konstruktor, który posiada dwa parametry, pierwszy jest nazwą środowiska, drugi wyłącza/włącza wyświetlanie komunikatów diagnostycznych:
$kernel = new AppKernel('prod', false);
$kernel = new AppKernel('dev', true);
Instrukcja namespace w klase – deklaruje przesrzenie nazw,
instrukcja use – dołącza wybrane klasy.
ORM – mapowanie obiektowo – relacyjne.
 W widokach ścieżki do zasobów z folderu web/ generujemy wywołując funckję pomocniczą asset(), np.:
<script src=”{{ asset('skrypt.js') }}”</script>

kopiowanie zasobów .css, .js, .jpg z …/Resource/public do /web:
php app/console assets:install web
Tworzenie nowego projektu Symfony2 przykład:
$ composer create-project symfony/framework-standard-edition /path/to/webroot/Symfony 2.1.x-dev

Instalacja NetBeans:
1. Download strona netbeans.org
2. Przechodzę do folderu Pobrane
3. Wykonuję dwie komendy: chmod u+x netbeans-8.0-javase-linux.sh
4. ./netbeans-8.0-javase-linux.sh

Publikowanie zasobów w folderze web/:
php app/console assets:install web

Twig – załączanie dekoracji widoku akcji z folderu np. My/LoremBundle/Resource/views:
{% extends ”MyLoremBundle::layout.html.twig” %}
{% block content %} //ustala treść któ©a zostanie umieszczona w 					  bloku
{% endblock %}
MyLoremBundle – nazwa pakietu
:: - pusta nazwa kontrolera (domyślnie pobierze z szablon z folderu views/ pakietu LoremBundle)
layout.html.twig – nazwa pakietu.

Domyślna nazwa widoków tworzona jest poprzez usunięcie przyrostka Action z nazwy metody i dodanie rozszerzenia .html.twig np.:
widok akcji dolorAction() kontrolera Ipsum
src/My/LoremBundle/Resources/views/Impus/dolor.html.twig
odwołanie do szablonu Twig (powyżej)odbywa się poprzez nazwę logiczną widoku:
MyLoremBundle:Ipsum:dolor.html.twig
[producent][pakiet][]Bundle:[kontroler]:[akcja].html.twig

Pakiet DoctrineFixturesBundle ułatwi a wypełnianie bazy danych na podstawie plików.
Pobranie wszystkich pakietów wymienionych w katalogu [projekt]/deps:
php bin/vendors install
Usuwanie folder .git/ które zawierają historię pakietów:
find vendor -name .git -type d -exec rm -fr {} \;
Pakiety rejestrujemy w pliku [projekt]/app/AppKernel.php. np.:
$bundles = array( 
... 
new Symfony\Bundle\DoctrineFixturesBundle\DoctrineFixturesBundle(), 
);

Automatyczne ładowanie klas konfigurujemy w skrypcie [projekt]/app/autoload.php. np.:
$loader->registerNamespaces(array( 
... 
'JMS' 		=> __DIR__.'/../vendor/bundles', 
'Doctrine\\Common\\DataFixtures' => __DIR__.'/../vendor/doctrine-fixtures/lib', 
'Doctrine\\Common' => __DIR__.'/../vendor/doctrine-common/lib', 
... 
));

W Symfony 2 klasę dostępu do bazy danych nazywamy modelem. 
Generowanie klasy dostępu do bazy danych:
php app/console generate:doctrine:entity
ścieżka do takiej klasy np.: src/My/FrontendBundle/Entity/Name.php
Metody get() służą do odczytu wartości właściwości, a metody set() — do ustalenia nowej wartości właściwości. Dostęp do właściwości prywatnych możliwy j est jedynie poprzez metod get() i set().
Rodzaje nazw logicznych: 
 logiczne nazwy widoków (np. MyFrontendBundle:Default:index.html.twig), 
 logiczne nazwy kontrolerów (np. MyFrontendBundle:Default:index), 
 logiczne nazwy modeli (np. MyFrontendBundle:Name).
Utworzenie tabeli w bazie (np. name według entity):
php app/console doctrine:schema:update –force
Wykonanie klasy wypełniającej tabelę danymi:
php app/console doctrine:fixtures:load

Usuwanie z systemu niepotrzebnych pakietów:
sudo apt-get autoremove

Doctrine2
doctrine:schema:create 
doctrine:schema:drop (nie usuwa nieaktualnych już informacji np. zmiany nazwy tabeli).
doctrine:schema:update - uaktualnia strukturę bazy na podstawie plików z folderów Entity/. (lub polecenia odpowiednio php app/console doctrine:schema:drop i php app/console doctrine:schema:create).
Pierwsza komenda tworzy, druga usuwa, a trzecia uaktualnia bazę danych.
Konfiguracja bazy w pliku app/config/parameters.ini
Parametr --force zabezpiecza przed przypadkowym usunięciem ważnych danych.

Wygenerowania pojedynczej klasy dostępu do bazy:
php app/console generate:doctrine:entity
Struktura bazy danych aplikacji jest ustalona wyłącznie klasami z folderów Entity/.
Adnotacje: 
@ORM\Table() 
@ORM\Entity 
oraz: 
@ORM\Column(name="caption", type="string", length=255) odpowiadają za wygenerowanie odpowiedniego kodu SQL.
Do ustalenia nazwy tabeli bazy danych odpowiadającej danej klasie Entity służy adnotacja 
@Table.
Mamy klasę np. klasa src/My/FrontendBundle/Entity/Lorem.php:
1. Dodajemy nową właściwość np. ipsum : 
/** 
* @var string $ipsum 
* 
* @ORM\Column(name="ipsum", type="string", length=255) 
*/ 
private $ipsum;
2. Generujemy w klasie Lorem.php metody getIpsum() oraz setIpsum():
php app/console doctrine:generate:entities My
3. Uaktualniamy strukrutrę bazy danych:
doctrine:schema:update –force
Wartość DEFAULT definiujemy następująco: 
private $imie = "jan";

Do pobrania wszystkich rekordów z bazy danych służy metoda findAll().
Uruchamianie plików .sh:
sh tworzenie-pustej-bazy-danych.sh
Klasy Entity podlegają mapowaniu na rekordy. Klasy Repository służą do wyszukiwania rekordów.
Operacje wykonywane na obiektach podczas uzyskiwania dostępu do bazy danych są 
nazywane zachowaniami (ang. behaviours). Najczęściej stosowanymi zachowaniami są: 
timestampable — podczas zapisywania obiektu w bazie danych generowana 
jest data zapisu. 
sluggable — podczas zapisywania obiektu w bazie danych generowany jest 
specjalny identyfikator slug, który wykorzystujemy w przyjaznych adresach URL. 
translatable — podczas odczytu rekordu z bazy danych dla wybranych 
kolumn wybierane są odpowiednie wersje językowe.
Integrację biblioteki DoctrineExtensions w projekcie Symfony 2 ułatwia pakiet 
StofDoctrineExtensionsBundle. Pakiet StofDoctrineExtensionsBundle zawiera uproszczone mechanizmy konfiguracji zachowań: 
tree, 
translatable, 
sluggable, 
timestampable, 
loggable.
