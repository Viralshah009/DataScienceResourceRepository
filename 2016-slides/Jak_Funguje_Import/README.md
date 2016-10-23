# Jak funguje import
## Petr Viktorin


Kontaktujte mě na encukou@gmail.com :)

### Nezodpovězené Otázky ze sli.do

#### Ake su najznamejsie kniznice, projekty ktore manipuluju import machinery a robia nieco nestandardne.

Nejznámější asi pytest a setuptools, ale tady je pár *zajímavých*:

* [MacroPy](https://pypi.python.org/pypi/MacroPy) přidává do Pythonu makra
* [hy](http://docs.hylang.org/en/latest/) umožňuje psát pythoní moduly v Lispu
* [Setuptools](https://pypi.python.org/pypi/setuptools) mají koncept "Namespace Packages", (který je sice dnes zastaralý, ale zase funguje na Pythonu 2)
* [enaml](http://nucleic.github.io/enaml/docs/) umí přímo importovat importovat definice UI
* David Beazley měl na PyConu [workshop](http://www.dabeaz.com/modulepackage/index.html) kde prezentoval svůj urlimport

#### Will we have hot code reload in python someday?

Is [importlib.reload](https://docs.python.org/3/library/importlib.html#importlib.reload) not hot enough? :)

[IPython's %autoreload](http://ipython.readthedocs.org/en/stable/config/extensions/autoreload.html) also solves a similar problem.

It seems that a fully robust solution to the many problems that people want when they say "reload" is probably incompatible with Python's object model.

#### Pokud modifikuju path kvuli nacitani, je velky rozdil jestli to pridam na zacatek nebo na konec?

Je. Pokud je na cestě víc modulů stejného jména, najde se jen ten první.
Takže když něco přidám na začátek, zastíní to všechny ostatní moduly včetně std. knihovny.

#### V e-mailu bylo napsáno, že v pátek bude vše ve slovenštině a Petr mluví česky. Je to vůbec možné? :D

Na PyCon SK je možné všechno!

#### Sú nejaké ďalšie rozdiely medzi py2 a py3?

Spousta. V Pythonu 2 fungují importy docela jinak, a navíc je referenční implementace importování napsaná v C, takže tomu moc lidí nerozumí :)

#### import braces

Stále bez šance. Leda bys chtěl slovník.

### Licence

Slajdy jsou pod licencí CC-BY-SA 3.0 Unported.
