I) WantedKeyword (wk) Python Type Checking tool (Python3)


Full doc at: www.kikonf.org/wk

1/ INTRODUCTION


What is it ?

wk allows to check a collection of fields, against their characteristics, defined into wk expressions.

A wk expression (a.k.a. wk definition) is a dictionary, where each key defines a specific control to be operated on the field.

e.g.1:
age = {'*type':int}

This expression Will check the field named: age using the control: *type defined into the wk definition: {'*type':int}.

{'*type':int}: means that age must be an integer otherwise an exception is raised.

e.g.2:
vegan = {'*type': bool}
Means that the field vegan must be a bool.

They are cumulative ex:
age = {'*type':int, '*ge':26, '*ne': 35, '*value': 40}
As we'll see later.


Running checks:

Checks can be performed in two ways.

a) Using the check method of the wk module:

Test this into the Python interpreter:
import wk

name = 'john'
age=45
p=wk.WKS()
p.name={'*type':'str'}
p.age={'*type':'int', '*value':30}
wk.check (wks=p, kws={'name':name, 'age':age})
>>> p.name
'john'
>>> p.age
45

Try this:
p=wk.WKS()
p.age={'*type':'int'}
wk.check (wks=p, kws={'age': 'hello'})

To see what happens !

This can be comfortably embedded into a function or method:

def hello(name, age):
    import wk
    p=wk.WKS()
    p.name={'*type':'str'}
    p.age={'*type':'int', '*value':30}
    wk.check (wks=p, kws={'name':name, 'age':age})

    print('Function hello! name: %s, age: %s' % (p.name, p.age))

>>> hello('john', None)
Function hello! name: john, age: 30


b) Using the cp annotation of the wk module:


from wk import cp
@cp({'*type': 'str'}, {'*type':'int', '*value':30})
def hello(name, age):
    print('Function hello! name: %s, age: %s' % (name, age))

>>> hello('john')
Function hello! name: john, age: 30



2/ USING THE CHECK FUNCTION


2.1/ A simple function hello

import wk
def hello(*args, **keywords):
    p=wk.WKS()
    p.name={'*type':'str'}
    p.age={'*type':'int', '*value':30}
    wk.check (wks=p, kws=keywords )

    print ('function: hello !')
    print ('-----------------')
    print('name: %s' % p.name)
    print('age: %s' % p.age)
    print('parameters as dict: %s' % wk.getAsDict(p))

>>> hello(name='john', age=25)

function: hello !
-----------------
name: john
age: 25
parameters as dict: {'age': 25, 'name': 'john'}
>>>

- With default value: *value:

>>> hello(name='john')
function: hello !
-----------------
name: john
age: 30 # <-- Default value for age
parameters as dict: {'age': 30, 'name': 'john'}


Lines explained:
Line 3: p=wk.WKS(): An WKS instance (wk.WKS is alias for wk.WantedKeyword) is created with no Parameter.
Line 4-5: p.name={'*type':'str'}: p. : One declares wk expressions on the fly on the wk instance.
Line 6:wk.check (wks=p, kws=kws ): The wk module's function: check, runs on each Attribute of the wk.WKS instances: "name" and "age".
Regarding the wk expression defined for each one.
The values are retreives from the kws parameter.
The final values are stored on the attributes of the WKS instance (p).

a) wk.check retreives the user value for the attribute name, from kws['name'].
b) wk.check verifies this value on the wk expresion: {'*type':'str'}, defined for attribute name.
c) wk.check applies the final value to p.name


- More on extra Parameters:

If you run the following call, you'll see that no error is generated althought parameter pc is not defined.
>>> hello(name='john', age=25, pc='12345')
To reject extra parameter you must use the keyword: strict.


2.2/ Same function but using strict

def hello(*args, **kws):
    p=wk.WKS()
    p.name={'*type':'str'}
    p.age={'*type':'int', '*value':30}
    wk.check (wks=p, kws=kws, strict=True )

    print('Function hello! parameters as dict: %s' % wk.getAsDict(p))

- Using the extra parameter: pc causes an error, because of strict=True:

>>> hello(name='john', age=25, pc='12345')
Traceback (most recent call last):
...
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On strick check:
Unsupported Attribute: pc !. Supported Attributes are: age, name. <==



As you can see into this excerpts from the previous exception: "SubClass:None SubMethod:None",
The calling class and method is not traced.
To trace them one should use class_exit and method_exit as follows.


2.3/ Same function but using class_exit and method_exit


def hello(*args, **kws):
    p=wk.WKS()
    p.name={'*type':'str'}
    p.age={'*type':'int', '*value':30}
    wk.check (wks=p, kws=kws, strict=True, class_exit='Main', method_exit='hello')

    print('Function hello! parameters as dict: %s' % wk.getAsDict(p))

>>> hello(name='john', age=25, pc='12345')
Traceback (most recent call last):
...
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:Main SubMethod:hello On strick check: <==
Unsupported Attribute: pc !. Supported Attributes are: age, name.
>>>


2.4/ Same function with arguments instead of parameters:


def hello(name, age):
    p=wk.WKS()
    p.name={'*type':'str'}
    p.age={'*type':'int', '*value':30}
    wk.check (wks=p, kws={'name':name, 'age':age}, strict=True )

    print('Function hello! parameters as dict: %s' % wk.getAsDict(p))

>>> hello(name='john', age=25)
Function hello! parameters as dict: {'age': 25, 'name': 'john'}



3/ USING THE CP ANNOTATION

cp (wk.cp is ans alias for wk.check_parameters) is an annotation that simplifies declaring wk expressions on Arguments and Parameters.


3.1/ As Arguments:


from wk import cp
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)
def hello(name, age):
    print('Function hello! name: %s, age: %s' % (name, age))

>>> hello('john', 25)
Function hello! name: john, age: 25

- Calling with None:

>>> hello('john', None)
Function hello! name: john, age: 30


- hello with *args signature:

from wk import cp
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)
def hello(*args):
    print('Function hello! Arguments: %s' % str(args))

- This trigger an error because on strict=True:
>>> hello('john', 25, 'blabla')
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:wkDecorator/args/str SubMethod:hello
On strick check: Unsupported Attribute: arg2 !. Supported Attributes are: arg0, arg1.


3.2/ As parameters:


from wk import cp
@cp(
    kws={
        'name': {'*type':'str'},
        'age': {'*type':'int', '*value':30},
    }, strict=True)
def hello(name=None, age=None):
    print('Function hello! name: %s, age: %s' % (name, age))

>>> hello(name='john')
Function hello! name: john, age: 30

- Mixed:

from wk import cp
@cp({'*type': 'str', '*value': 'john'}, {'*type': 'int', '*value': 30},
    kws={
        'pc': {'*reg': '^\d{5}([\-]?\d{4})?$'},
        'vegan': {'*type': 'bool', '*value': True}
    }, strict=True)
def hello(*args, pc=None, age=None, vegan=False):
    print('Function hello! arguments: %s, pc: %s, vegan: %s' % (str(args), pc, vegan))

>>> hello()
Function hello! arguments: ('john', 30), pc: None, vegan: True
>>> hello('mia', pc='12345-9876')
Function hello! arguments: ('mia', 30), pc: 12345-9876, vegan: True



II) wk Expressions in Depht



1> NO CHECK AT ALL:


a) No check with None:

import wk
p=wk.WKS()
p.any=None
wk.check (wks=p, kws={'any': 'hello'}, strict=True )
>>> p.any
'hello'


b) No check with default attribute:

import wk
p=wk.WKS()
p.any='hello'
wk.check (wks=p, kws={}, strict=True )
>>> p.any
'hello'



2/ WORKING WITH WK BASE TYPES:


2.1> *type:

*type: checks the value (and default value) for the provided <type>.

Supported types are listed in: wk.SUPPORTED_TYPES:
    ('str', 'int', 'float', 'tuple', 'list', 'dict', 'bool', 'xml', 'wkDef')
Or
Any Python type: (a Python type is an obj for which this is True: isinstance(obj, type))
Or A list of Python types.

Syntax: '*type': <type>

Note:
- a) When is a Python type (or a list of ...), wk always perform isintance ans issubclass on it.
And will return an error only if both are false.
- b) <type> : can be provided as string: like in {'*type': 'int'},
or not: like in {'*type': int}
This is true only for Python standard base types: str, int, float, tuple, list, dict, bool.


Examples:


a) with Base types:

import wk
p=wk.WKS()
p.age={'*type': int}
wk.check (wks=p, kws={'age': 5})
>>> p.age
5

p=wk.WKS()
p.vegan={'*type': bool}
wk.check (wks=p, kws={'vegan': False})
>>> p.vegan
False

b) with any Pyhon types:

import wk
class A:pass

class B(A, list):pass

p=wk.WKS()
p.obj={'*type': A}
wk.check ( wks=p, kws={'obj': A()} )
>>> p.obj
<__main__.A object at 0x00000000032BAD68>

p=wk.WKS()
p.obj={'*type': (A, list)}
wk.check ( wks=p, kws={'obj': B()} )
>>> p.obj
[]


2.2/ *value:

*value: Provide a default value to a field.

Syntax: '*value': <any>

import wk
p=wk.WKS()
p.name={'*value': 'john'}
wk.check ( wks=p, kws={'name': None} )
>>> p.name
'john'


2.3/ *required:

*required raises an exception if no value is provided for the field.

Syntax: '*required': True

import wk
p=wk.WKS()
p.name={'*required': True}
wk.check ( wks=p, kws={'name': None} )

wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On *Required:Received no value
for required key:name. Your wkDefinition: {'*required': True}



3/ COMPARATORS:


3.1/ *lt, *gt, *eq, *le, *ge, *ne:

All are unary comparators.
And compare anything that is comparable by the standard Python operators (<, >, ==, <=, >=, !=).

Syntaxe: '*lt': <value>

Examples:

import wk
p=wk.WKS()
p.item={'*lt': 'bbb'}
wk.check ( wks=p, kws={'item': 'aaa'} )
>>> p.item
'aaa'


import wk
p=wk.WKS()
p.index={'*lt': 4}
wk.check ( wks=p, kws={'index': 2} )
>>> p.index
2


3.2/ *between:

*between is a binary comparator.

Syntaxe: '*between': <list/tuple> Examples:

import wk
p=wk.WKS()
p.item={'*between': ('aaa', 'ccc')}
wk.check ( wks=p, kws={'item': 'bbb'} )
>>> p.item
'bbb'

import wk
p=wk.WKS()
p.item={'*between': ('aaa', 'ccc')}
wk.check ( wks=p, kws={'item': 'zbbb'} )

wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *between:Received Bad
value for key:item Value Expected: between ('aaa', 'ccc'). Value Received:zbbb. Your wkDefinition: {'*between': ('aaa', 'ccc')}

import wk
p=wk.WKS()
p.item={'*between': (1, 3)}
wk.check ( wks=p, kws={'item': 2} )
>>> p.item
2



4/ DEEP INTO COMPLEXE TYPES DICT, LIST and TUPLE:


4.1/ *ltype:

*ltype is an advanced form of {'*type': list} or {'*type': tuple}.
*ltype is only supported for {'*type': list} or {'*type': tuple}.
*ltype will describe the type of the list items by a new wk definition.

Syntax: '*type': list, '*ltype': <wk expression>

Examples:

import wk
p=wk.WKS()
p.cars={'*type': list, '*ltype': {'*type': int}}
wk.check( wks=p, kws={'cars': [1,2,3,4,5]} )
>>> p.cars
[1, 2, 3, 4, 5]


List with Colltyping e.g.:

p=wk.WKS()
p.cars={'*type': list, '*ltype': {'*type': int}, '*withCoolTyping': True}
wk.check( wks=p, kws={'cars': '[1,2,3,4,5]'} )
>>> p.cars
[1, 2, 3, 4, 5]


4.2/ *dtype:

*dtype is an advanced form of {'*type': dict}.
*dtype is only supported for {'*type': dict}.
*dtype will describe each key of the expected dictionary with a new wk definition.

Syntax: '*type': dict, '*dtype': {'key1': <wk expression>, ['keyn': <wk expression>]}

Examples:

import wk
p=wk.WKS()
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*required': True}
wk.check( wks=p, kws={'person': {'name': 'john', 'age': 30, 'vegan' : False} })
>>> p.person
{'age': 30, 'vegan': False, 'name': 'john'}


Dict with Colltyping e.g.:

p=wk.WKS()
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*withCoolTyping': True}
wk.check( wks=p, kws={'person': '{name:john,age:30,vegan:False}' })
>>> p.person
{'age': 30, 'vegan': False, 'name': 'john'}

List of dict:

p=wk.WKS()
p.lperson={'*type': list, '*ltype':{
    '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}
}}
wk.check( wks=p, kws={'lperson': [
    {'name': 'john', 'age': 30, 'vegan' : False},
    {'name': 'jim', 'age': 40, 'vegan' : True},
    {'name': 'lee', 'age': 50, 'vegan' : False}
]})
>>> p.lperson
[{'age': 30, 'name': 'john', 'vegan': False}, {'age': 40, 'name': 'jim', 'vegan': True}, {'age': 50, 'name': 'lee', 'vegan': False}]
>>>



4.3/ *checkIn:

checkIn checks a value in a list (or tuple).

Syntax: '*checkIn': <tuple or list<

p=wk.WKS()
p.color={'*type': str, '*checkIn': ('green', 'violet', 'red')}
wk.check( wks=p, kws={'color': 'violet'} )
>>> p.color
'violet'


p=wk.WKS()
p.any={'*type': int, '*checkIn': (False, 5, ('hello',))}
wk.check( wks=p, kws={'any': 5} )
>>> p.any
5



4.4/ *checkXIn:

*checkXIn checks the presence of all elements of a list (or tuple) into another list (or tuple).
value must be a list ('*type': list or '*type': tuple).

Syntax: '*checkXIn': <tuple or list>

Examples:

p=wk.WKS()
p.colors={'*type': tuple, '*checkXIn': ('green', 'violet', 'red')}
wk.check( wks=p, kws={'colors': ('violet', 'green')} )
>>> p.colors
('violet', 'green')



5/ CONVERTIONS:


Sometimes the input value for the field is not formated like you want it.
And you migth want to apply a specific convertion to this value before to perform any control on it.
The following wk keys stands for this purpose.
Convertions are always performed first and Controls afterward.
Convertions are applyied on any value or default value (*value) for a field.


5.1/ *force_str:

*force_str: force the convertion of an obj to a string.

Syntax: 'force_str': True
Note: if value is a bytes, wk will try: value.decode('utf-8') on it.

Examples:

a) An int to a string:

import wk
p=wk.WKS()
p.name={'*force_str': True, '*type': str} # While simple this fails: {'*type': str}
wk.check ( wks=p, kws={'name': 123 } )
>>> p.name
'123'

b) A byte to a string:
import wk
p=wk.WKS()
p.name={'*force_str': True, '*type': str}
wk.check ( wks=p, kws={'name': 'abc'.encode('utf-8') } )
>>> p.name
'abc'



5.2/ *withEval:


Restricted evals:
*withEval is supported by a python eval handled par the _eval function of the ct.py mofdule.
The _eval function use a restricted eval is the sens that it uses no locals, globals no extra function or modules.
But a restricted "__builtins__" list of base types:
    ('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').
So any of these base type can be evaluated.

The field's value in input, must always be a String.

Syntax: '*withEval': True

Examples:

import wk
p=wk.WKS()
p.age={'*type': int, '*withEval': True}
wk.check (wks=p, kws={'age': '5'})
>>> p.age
5


p=wk.WKS()
p.age={'*type': float, '*withEval': True}
wk.check (wks=p, kws={'age': '5.0'})
>>> p.age
5.0


p=wk.WKS()
p.cars={'*type': tuple, '*withEval': True}
wk.check (wks=p, kws={'cars': '(1, 2, 3)'})
>>> p.cars
(1, 2, 3)


p=wk.WKS()
p.person={'*type': dict, '*withEval': True}
wk.check (wks=p, kws={
    'person': "{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}"
})
>>> p.person
{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}


And so on !


5.3/ *withCoolTyping:

*withCoolTyping is a more advanced version of *withEval.
*withCoolTyping stands for convertion of string values with no: ' and no: " to any of the previous base types:
    ('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').
*withCoolTyping stands to be used in environment that mess with: ' and " like terminal, console, xml for example.

The field's value in input, must always be a String.

Syntax: '*withCoolTyping': True

Examples:

import wk
p=wk.WKS()
p.person={'*type': dict, '*withCoolTyping': True}
wk.check (wks=p, kws={
    'person': "{name:john,age:30,adress:21 jump street,vegan:True}" # Note that all not significative spaces must be wiped off !
})
>>> p.person
'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}

p=wk.WKS()
p.story={'*type': tuple, '*withCoolTyping': True}
wk.check (wks=p, kws={
    'story': "(once,upon a time,in the east)"
})
>>> p.story
('once', 'upon a time', 'in the east')

And so on !


5.4/ *withConvert:

*withConvert is to be used when none of the previous convertion method works.
*withConvert takes a user function in its description, and calls this function on any value or default value (*value) for the field.
The field's value in input, may be of any type.

Syntax: '*withConvert': <Python function(value)>

Examples:

p=wk.WKS()
p.age={'*type': int, '*withConvert': lambda e: int(e)}
wk.check (wks=p, kws={
    'age': "123"
})
>>> p.age
123

p=wk.WKS()
p.person={'*type': int, '*withConvert': lambda e: e == 'john'}
wk.check (wks=p, kws={
    'person': "john"
})
>>> p.person
True



6/ REGULAR EXPRESSION:

6.1/ *reg:
*reg will check any value (or the default value) on this regular expression.

Syntax: '*reg': '<regular_expression>'

Examples:

p=wk.WKS()
p.pc={'*reg': '^\d{5}([\-]?\d{4})?$'}
wk.check (wks=p, kws={'pc': "12345-9876"})
>>> p.pc
"12345-9876"

p=wk.WKS()
p.pc={'*reg': '^wel.+'}
wk.check (wks=p, kws={'pc': "wel"})

wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *reg:Received Bad value fo
r key:pc Value Expected: reg:dct['*reg']. Value Received:wel, type:. Your wkDefinition: {'*reg': '^wel.+'}

p=wk.WKS()
p.pc={'*reg': '^wel.+'}
wk.check (wks=p, kws={'pc': "welcome"})
>>> p.pc
'welcome'



7/ DATE AND TIMESTAMP:


7.1/ *date:

*date expects a Standard Python strptime/strftime string format (actually based on the C standard (1989 version)).
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at
    https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior

Syntax:
Either you use '*date': and your date entry should match the exact pattern.
or {'*date': None} and your date entry may be:
    - a String supported date literal
    - or an instance of datetime.date.

Examples :
a) With a strptime string:

import wk
p=wk.WKS()
p.birthday = {'*date': '%d/%m/%Y'}
wk.check( wks=p, kws={'birthday': '01/11/2016'})
>>> p.birthday
datetime.datetime(2016, 11, 1)

- In fact wk do this in background:
>>> datetime.date(*time.strptime("01/11/2016", "%d/%m/%Y")[0:3])
datetime.date(2016, 11, 1)

Or:

p = wk.WKS()
p.birthday = {'*date': '%d %b %Y'}
wk.check(p, {'birthday': '29 Aug 2017'})
>>> p.birthday
datetime.date(2017, 8, 29)


b) With None

Value is a datetime:
import datetime
p = wk.WKS()
p.birthday = {'*date': None}
wk.check(p, {'birthday': datetime.date(2016, 11, 1)})
>>> p.birthday
datetime.date(2016, 11, 1)

Value a Supported date literal:
p = wk.WKS()
p.birthday = {'*date': None}
wk.check(p, {'birthday': 'Tue Jun 16 20:18:03 1981'})
>>> p.birthday
datetime.date(1981, 6, 16)

DATE With Colltyping e.g.:

import wk
p=wk.WKS()
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthdate': {'*date': '%d/%m/%Y'}}, '*withCoolTyping': True}
wk.check( wks=p, kws={'person': '{name:john,age:30,birthdate:01/11/2016}' }, strict=True )
>>> p.person
{'age': 30, 'birthdate': datetime.datetime(2016, 11, 1), 'name': 'john'}


7.2/ *ts:

*ts expects a Standard Python strptime/strftime string format (actually based on the C standard (1989 version)).
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at Python doc.

Syntax:
Either you use '*ts': and your date entry should match the exact pattern.
or {'*ts': None} and your date entry may be:
    - a String supported date literal
    - or an instance of datetime.date.

Examples :

a) With a strptime string:

import wk
p=wk.WKS()
p.birthdate = {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}
wk.check( wks=p, kws={'birthdate': '01/11/2016 16h40mn3s'})
>>> p.birthdate
datetime.datetime(2016, 11, 1, 16, 40, 3)

- Actually fact wk do this in background:
>>> datetime.datetime(*time.strptime("01/11/2016 16h40mn3s", "%d/%m/%Y %Hh%Mmn%Ss")[0:6])
datetime.datetime(2016, 11, 1, 16, 40, 3))

Or:

p = wk.WKS()
p.birthdate = {'*ts': '%d %b %Y:%H:%M:%S'}
wk.check(p, {'birthdate': '29 Aug 2017:02:00:46'})
>>> p.birthdate
datetime.datetime(2017, 8, 29, 2, 0, 46)


b) With None

Value is a datetime:

import datetime
p = wk.WKS()
p.birthdate = {'*ts': None}
wk.check(p, {'birthdate': datetime.datetime(2016, 11, 1, 16, 40, 3)})
>>> p.birthdate
datetime.datetime(2016, 11, 1, 16, 40, 3)

Value a Supported datetime literal:

p = wk.WKS()
p.birthdate = {'*ts': None}
wk.check(p, {'birthdate': 'Tue Jun 16 20:18:03 1981'})
>>> p.birthdate
datetime.datetime(1981, 6, 16, 20, 18, 3)

TS With Colltyping e.g.:

import wk
p=wk.WKS()
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthday': {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}}, '*withCoolTyping': True}
wk.check( wks=p, kws={'person': '{name:john,age:30,birthday:01/11/2016 16h40mn3s}' }, strict=True )
>>> p.person
{'age': 30, 'birthday': datetime.datetime(2016, 11, 1, 16, 40, 3), 'name': 'john'}



8/ MISCELLANEOUS:


8.1> *len:

Syntax: '*len': <int>

Examples :

import wk
p=wk.WKS()
p.size={'*len': 3}
wk.check( wks=p, kws={'size': 'abc'} ) # Will check len('abc') == 3
>>> p.size
'abc'


8.2> *minLen, *maxLen:

Syntax: '*minLen': <int>

Examples :

import wk
p=wk.WKS()
p.size={'*minLen': 3}
wk.check( wks=p, kws={'size': 'abcefg'} ) # Will check len('abc') >= 3
>>> p.size
'abcefg'


8.3> *startsWith, *endsWith:

Syntax: '*startsWith': <value>

Examples :

import wk
p=wk.WKS()
p.story={'*startsWith': 'once upon'}
wk.check( wks=p, kws={'story': 'once upon a time'} )
>>> p.story
'once upon a time'



9/ WKDEF ITSELF:

wkDef: check that value is a wk expression.

Syntax: '*type': '*wkDef'

Examples :

a) Example with the previous one for *startsWith.
import wk
p=wk.WKS()
p.wkexp={'*type': 'wkDef'}
wk.check( wks=p, kws={'wkexp':
    {'*startsWith': 'once upon'}
}, strict=True )
>>> p.wkexp
{'*startsWith': 'once upon'}


b) Exemple with this wk expression from the *dict example:
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}}


import wk
p=wk.WKS()
p.wkexp={'*type': 'wkDef', '*withCoolTyping': True}
wk.check( wks=p, kws={'wkexp':
    {'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}} # The CoolTyped expression.
}, strict=True )
p.wkexp
{'*type': <class 'dict'>, '*dtype': {'name': {'*type': <class 'str'>}, 'age': {'*type': <class 'int'>}, 'vegan': {'*type': <class 'bool'>}}}



10> INFO KEYS:

Here are 3 harmless key with no associated function at all, free to be used for any purpose.
Syntax: *label: <anything>
*help: <anything>
*lhelp: <anything>

