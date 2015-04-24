# Ares_util #

[![Supported Python versions](https://pypip.in/py_versions/ares_util/badge.svg)]( https://pypi.python.org/pypi/ares_util/)
&nbsp;
[![Travis CI Badge](https://api.travis-ci.org/illagrenan/ares_util.png)](https://travis-ci.org/illagrenan/ares_util)
&nbsp;
[![Coverage Status](https://coveralls.io/repos/illagrenan/ares_util/badge.png)](https://coveralls.io/r/illagrenan/ares_util)
&nbsp;
[![PyPI version](https://badge.fury.io/py/ares_util.png)](http://badge.fury.io/py/ares_util)
&nbsp;
[![License](https://pypip.in/license/ares_util/badge.png)](https://pypi.python.org/pypi/ares_util/)
&nbsp;
[![Requirements Status](https://requires.io/github/illagrenan/ares_util/requirements.svg?branch=master)](https://requires.io/github/illagrenan/ares_util/requirements/?branch=master)

<!---
[![Wheel Status](https://pypip.in/wheel/ares_util/badge.svg)](https://pypi.python.org/pypi/ares_util/)
&nbsp;
[![Egg Status](https://pypip.in/egg/ares_util/badge.svg)](https://pypi.python.org/pypi/ares_util/)
&nbsp;
-->

## Představení

Jednoduchý nástroj pro validaci<sup>1</sup> českého IČ. U předaného čísla je nejdříve ověřen jeho kontrolní součet (viz [Reference](https://github.com/illagrenan/ares_util/master/README.md#reference)) a dle výsledku se následně zasílá požadavek na [ARES XML API](http://wwwinfo.mfcr.cz/ares/ares_xml.html.cz).

Pokud je IČ validní, vrací nástroj `dict` se základními údaji o firmě (obchodní název, adresa).


> Doporučuji prostudovat [Podmínky provozu ARES API](http://wwwinfo.mfcr.cz/ares/ares_podminky.html.cz), zejména pak:
> 
> Ministerstvo financí vyhrazuje právo omezit nebo znemožnit přístup k www aplikaci ARES uživatelům, kteří:
> * odešlou k vyřízení více než 1000 dotazů v době od 8:00 hod. do 18:00 hod.,
> * odešlou k vyřízení více než 5000 dotazů v době od 18:00 hod. do 8:00 hod. rána následujícího dne,
> * opakovaně posílají nesprávně vyplněné dotazy,
> * opakovaně posílají stejné dotazy,
> * mají větší počet současně zadaných dotazů (pro automatizované XML dotazy),
> * obcházejí povolené limity využíváním dotazování z většího množství IP adres,
> * automatizovaně propátrávají databázi náhodnými údaji nebo generují většinu nesprávných dotazů.


## Instalace

**A)** Stabilní verze (doporučeno)

```shell
pip install --upgrade ares-util
```

**B)** Vývojová verze

```shell
pip install --upgrade git+git://github.com/illagrenan/ares_util.git
```

## Použití ##

> Doplňek podporuje Python `2.7.*`, od verze `0.1.6` experimentálně Python `3.3` a `3.4`.

```shell
python
>>> from ares_util.ares import call_ares
>>> call_ares(42)
False
>>> call_ares('68407700')
{
    u'legal': {
        u'company_vat_id': u'CZ68407700',
        u'company_name': u'České vysoké učení technické v Praze',
        u'legal_form': u'601', u'company_id': u'68407700'
    },
    u'address': {
        u'city': u'Praha',
		u'region': u'Hlavní město Praha',
		u'street': u'Zikova 1903/4',
        u'city_part': u'Dejvice',
		u'zip_code': u'16000'
    }
}
```

### Django podpora

> Testováno pro Django `>=1.5.*,=<1.8`.

K dispozi jsou dva [Django validátory](https://docs.djangoproject.com/en/dev/ref/validators/) formulářových polí:

* `czech_company_id_numeric_validator` - Ověřuje, zda IČ splňuje statické parametry, tj. 7 nebo 8 číslic a kontrolní součet.
* `czech_company_id_ares_api_validator` - Platnost IČ ověřuje pomocí ARES API. Tento validátor před ARES požadavkem rovněž ověřuje statické parametry, proto by **neměly být použity oba validátory zároveň**.

```python
# settings.py
INSTALLED_APPS = (
    # ...
    'ares_util',
)

```
```python
from ares_util.validators import czech_company_id_numeric_validator, czech_company_id_ares_api_validator
from django import forms

# forms.py
class DemoForm(forms.Form):
    company_id = forms.IntegerField(required=True, validators=[czech_company_id_ares_api_validator])

```

# Reference
1. http://www.abclinuxu.cz/blog/bloK/2008/10/kontrola-ic, http://phpfashion.com/jak-overit-platne-ic-a-rodne-cislo

# Technické informace

XML response z ARESu je zpracována pomocí [xmltodict](https://github.com/martinblech/xmltodict).

# Licence

The MIT License (MIT)

Copyright (c) 2013&ndash;2015 Vašek Dohnal

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
