<HTML>

<HEAD>
</HEAD>

<BODY style='font-family:courier'>

</br>
</br>
<div style='with=100%;text-align:center;background:cyan;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
I) WantedKeyword (wk) Python Type Checking tool (Python3)<br />

Full doc at: www.kikonf.org/wk

</div>

<br />
<br />
<br />
<div style='with=100%;text-align:center;background: #ebdef0 ;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
1/ INTRODUCTION
</div>

<br />
<br />

<u>What is it ?<br /></u>
<br />
wk allows to check a collection of fields, against their characteristics, defined into wk expressions.<br />
<br />
A wk expression (a.k.a. wk definition) is a dictionary, where each key defines a specific control to be operated on the field.<br />
<br />
e.g.1:<br />
age = {'*type':int}<br />
<br />
This expression Will check the field named: age using the control: *type defined into the wk definition: {'*type':int}.<br />
<br />
{'*type':int}: means that age must be an integer otherwise an exception is raised.<br />
<br />
e.g.2:<br />
vegan = {'*type': bool}<br />
Means that the field vegan must be a bool.<br />
<br />
They are cumulative ex:<br />
age = {'*type':int, '*ge':26, '*ne': 35, '*value': 40}<br />
As we'll see later.<br />
<br />
<br />
<u>Running checks:</u><br />
<br />
Checks can be performed in two ways.<br />
<br />
a) Using the check method of the wk module:<br />
<br />
Test this into the Python interpreter:<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
<br />
name = 'john'<br />
age=45<br />
p=wk.WKS()<br />
p.name={'*type':'str'}<br />
p.age={'*type':'int', '*value':30}<br />
wk.check (wks=p, kws={'name':name, 'age':age})<br />
>>> p.name<br />
'john'<br />
>>> p.age<br />
45<br />
</code></div>
<br />
Try this:<br />
p=wk.WKS()<br />
p.age={'*type':'int'}<br />
wk.check (wks=p, kws={'age': 'hello'})<br />
<br />
To see what happens !<br />
<br />
This can be comfortably embedded into a function or method:<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(name, age):<br />
&nbsp;&nbsp;&nbsp;&nbsp;import wk<br />
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}<br />
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws={'name':name, 'age':age})<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (p.name, p.age))<br />
<br />
>>> hello('john', None)<br />
Function hello! name: john, age: 30<br />
</code></div>
<br />
<br />
b) Using the cp annotation of the wk module:<br />
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp<br />
@cp({'*type': 'str'}, {'*type':'int', '*value':30})<br />
def hello(name, age):<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))<br />
<br />
>>> hello('john')<br />
Function hello! name: john, age: 30<br />
</code></div>
<br />

<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
2/ USING THE CHECK FUNCTION
</div>
<br />
<br />
2.1/ A simple function hello<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
def hello(*args, **keywords):<br />
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}<br />
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=keywords )<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;print ('function: hello !')<br />
&nbsp;&nbsp;&nbsp;&nbsp;print ('-----------------')<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('name: %s' % p.name)<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('age: %s' % p.age)<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('parameters as dict: %s' % wk.getAsDict(p))<br />
<br />
>>> hello(name='john', age=25)<br />
<br />
function: hello !<br />
-----------------<br />
name: john<br />
age: 25<br />
parameters as dict: {'age': 25, 'name': 'john'}<br />
>>>
</code></div>
<br />
- With default value: *value:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello(name='john')<br />
function: hello !<br />
-----------------<br />
name: john<br />
age: 30 # <-- Default value for age<br />
parameters as dict: {'age': 30, 'name': 'john'}<br />
</code></div><br />
<br />
<div style="background: #FFFFC2;border: 1px solid black; border-radius: 10px; padding:10px;">
<u>Lines explained:<br /></u>
Line 3: p=wk.WKS(): An WKS instance (wk.WKS is alias for wk.WantedKeyword) is created with no Parameter.<br />
Line 4-5: p.name={'*type':'str'}: p.<attribute-name = <wk expression> : One declares wk expressions on the fly on the wk instance.<br />
Line 6:wk.check (wks=p, kws=kws ): The wk module's function: check, runs on each Attribute of the wk.WKS instances: "name" and "age".<br />
Regarding the wk expression defined for each one.<br />
The values are retreives from the kws parameter.<br />
The final values are stored on the attributes of the WKS instance (p).<br />
<br />
a) wk.check retreives the user value for the attribute name, from kws['name'].<br />
b) wk.check verifies this value on the wk expresion: {'*type':'str'}, defined for attribute name.<br />
c) wk.check applies the final value to p.name<br />
</div>
<br />
<br />
- More on extra Parameters:<br />
<br />
If you run the following call, you'll see that no error is generated althought parameter pc is not defined.<br />
>>> hello(name='john', age=25, pc='12345')<br />
To reject extra parameter you must use the keyword: strict.<br />
<br />
<br />
2.2/ Same function but using strict<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(*args, **kws):<br />
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}<br />
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=kws, strict=True )<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))<br />
</code></div>
<br />
- Using the extra parameter: pc causes an error, because of strict=True:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello(name='john', age=25, pc='12345')<br />
Traceback (most recent call last):<br />
...<br />
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On strick check:<br />
Unsupported Attribute: pc !. Supported Attributes are: age, name. <==<br />
</code></div><br />
<br />
<br />
As you can see into this excerpts from the previous exception: "SubClass:None SubMethod:None",<br />
The calling class and method is not traced.<br />
To trace them one should use class_exit and method_exit as follows.<br />
<br />
<br />
2.3/ Same function but using class_exit and method_exit<br />
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(*args, **kws):<br />
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}<br />
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=kws, strict=True, class_exit='Main', method_exit='hello')<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))<br />
<br />
>>> hello(name='john', age=25, pc='12345')<br />
Traceback (most recent call last):<br />
...<br />
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:Main SubMethod:hello On strick check: <==<br />
Unsupported Attribute: pc !. Supported Attributes are: age, name.<br />
>>>
</code></div>
<br />
<br />
2.4/ Same function with arguments instead of parameters:<br />
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(name, age):<br />
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}<br />
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}<br />
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws={'name':name, 'age':age}, strict=True )<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))<br />
<br />
>>> hello(name='john', age=25)<br />
Function hello! parameters as dict: {'age': 25, 'name': 'john'}<br />
</code></div>
<br />
<br />

<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
3/ USING THE CP ANNOTATION
</div>
<br />
cp (wk.cp is ans alias for wk.check_parameters) is an annotation that simplifies declaring wk expressions on Arguments and Parameters.<br />
<br />
<br />
<u>3.1/ As Arguments:</u><br />
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp<br />
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)<br />
def hello(name, age):<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))<br />
<br />
>>> hello('john', 25)<br />
Function hello! name: john, age: 25<br />
</code></div>
<br />
- Calling with None:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello('john', None)<br />
Function hello! name: john, age: 30<br />
</code></div>
<br />
<br />
- hello with *args signature:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp<br />
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)<br />
def hello(*args):<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! Arguments: %s' % str(args))<br />
<br />
- This trigger an error because on strict=True:<br />
>>> hello('john', 25, 'blabla')<br />
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:wkDecorator/args/str SubMethod:hello<br />
On strick check: Unsupported Attribute: arg2 !. Supported Attributes are: arg0, arg1.<br />
</code></div>
<br />
<br />
<u>3.2/ As parameters:</u><br />
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp<br />
@cp(<br />
&nbsp;&nbsp;&nbsp;&nbsp;kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'name': {'*type':'str'},<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'age': {'*type':'int', '*value':30},<br />
&nbsp;&nbsp;&nbsp;&nbsp;}, strict=True)<br />
def hello(name=None, age=None):<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))<br />
<br />
>>> hello(name='john')<br />
Function hello! name: john, age: 30<br />
</code></div>
<br />
- Mixed:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp<br />
@cp({'*type': 'str', '*value': 'john'}, {'*type': 'int', '*value': 30},<br />
&nbsp;&nbsp;&nbsp;&nbsp;kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'pc': {'*reg': '^\d{5}([\-]?\d{4})?$'},<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'vegan': {'*type': 'bool', '*value': True}<br />
&nbsp;&nbsp;&nbsp;&nbsp;}, strict=True)<br />
def hello(*args, pc=None, age=None, vegan=False):<br />
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! arguments: %s, pc: %s, vegan: %s' % (str(args), pc, vegan))<br />
<br />
>>> hello()<br />
Function hello! arguments: ('john', 30), pc: None, vegan: True<br />
>>> hello('mia', pc='12345-9876')<br />
Function hello! arguments: ('mia', 30), pc: 12345-9876, vegan: True<br />
</code></div>
<br />
<br />
<br />
<div style='with=100%;text-align:center;background:cyan;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
II) wk Expressions in Depht
</div>

<br />
<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
1> NO CHECK AT ALL:
</div>
<br />
<br />
a) No check with None:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.any=None<br />
wk.check (wks=p, kws={'any': 'hello'}, strict=True )<br />
>>> p.any<br />
'hello'<br />
</code></div>
<br />

<br />
b) No check with default attribute:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.any='hello'<br />
wk.check (wks=p, kws={}, strict=True )<br />
>>> p.any<br />
'hello'<br />
</code></div>
<br />

<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
2/ WORKING WITH WK BASE TYPES:
</div>
<br />
<br />
2.1> *type:<br />
<br />
*type: checks the value (and default value) for the provided &lt;type&gt;.<br />
<br />
Supported types are listed in: wk.SUPPORTED_TYPES:<br />
&nbsp;&nbsp;&nbsp;&nbsp;('str', 'int', 'float', 'tuple', 'list', 'dict', 'bool', 'xml', 'wkDef')<br />
Or<br />
Any Python type: (a Python type is an obj for which this is True: isinstance(obj, type))<br />
Or  A list of Python types.<br />
<br />
<u>Syntax:</u> <b>'*type': &lt;type&gt;</b>
<br />
<br />

<u>Note:</u>
<br />
- a) When <type> is a Python type (or a list of ...),  wk always perform isintance ans issubclass on it.<br />
And will return an error only if both are false.<br />
- b) &lt;type&gt; : can be provided as string: like in {'*type': 'int'},<br />
or not: like in {'*type': int}<br />
This is true only for Python standard base types: str, int, float, tuple, list, dict, bool.<br />
<br />
<br />
<u>Examples:</u>
<br />
<br />
<br />
a) with Base types:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.age={'*type': int}<br />
wk.check (wks=p, kws={'age': 5})<br />
>>> p.age<br />
5<br />
</code></div>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.vegan={'*type': bool}<br />
wk.check (wks=p, kws={'vegan': False})<br />
>>> p.vegan<br />
False<br />
</code></div>
<br />
b) with any Pyhon types:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
class A:pass<br />
<br />
class B(A, list):pass<br />
<br />
p=wk.WKS()<br />
p.obj={'*type': A}<br />
wk.check ( wks=p, kws={'obj': A()} )<br />
>>> p.obj<br />
<__main__.A object at 0x00000000032BAD68><br />
</code></div>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.obj={'*type': (A, list)}<br />
wk.check ( wks=p, kws={'obj': B()} )<br />
>>> p.obj<br />
[]<br />
</code></div>
<br />
<br />
2.2/ *value:<br />
<br />
*value: Provide a default value to a field.<br />
<br />
<u>Syntax:</u> <b>'*value': &lt;any&gt;<br /></b>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.name={'*value': 'john'}<br />
wk.check ( wks=p, kws={'name': None} )<br />
>>> p.name<br />
'john'<br />
</code></div>
<br />
<br />
2.3/ *required:<br />
<br />
*required raises an exception if no value is provided for the field.<br />
<br />
<u>Syntax:</u> <b>'*required': True</b>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.name={'*required': True}<br />
wk.check ( wks=p, kws={'name': None} )<br />
<br />
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On *Required:Received no value<br />
 for required key:name. Your wkDefinition: {'*required': True}<br />
</code></div>
<br />
<br />

<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
3/ COMPARATORS:
</div>
<br />
<br />
3.1/ *lt, *gt, *eq, *le, *ge, *ne:<br />
<br />
All are unary comparators.<br />
And compare anything that is comparable by the standard Python operators (<, >, ==, <=, >=, !=).<br />
<br />
<u>Syntaxe:</u> <b>'*lt': &lt;value&gt;</b>
<br />
<br />
<u>Examples:</u>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.item={'*lt': 'bbb'}<br />
wk.check ( wks=p, kws={'item': 'aaa'} )<br />
>>> p.item<br />
'aaa'<br />
</code></div>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.index={'*lt': 4}<br />
wk.check ( wks=p, kws={'index': 2} )<br />
>>> p.index<br />
2<br />
</code></div>
<br />
<br />

3.2/ *between:<br />
<br />
*between is a binary comparator.<br />
<br />
<u>Syntaxe:</u> <b>'*between': &lt;list/tuple&gt;</b>


<u>Examples:</u>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.item={'*between': ('aaa', 'ccc')}<br />
wk.check ( wks=p, kws={'item': 'bbb'} )<br />
>>> p.item<br />
'bbb'<br />
</code></div>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.item={'*between': ('aaa', 'ccc')}<br />
wk.check ( wks=p, kws={'item': 'zbbb'} )<br />
<br />
wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *between:Received Bad<br />
value for key:item  Value Expected: between ('aaa', 'ccc').  Value Received:zbbb. Your wkDefinition: {'*between': ('aaa', 'ccc')}<br />
</code></div>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.item={'*between': (1, 3)}<br />
wk.check ( wks=p, kws={'item': 2} )<br />
>>> p.item<br />
2<br />
</code></div>
<br />
<br />

<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
4/ DEEP INTO COMPLEXE TYPES DICT, LIST and TUPLE:
</div>
<br />
<br />

4.1/ *ltype:<br />
<br />
*ltype is an advanced form of {'*type': list} or {'*type': tuple}.<br />
*ltype is only supported for {'*type': list} or {'*type': tuple}.<br />
*ltype will describe the type of the list items by a new wk definition.<br />
<br />
<u>Syntax:</u> <b>'*type': list, '*ltype': &lt;wk expression&gt;</b>
<br />
<br />
<u>Examples:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.cars={'*type': list, '*ltype': {'*type': int}}<br />
wk.check( wks=p, kws={'cars': [1,2,3,4,5]} )<br />
>>> p.cars<br />
[1, 2, 3, 4, 5]<br />
</code></div>
<br />
<br />
<u>List with Colltyping e.g.:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.cars={'*type': list, '*ltype': {'*type': int}, '*withCoolTyping': True}<br />
wk.check( wks=p, kws={'cars': '[1,2,3,4,5]'} )<br />
>>> p.cars<br />
[1, 2, 3, 4, 5]<br />
</code></div>
<br />
<br />

4.2/ *dtype:<br />
<br />
*dtype is an advanced form of {'*type': dict}.<br />
*dtype is only supported for {'*type': dict}.<br />
*dtype will describe each key of the expected dictionary with a new wk definition.<br />
<br />
<u>Syntax:</u> <b>'*type': dict, '*dtype': {'key1': &lt;wk expression&gt;, ['keyn': &lt;wk expression&gt;]}</b>
<br />
<br />
<u>Examples:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*required': True}<br />
wk.check( wks=p, kws={'person': {'name': 'john', 'age': 30, 'vegan' : False} })<br />
>>> p.person<br />
{'age': 30, 'vegan': False, 'name': 'john'}<br />
</code></div>
<br />
<br />
<u>Dict with Colltyping e.g.:</u>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*withCoolTyping': True}<br />
wk.check( wks=p, kws={'person': '{name:john,age:30,vegan:False}' })<br />
>>> p.person<br />
{'age': 30, 'vegan': False, 'name': 'john'}<br />
</code></div>
<br />
<u>List of dict:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.lperson={'*type': list, '*ltype':{<br />
&nbsp;&nbsp;&nbsp;&nbsp;'*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}<br />
}}<br />
wk.check( wks=p, kws={'lperson': [<br />
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'john', 'age': 30, 'vegan' : False},<br />
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'jim', 'age': 40, 'vegan' : True},<br />
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'lee', 'age': 50, 'vegan' : False}<br />
]})<br />
>>> p.lperson<br />
[{'age': 30, 'name': 'john', 'vegan': False}, {'age': 40, 'name': 'jim', 'vegan': True}, {'age': 50, 'name': 'lee', 'vegan': False}]<br />
>>><br />
</code></div>
<br />
<br />
<br />
4.3/ *checkIn:<br />
<br />
checkIn checks a value in a list (or tuple).<br />
<br />
<u>Syntax:</u> <b>'*checkIn': &lt;tuple or list&lt;</b>
<br />

<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.color={'*type': str, '*checkIn': ('green', 'violet', 'red')}<br />
wk.check( wks=p, kws={'color': 'violet'} )<br />
>>> p.color<br />
'violet'<br />
</code></div>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.any={'*type': int, '*checkIn': (False, 5, ('hello',))}<br />
wk.check( wks=p, kws={'any': 5} )<br />
>>> p.any<br />
5<br />
</code></div>
<br />


<br />
<br />
4.4/ *checkXIn:<br />
<br />
*checkXIn checks the presence of all elements of a list (or tuple) into another list (or tuple).<br />
value must be a list ('*type': list or '*type': tuple).<br />
<br />
<u>Syntax:</u> <b>'*checkXIn': &lt;tuple or list&gt;</b>
<br />
<br />


<u>Examples:</u>
<br />

<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.colors={'*type': tuple, '*checkXIn': ('green', 'violet', 'red')}<br />
wk.check( wks=p, kws={'colors': ('violet', 'green')} )<br />
>>> p.colors<br />
('violet', 'green')<br />
</code></div>
<br />
<br />

<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
5/ CONVERTIONS:
</div>
<br />
<br />

Sometimes the input value for the field is not formated like you want it.<br />
And you migth want to apply a specific convertion to this value before to perform any control on it.<br />
The following wk keys stands for this purpose.<br />
Convertions are always performed first and Controls afterward.<br />
Convertions are applyied on any value or default value (*value) for a field.<br />
<br />

<br />
5.1/ *force_str:<br />
<br />
*force_str: force the convertion of an obj to a string.<br />
<br />
<u>Syntax:</u> <b>'force_str': True</b>
<br />
Note: if value is a bytes, wk will try: value.decode('utf-8') on it.<br />
<br />
<u>Examples:</u>
<br />
<br />
a) An int to a string:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.name={'*force_str': True, '*type': str} # While simple this fails: {'*type': str}<br />
wk.check ( wks=p, kws={'name': 123 } )<br />
>>> p.name<br />
'123'<br />
</code></div>
<br />
b) A byte to a string:<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.name={'*force_str': True, '*type': str}<br />
wk.check ( wks=p, kws={'name': 'abc'.encode('utf-8') } )<br />
>>> p.name<br />
'abc'<br />
</code></div>
<br />

<br />
<br />
5.2/ *withEval:<br />
<br />
<br />
<b>Restricted evals:</b><br />
*withEval is supported by a python eval handled par the _eval function of the ct.py mofdule.<br />
The _eval function use a restricted eval is the sens that it uses no locals, globals no extra function or modules.<br />
But a restricted "__builtins__" list of base types:<br />
&nbsp;&nbsp;&nbsp;&nbsp;('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').<br />
So any of these base type can be evaluated.<br />
<br />
The field's value in input, must always be a String.<br />
<br />
<u>Syntax:</u> <b>'*withEval': True</b>
<br />
<br />

<u>Examples:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.age={'*type': int, '*withEval': True}<br />
wk.check (wks=p, kws={'age': '5'})<br />
>>> p.age<br />
5<br />
</code></div>
<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.age={'*type': float, '*withEval': True}<br />
wk.check (wks=p, kws={'age': '5.0'})<br />
>>> p.age<br />
5.0<br />
</code></div>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.cars={'*type': tuple, '*withEval': True}<br />
wk.check (wks=p, kws={'cars': '(1, 2, 3)'})<br />
>>> p.cars<br />
(1, 2, 3)<br />
</code></div>

<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.person={'*type': dict, '*withEval': True}<br />
wk.check (wks=p, kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;'person': "{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}"<br />
})<br />
>>> p.person<br />
{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}<br />
</code></div>
<br />
<br />
And so on !<br />
<br />
<br />

5.3/ *withCoolTyping:<br />
<br />
*withCoolTyping is a more advanced version of *withEval.<br />
*withCoolTyping stands for convertion of string values with no: ' and no: " to any of the previous base types:<br />
&nbsp;&nbsp;&nbsp;&nbsp;('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').<br />
*withCoolTyping stands to be used in environment that mess with: ' and " like terminal, console, xml for example.<br />
<br />
The field's value in input, must always be a String.<br />
<br />
<u>Syntax:</u> <b>'*withCoolTyping': True</b>
<br />
<br />
<u>Examples:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.person={'*type': dict, '*withCoolTyping': True}<br />
wk.check (wks=p, kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;'person': "{name:john,age:30,adress:21 jump street,vegan:True}" # Note that all not significative spaces must be wiped off !<br />
})<br />
>>> p.person<br />
'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}<br />
<br />
p=wk.WKS()<br />
p.story={'*type': tuple, '*withCoolTyping': True}<br />
wk.check (wks=p, kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;'story': "(once,upon a time,in the east)"<br />
})<br />
>>> p.story<br />
('once', 'upon a time', 'in the east')<br />
</code></div>
<br />
And so on !<br />
<br />
<br />
5.4/ *withConvert:<br />
<br />
*withConvert is to be used when none of the previous convertion method works.<br />
*withConvert takes a user function in its description, and calls this function on any value or default value (*value) for the field.<br />
The field's value in input, may be of any type.<br />

<br />
<u>Syntax:</u> <b>'*withConvert': &lt;Python function(value)&gt;</b>
<br />

<br />
<u>Examples:</u>
<br />
<br />


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.age={'*type': int, '*withConvert': lambda e: int(e)}<br />
wk.check (wks=p, kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;'age': "123"<br />
})<br />
>>> p.age<br />
123<br />
</code></div>
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.person={'*type': int, '*withConvert': lambda e: e == 'john'}<br />
wk.check (wks=p, kws={<br />
&nbsp;&nbsp;&nbsp;&nbsp;'person': "john"<br />
})<br />
>>> p.person<br />
True<br />
</code></div>
<br />

<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
6/ REGULAR EXPRESSION:
</div>
<br />
6.1/ *reg:<br />
*reg will check any value (or the default value) on this regular expression.<br />
<br />
<u>Syntax:</u> <b>'*reg': '&lt;regular_expression&gt;'</b>
<br />
<br />
<u>Examples:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.pc={'*reg': '^\d{5}([\-]?\d{4})?$'}<br />
wk.check (wks=p, kws={'pc': "12345-9876"})<br />
>>> p.pc<br />
"12345-9876"<br />
</code></div>
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()<br />
p.pc={'*reg': '^wel.+'}<br />
wk.check (wks=p, kws={'pc': "wel"})<br />
<br />
wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *reg:Received Bad  value fo<br />
r key:pc  Value Expected: reg:dct['*reg'].  Value Received:wel, type:<class 'str'>. Your wkDefinition: {'*reg': '^wel.+'}<br />
<br />
p=wk.WKS()<br />
p.pc={'*reg': '^wel.+'}<br />
wk.check (wks=p, kws={'pc': "welcome"})<br />
>>> p.pc<br />
'welcome'<br />
</code></div>
<br />
<br />

<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
7/ DATE AND TIMESTAMP:
</div>
<br />

<br />
7.1/ *date:<br />
<br />
*date expects a Standard Python strptime/strftime string format  (actually based on the C standard (1989 version)).<br />
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at<br />
&nbsp;&nbsp;&nbsp;&nbsp;https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior<br />
<br />
<u>Syntax:</u>
<br />
Either you use '*date': <strptime string> and your date entry should match the exact pattern.<br />
or {'*date': None} and your date entry may be:<br />
&nbsp;&nbsp;&nbsp;&nbsp;- a String supported date literal<br />
&nbsp;&nbsp;&nbsp;&nbsp;- or an instance of datetime.date.<br />
<br />

<u>Examples :</u>
<br />
a) With a strptime string:<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.birthday = {'*date': '%d/%m/%Y'}<br />
wk.check( wks=p, kws={'birthday': '01/11/2016'})<br />
>>> p.birthday<br />
datetime.datetime(2016, 11, 1)<br />
<br />
- In fact wk do this in background:<br />
>>> datetime.date(*time.strptime("01/11/2016", "%d/%m/%Y")[0:3])<br />
datetime.date(2016, 11, 1)<br />
</code></div>
<br />
Or:<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()<br />
p.birthday = {'*date': '%d %b %Y'}<br />
wk.check(p, {'birthday': '29 Aug 2017'})<br />
>>> p.birthday<br />
datetime.date(2017, 8, 29)<br />
</code></div>
<br />
<br />
b) With None<br />
<br />
Value is a datetime:<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import datetime<br />
p = wk.WKS()<br />
p.birthday = {'*date': None}<br />
wk.check(p, {'birthday': datetime.date(2016, 11, 1)})<br />
>>> p.birthday<br />
datetime.date(2016, 11, 1)<br />
</code></div>
<br />
Value a Supported date literal:<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()<br />
p.birthday  = {'*date': None}<br />
wk.check(p, {'birthday': 'Tue Jun 16 20:18:03 1981'})<br />
>>> p.birthday<br />
datetime.date(1981, 6, 16)<br />
</code></div>
<br />

<u>DATE With Colltyping e.g.:</u>
<br />


<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthdate': {'*date': '%d/%m/%Y'}}, '*withCoolTyping': True}<br />
wk.check( wks=p, kws={'person': '{name:john,age:30,birthdate:01/11/2016}' }, strict=True )<br />
>>> p.person<br />
{'age': 30, 'birthdate': datetime.datetime(2016, 11, 1), 'name': 'john'}<br />
</code></div>
<br />
<br />
7.2/ *ts:<br />
<br />
*ts expects a Standard Python strptime/strftime string format  (actually based on the C standard (1989 version)).<br />
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at 
<a href=https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior>Python doc.</a><br />
<br />
<u>Syntax:</u>
<br />
Either you use '*ts': <strptime string> and your date entry should match the exact pattern.<br />
or {'*ts': None} and your date entry may be:<br />
&nbsp;&nbsp;&nbsp;&nbsp;- a String supported date literal<br />
&nbsp;&nbsp;&nbsp;&nbsp;- or an instance of datetime.date.<br />
<br />
<u>Examples :</u>
<br />
<br />
a) With a strptime string:<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.birthdate = {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}<br />
wk.check( wks=p, kws={'birthdate': '01/11/2016 16h40mn3s'})<br />
>>> p.birthdate<br />
datetime.datetime(2016, 11, 1, 16, 40, 3)<br />
</code></div>
<br />
- Actually fact wk do this in background:<br />
>>> datetime.datetime(*time.strptime("01/11/2016 16h40mn3s", "%d/%m/%Y %Hh%Mmn%Ss")[0:6])<br />
datetime.datetime(2016, 11, 1, 16, 40, 3))<br />
<br />
Or:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()<br />
p.birthdate = {'*ts': '%d %b %Y:%H:%M:%S'}<br />
wk.check(p, {'birthdate': '29 Aug 2017:02:00:46'})<br />
>>> p.birthdate<br />
datetime.datetime(2017, 8, 29, 2, 0, 46)<br />
</code></div>
<br />

<br />
b) With None<br />
<br />
Value is a datetime:<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import datetime<br />
p = wk.WKS()<br />
p.birthdate = {'*ts': None}<br />
wk.check(p, {'birthdate': datetime.datetime(2016, 11, 1, 16, 40, 3)})<br />
>>> p.birthdate<br />
datetime.datetime(2016, 11, 1, 16, 40, 3)<br />
</code></div>

<br />
Value a Supported datetime literal:<br />
<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()<br />
p.birthdate  = {'*ts': None}<br />
wk.check(p, {'birthdate': 'Tue Jun 16 20:18:03 1981'})<br />
>>> p.birthdate<br />
datetime.datetime(1981, 6, 16, 20, 18, 3)<br />
</code></div>
<br />

<u>TS With Colltyping e.g.:</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthday': {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}}, '*withCoolTyping': True}<br />
wk.check( wks=p, kws={'person': '{name:john,age:30,birthday:01/11/2016 16h40mn3s}' }, strict=True )<br />
>>> p.person<br />
{'age': 30, 'birthday': datetime.datetime(2016, 11, 1, 16, 40, 3), 'name': 'john'}<br />
</code></div>
<br />


<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
8/ MISCELLANEOUS:
</div>

<br />
<br />
8.1> *len:<br />

<br />
<u>Syntax:</u> <b>'*len': &lt;int&gt;</b>
<br />
<br />

<u>Examples :</u>
<br />
<br />


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.size={'*len': 3}<br />
wk.check( wks=p, kws={'size': 'abc'} ) # Will check len('abc') == 3<br />
>>> p.size<br />
'abc'<br />
</code></div>

<br />
<br />
8.2> *minLen, *maxLen:<br />
<br />
<u>Syntax:</u> <b>'*minLen': &lt;int&gt;</b>
<br />
<br />

<u>Examples :</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.size={'*minLen': 3}<br />
wk.check( wks=p, kws={'size': 'abcefg'} ) # Will check len('abc') >= 3<br />
>>> p.size<br />
'abcefg'<br />
</code></div>

<br />
<br />
8.3> *startsWith, *endsWith:<br />
<br />
<u>Syntax:</u> <b>'*startsWith': &lt;value&gt;</b>
<br />
<br />

<u>Examples :</u>
<br />
<br />

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.story={'*startsWith': 'once upon'}<br />
wk.check( wks=p, kws={'story': 'once upon a time'} )<br />
>>> p.story<br />
'once upon a time'<br />
</code></div>

<br />
<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
9/ WKDEF ITSELF:
</div>
<br />
wkDef: check that value is a wk expression.<br />

<br />
<u>Syntax:</u> <b>'*type': '*wkDef'</b>
<br />
<br />
<u>Examples :</u>
<br />
<br />

a) Example with the previous one for *startsWith.<br />


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.wkexp={'*type': 'wkDef'}<br />
wk.check( wks=p, kws={'wkexp':<br />
&nbsp;&nbsp;&nbsp;&nbsp;{'*startsWith': 'once upon'}<br />
}, strict=True )<br />
>>> p.wkexp<br />
{'*startsWith': 'once upon'}<br />
</code></div>


<br />
<br />
b) Exemple with this wk expression from the *dict example:<br />
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'><i>p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}}</i></code></div>
<br />
<br />


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk<br />
p=wk.WKS()<br />
p.wkexp={'*type': 'wkDef', '*withCoolTyping': True}<br />
wk.check( wks=p, kws={'wkexp':<br />
&nbsp;&nbsp;&nbsp;&nbsp;{'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}} # The CoolTyped expression.<br />
}, strict=True )<br />
p.wkexp<br />
{'*type': &lt;class 'dict'&gt;, '*dtype': {'name': {'*type': &lt;class 'str'&gt;}, 'age': {'*type': &lt;class 'int'&gt;}, 'vegan': {'*type': &lt;class 'bool'&gt;}}}<br />
</code></div>
<br />
<br />
<br />
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
10> INFO KEYS:
</div>
<br />

Here are 3 harmless key with no associated function at all, free to be used for any purpose.<br />

<u>Syntax:</u>


<b>
*label: &lt;anything&gt;<br />
*help: &lt;anything&gt;<br />
*lhelp: &lt;anything&gt;<br />
</b>
<br />

</BODY>
</HTML>
