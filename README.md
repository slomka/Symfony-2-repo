<h1><b>Symfony2</h1>
<p>Struktura katalogów archiwum Symfony:</b>

	app/ - pliki konfiguracyjne aplikacji,
	bin/ - polecenia wsadowe pakietów dodatkowych
	src/ - kod źródłowy aplikacji
	vendor/ - pakiety dodatkowe – np. doctrine, twig, swiftmailer
	web/ - folder zawierający główny kontroler aplikacji – skrypt app.php, CSS, pliki graficzne, javascript. 		       Jedyny dostępny publicznie katalog za pomocą HTTP.
	
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

Pakiety są niezależnymi fragmentami aplikacji. Pakiety są niezależne od konkretnej aplikacji. 

Nazwa pakietu musi kończyć się przyroskiem Bundle np.: Slomka/HelloworldBundle
Kontroler odpowiada za przetworzenie żądania HTTP i wygenerowaniu odpowiedzi.

Widoki to pliki, które zawierają kod HTML oraz specjalne instrukcje umieszczające w kodzie HTML wartości zmiennych. 

Twig jest domyślnym językiem przetwarzania - .html.twig.

Biała strona podczas uruchamiani aplikacji:
	1. czyszczenie cache,
	2. restart apache,
	3. sprawdzenie web/config.php.

<b>czyszczenie cache:</b>
php app/console cache:clear

<b>tworzenie pakietu – bundla:</b>
php app/console gernerate:bundle

<b>sprawdzanie poprawności instalacji PHP:</b>
php app/check.php

<b>edycja pliku np. php.ini:</b>
gksudo gedit /etc/php5/apache2/php.ini

<b>restart serwera apache:</b>
sudo /etc/init.d/apache2 restart

Adnotacje są specjalnymi komentarzami umieszczanymi w kodzie PHP, któ©e konfigurują pakiet. Zapisywane są w plikach kontrolerów. Kod kontrolera+opcje dostępne są w jednym miejscu. W adnotacjach stosujemy cudzysłowa.

<b>Generowanie bundla (pakietu) w sposób wsadowy (bez trybu interaktywnego):</b>
php app/console generate:bundle -–namespace=MyHelloWorldBundle --dir=src -–no-interaction

.htaccess->deny from all->blokuje dostęp do zawartości folderu xxx/src/ za pomocą protokołu HTTP (nie da się zajrzeć za pomocą przeglądarki).

Web Debug Toolbar – pasek narzędzi developerskich – wyświetlany jest tylko w trybie deweloperskim i gdy strona zawiera znaczniki <html.> i <body.>. 

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
//<.script src=”{{ asset('skrypt.js') }}”</script>


<b>kopiowanie zasobów .css, .js, .jpg z …/Resource/public do /web:</b>
php app/console assets:install web

<b>Tworzenie nowego projektu Symfony2 przykład:</b>
$ composer create-project symfony/framework-standard-edition /path/to/webroot/Symfony 2.1.x-dev


Instalacja NetBeans:
	1. Download strona netbeans.org
	2. Przechodzę do folderu Pobrane
	3. Wykonuję dwie komendy: chmod u+x netbeans-8.0-javase-linux.sh
	4. ./netbeans-8.0-javase-linux.sh

<b>Publikowanie zasobów w folderze web/:</b>
php app/console assets:install web

<b>Twig – załączanie dekoracji widoku akcji z folderu np. My/LoremBundle/Resource/views:</b>
{% extends ”MyLoremBundle::layout.html.twig” %}
{% block content %} //ustala treść któ©a zostanie umieszczona w bloku
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

<b>Pobranie wszystkich pakietów wymienionych w katalogu [projekt]/deps:</b>
php bin/vendors install

<b>Usuwanie folder .git/ które zawierają historię pakietów:</b>
find vendor -name .git -type d -exec rm -fr {} \;

<b>Pakiety rejestrujemy w pliku [projekt]/app/AppKernel.php. np.:</b>
$bundles = array( 
... 
new Symfony\Bundle\DoctrineFixturesBundle\DoctrineFixturesBundle(), 
);

<b>Automatyczne ładowanie klas konfigurujemy w skrypcie [projekt]/app/autoload.php. np.:</b>
$loader->registerNamespaces(array( 
... 
'JMS' 		=> __DIR__.'/../vendor/bundles', 
'Doctrine\\Common\\DataFixtures' => __DIR__.'/../vendor/doctrine-fixtures/lib', 
'Doctrine\\Common' => __DIR__.'/../vendor/doctrine-common/lib', 
... 
));


W Symfony 2 klasę dostępu do bazy danych nazywamy modelem. <b>Generowanie klasy dostępu do bazy danych:</b>
php app/console generate:doctrine:entity

ścieżka do takiej klasy np.: src/My/FrontendBundle/Entity/Name.php

Metody get() służą do odczytu wartości właściwości, a metody set() — do ustalenia nowej wartości właściwości. Dostęp do właściwości prywatnych możliwy j est jedynie poprzez metod get() i set().

Rodzaje nazw logicznych: 
	 logiczne nazwy widoków (np. MyFrontendBundle:Default:index.html.twig), 
	 logiczne nazwy kontrolerów (np. MyFrontendBundle:Default:index), 
	 logiczne nazwy modeli (np. MyFrontendBundle:Name).

<b>Utworzenie tabeli w bazie (np. name według entity):</b>
php app/console doctrine:schema:update –force

</b>Wykonanie klasy wypełniającej tabelę danymi:</b>
php app/console doctrine:fixtures:load

<b>Usuwanie z systemu niepotrzebnych pakietów:</b>
sudo apt-get autoremove


<b>Doctrine2</b>
doctrine:schema:create 
doctrine:schema:drop (nie usuwa nieaktualnych już informacji np. zmiany nazwy tabeli).
doctrine:schema:update - uaktualnia strukturę bazy na podstawie plików z folderów Entity/. (lub polecenia odpowiednio php app/console doctrine:schema:drop i php app/console doctrine:schema:create).

Pierwsza komenda tworzy, druga usuwa, a trzecia uaktualnia bazę danych.

Konfiguracja bazy w pliku app/config/parameters.ini

Parametr --force zabezpiecza przed przypadkowym usunięciem ważnych danych.


<b>Wygenerowania pojedynczej klasy dostępu do bazy:</b>
php app/console generate:doctrine:entity

Struktura bazy danych aplikacji jest ustalona wyłącznie klasami z folderów Entity/.

Adnotacje: 
	@ORM\Table() 
	@ORM\Entity 
	oraz: 
	@ORM\Column(name="caption", type="string", length=255) odpowiadają za wygenerowanie odpowiedniego kodu 		SQL.
	
Do ustalenia nazwy tabeli bazy danych odpowiadającej danej klasie Entity służy adnotacja @Table.

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

<b>Uruchamianie plików .sh:</b>
sh tworzenie-pustej-bazy-danych.sh

Klasy Entity podlegają mapowaniu na rekordy. Klasy Repository służą do wyszukiwania rekordów.

Operacje wykonywane na obiektach podczas uzyskiwania dostępu do bazy danych są nazywane zachowaniami (ang. behaviours). Najczęściej stosowanymi zachowaniami są: 
	timestampable — podczas zapisywania obiektu w bazie danych generowana jest data zapisu. 
	sluggable — podczas zapisywania obiektu w bazie danych generowany jest specjalny identyfikator slug, który 		    wykorzystujemy w przyjaznych adresach URL. 
	translatable — podczas odczytu rekordu z bazy danych dla wybranych kolumn wybierane są odpowiednie wersje 		       językowe.

Integrację biblioteki DoctrineExtensions w projekcie Symfony 2 ułatwia pakiet StofDoctrineExtensionsBundle. Pakiet StofDoctrineExtensionsBundle zawiera uproszczone mechanizmy konfiguracji zachowań: 
	tree, 
	translatable, 
	sluggable, 
	timestampable, 
	loggable.
