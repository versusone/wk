<HTML>

<HEAD>
</HEAD>

<BODY style='font-family:courier'>

</br>
</br>
<div style='with=100%;text-align:center;background:cyan;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
I) WantedKeyword (wk) Python Type Checking tool (Python3)  
</div>

  
  
  
<div style='with=100%;text-align:center;background: #ebdef0 ;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
1/ INTRODUCTION
</div>

  
  

<u>What is it ?  </u>
  
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
  
  
<u>Running checks:</u>  
  
Checks can be performed in two ways.  
  
a) Using the check method of the wk module:  
  
Test this into the Python interpreter:  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
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
</code></div>
  
Try this:  
p=wk.WKS()  
p.age={'*type':'int'}  
wk.check (wks=p, kws={'age': 'hello'})  
  
To see what happens !  
  
This can be comfortably embedded into a function or method:  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(name, age):  
&nbsp;&nbsp;&nbsp;&nbsp;import wk  
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()  
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}  
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}  
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws={'name':name, 'age':age})  
  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (p.name, p.age))  
  
>>> hello('john', None)  
Function hello! name: john, age: 30  
</code></div>
  
  
b) Using the cp annotation of the wk module:  
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp  
@cp({'*type': 'str'}, {'*type':'int', '*value':30})  
def hello(name, age):  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))  
  
>>> hello('john')  
Function hello! name: john, age: 30  
</code></div>
  

  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
2/ USING THE CHECK FUNCTION
</div>
  
  
2.1/ A simple function hello  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
def hello(*args, **keywords):  
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()  
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}  
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}  
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=keywords )  
  
&nbsp;&nbsp;&nbsp;&nbsp;print ('function: hello !')  
&nbsp;&nbsp;&nbsp;&nbsp;print ('-----------------')  
&nbsp;&nbsp;&nbsp;&nbsp;print('name: %s' % p.name)  
&nbsp;&nbsp;&nbsp;&nbsp;print('age: %s' % p.age)  
&nbsp;&nbsp;&nbsp;&nbsp;print('parameters as dict: %s' % wk.getAsDict(p))  
  
>>> hello(name='john', age=25)  
  
function: hello !  
-----------------  
name: john  
age: 25  
parameters as dict: {'age': 25, 'name': 'john'}  
>>>
</code></div>
  
- With default value: *value:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello(name='john')  
function: hello !  
-----------------  
name: john  
age: 30 # <-- Default value for age  
parameters as dict: {'age': 30, 'name': 'john'}  
</code></div>  
  
<div style="background: #FFFFC2;border: 1px solid black; border-radius: 10px; padding:10px;">
<u>Lines explained:  </u>
Line 3: p=wk.WKS(): An WKS instance (wk.WKS is alias for wk.WantedKeyword) is created with no Parameter.  
Line 4-5: p.name={'*type':'str'}: p.<attribute-name = <wk expression> : One declares wk expressions on the fly on the wk instance.  
Line 6:wk.check (wks=p, kws=kws ): The wk module's function: check, runs on each Attribute of the wk.WKS instances: "name" and "age".  
Regarding the wk expression defined for each one.  
The values are retreives from the kws parameter.  
The final values are stored on the attributes of the WKS instance (p).  
  
a) wk.check retreives the user value for the attribute name, from kws['name'].  
b) wk.check verifies this value on the wk expresion: {'*type':'str'}, defined for attribute name.  
c) wk.check applies the final value to p.name  
</div>
  
  
- More on extra Parameters:  
  
If you run the following call, you'll see that no error is generated althought parameter pc is not defined.  
>>> hello(name='john', age=25, pc='12345')  
To reject extra parameter you must use the keyword: strict.  
  
  
2.2/ Same function but using strict  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(*args, **kws):  
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()  
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}  
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}  
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=kws, strict=True )  
  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))  
</code></div>
  
- Using the extra parameter: pc causes an error, because of strict=True:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello(name='john', age=25, pc='12345')  
Traceback (most recent call last):  
...  
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On strick check:  
Unsupported Attribute: pc !. Supported Attributes are: age, name. <==  
</code></div>  
  
  
As you can see into this excerpts from the previous exception: "SubClass:None SubMethod:None",  
The calling class and method is not traced.  
To trace them one should use class_exit and method_exit as follows.  
  
  
2.3/ Same function but using class_exit and method_exit  
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(*args, **kws):  
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()  
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}  
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}  
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws=kws, strict=True, class_exit='Main', method_exit='hello')  
  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))  
  
>>> hello(name='john', age=25, pc='12345')  
Traceback (most recent call last):  
...  
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:Main SubMethod:hello On strick check: <==  
Unsupported Attribute: pc !. Supported Attributes are: age, name.  
>>>
</code></div>
  
  
2.4/ Same function with arguments instead of parameters:  
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
def hello(name, age):  
&nbsp;&nbsp;&nbsp;&nbsp;p=wk.WKS()  
&nbsp;&nbsp;&nbsp;&nbsp;p.name={'*type':'str'}  
&nbsp;&nbsp;&nbsp;&nbsp;p.age={'*type':'int', '*value':30}  
&nbsp;&nbsp;&nbsp;&nbsp;wk.check (wks=p, kws={'name':name, 'age':age}, strict=True )  
  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! parameters as dict: %s' % wk.getAsDict(p))  
  
>>> hello(name='john', age=25)  
Function hello! parameters as dict: {'age': 25, 'name': 'john'}  
</code></div>
  
  

  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
3/ USING THE CP ANNOTATION
</div>
  
cp (wk.cp is ans alias for wk.check_parameters) is an annotation that simplifies declaring wk expressions on Arguments and Parameters.  
  
  
<u>3.1/ As Arguments:</u>  
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp  
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)  
def hello(name, age):  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))  
  
>>> hello('john', 25)  
Function hello! name: john, age: 25  
</code></div>
  
- Calling with None:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
>>> hello('john', None)  
Function hello! name: john, age: 30  
</code></div>
  
  
- hello with *args signature:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp  
@cp({'*type': 'str'}, {'*type':'int', '*value':30}, strict=True)  
def hello(*args):  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! Arguments: %s' % str(args))  
  
- This trigger an error because on strict=True:  
>>> hello('john', 25, 'blabla')  
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:wkDecorator/args/str SubMethod:hello  
On strick check: Unsupported Attribute: arg2 !. Supported Attributes are: arg0, arg1.  
</code></div>
  
  
<u>3.2/ As parameters:</u>  
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp  
@cp(  
&nbsp;&nbsp;&nbsp;&nbsp;kws={  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'name': {'*type':'str'},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'age': {'*type':'int', '*value':30},  
&nbsp;&nbsp;&nbsp;&nbsp;}, strict=True)  
def hello(name=None, age=None):  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! name: %s, age: %s' % (name, age))  
  
>>> hello(name='john')  
Function hello! name: john, age: 30  
</code></div>
  
- Mixed:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
from wk import cp  
@cp({'*type': 'str', '*value': 'john'}, {'*type': 'int', '*value': 30},  
&nbsp;&nbsp;&nbsp;&nbsp;kws={  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'pc': {'*reg': '^\d{5}([\-]?\d{4})?$'},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'vegan': {'*type': 'bool', '*value': True}  
&nbsp;&nbsp;&nbsp;&nbsp;}, strict=True)  
def hello(*args, pc=None, age=None, vegan=False):  
&nbsp;&nbsp;&nbsp;&nbsp;print('Function hello! arguments: %s, pc: %s, vegan: %s' % (str(args), pc, vegan))  
  
>>> hello()  
Function hello! arguments: ('john', 30), pc: None, vegan: True  
>>> hello('mia', pc='12345-9876')  
Function hello! arguments: ('mia', 30), pc: 12345-9876, vegan: True  
</code></div>
  
  
  
<div style='with=100%;text-align:center;background:cyan;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
II) wk Expressions in Depht
</div>

  
  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
1> NO CHECK AT ALL:
</div>
  
  
a) No check with None:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.any=None  
wk.check (wks=p, kws={'any': 'hello'}, strict=True )  
>>> p.any  
'hello'  
</code></div>
  

  
b) No check with default attribute:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.any='hello'  
wk.check (wks=p, kws={}, strict=True )  
>>> p.any  
'hello'  
</code></div>
  

  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
2/ WORKING WITH WK BASE TYPES:
</div>
  
  
2.1> *type:  
  
*type: checks the value (and default value) for the provided &lt;type&gt;.  
  
Supported types are listed in: wk.SUPPORTED_TYPES:  
&nbsp;&nbsp;&nbsp;&nbsp;('str', 'int', 'float', 'tuple', 'list', 'dict', 'bool', 'xml', 'wkDef')  
Or  
Any Python type: (a Python type is an obj for which this is True: isinstance(obj, type))  
Or  A list of Python types.  
  
<u>Syntax:</u> <b>'*type': &lt;type&gt;</b>
  
  

<u>Note:</u>
  
- a) When <type> is a Python type (or a list of ...),  wk always perform isintance ans issubclass on it.  
And will return an error only if both are false.  
- b) &lt;type&gt; : can be provided as string: like in {'*type': 'int'},  
or not: like in {'*type': int}  
This is true only for Python standard base types: str, int, float, tuple, list, dict, bool.  
  
  
<u>Examples:</u>
  
  
  
a) with Base types:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.age={'*type': int}  
wk.check (wks=p, kws={'age': 5})  
>>> p.age  
5  
</code></div>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.vegan={'*type': bool}  
wk.check (wks=p, kws={'vegan': False})  
>>> p.vegan  
False  
</code></div>
  
b) with any Pyhon types:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
class A:pass  
  
class B(A, list):pass  
  
p=wk.WKS()  
p.obj={'*type': A}  
wk.check ( wks=p, kws={'obj': A()} )  
>>> p.obj  
<__main__.A object at 0x00000000032BAD68>  
</code></div>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.obj={'*type': (A, list)}  
wk.check ( wks=p, kws={'obj': B()} )  
>>> p.obj  
[]  
</code></div>
  
  
2.2/ *value:  
  
*value: Provide a default value to a field.  
  
<u>Syntax:</u> <b>'*value': &lt;any&gt;  </b>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.name={'*value': 'john'}  
wk.check ( wks=p, kws={'name': None} )  
>>> p.name  
'john'  
</code></div>
  
  
2.3/ *required:  
  
*required raises an exception if no value is provided for the field.  
  
<u>Syntax:</u> <b>'*required': True</b>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.name={'*required': True}  
wk.check ( wks=p, kws={'name': None} )  
  
wkexception.wkSystemException: EWK: From class:wk, from method:getKeywords SubClass:None SubMethod:None On *Required:Received no value  
 for required key:name. Your wkDefinition: {'*required': True}  
</code></div>
  
  

  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
3/ COMPARATORS:
</div>
  
  
3.1/ *lt, *gt, *eq, *le, *ge, *ne:  
  
All are unary comparators.  
And compare anything that is comparable by the standard Python operators (<, >, ==, <=, >=, !=).  
  
<u>Syntaxe:</u> <b>'*lt': &lt;value&gt;</b>
  
  
<u>Examples:</u>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.item={'*lt': 'bbb'}  
wk.check ( wks=p, kws={'item': 'aaa'} )  
>>> p.item  
'aaa'  
</code></div>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.index={'*lt': 4}  
wk.check ( wks=p, kws={'index': 2} )  
>>> p.index  
2  
</code></div>
  
  

3.2/ *between:  
  
*between is a binary comparator.  
  
<u>Syntaxe:</u> <b>'*between': &lt;list/tuple&gt;</b>


<u>Examples:</u>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.item={'*between': ('aaa', 'ccc')}  
wk.check ( wks=p, kws={'item': 'bbb'} )  
>>> p.item  
'bbb'  
</code></div>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.item={'*between': ('aaa', 'ccc')}  
wk.check ( wks=p, kws={'item': 'zbbb'} )  
  
wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *between:Received Bad  
value for key:item  Value Expected: between ('aaa', 'ccc').  Value Received:zbbb. Your wkDefinition: {'*between': ('aaa', 'ccc')}  
</code></div>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.item={'*between': (1, 3)}  
wk.check ( wks=p, kws={'item': 2} )  
>>> p.item  
2  
</code></div>
  
  

  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
4/ DEEP INTO COMPLEXE TYPES DICT, LIST and TUPLE:
</div>
  
  

4.1/ *ltype:  
  
*ltype is an advanced form of {'*type': list} or {'*type': tuple}.  
*ltype is only supported for {'*type': list} or {'*type': tuple}.  
*ltype will describe the type of the list items by a new wk definition.  
  
<u>Syntax:</u> <b>'*type': list, '*ltype': &lt;wk expression&gt;</b>
  
  
<u>Examples:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.cars={'*type': list, '*ltype': {'*type': int}}  
wk.check( wks=p, kws={'cars': [1,2,3,4,5]} )  
>>> p.cars  
[1, 2, 3, 4, 5]  
</code></div>
  
  
<u>List with Colltyping e.g.:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.cars={'*type': list, '*ltype': {'*type': int}, '*withCoolTyping': True}  
wk.check( wks=p, kws={'cars': '[1,2,3,4,5]'} )  
>>> p.cars  
[1, 2, 3, 4, 5]  
</code></div>
  
  

4.2/ *dtype:  
  
*dtype is an advanced form of {'*type': dict}.  
*dtype is only supported for {'*type': dict}.  
*dtype will describe each key of the expected dictionary with a new wk definition.  
  
<u>Syntax:</u> <b>'*type': dict, '*dtype': {'key1': &lt;wk expression&gt;, ['keyn': &lt;wk expression&gt;]}</b>
  
  
<u>Examples:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*required': True}  
wk.check( wks=p, kws={'person': {'name': 'john', 'age': 30, 'vegan' : False} })  
>>> p.person  
{'age': 30, 'vegan': False, 'name': 'john'}  
</code></div>
  
  
<u>Dict with Colltyping e.g.:</u>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}, '*withCoolTyping': True}  
wk.check( wks=p, kws={'person': '{name:john,age:30,vegan:False}' })  
>>> p.person  
{'age': 30, 'vegan': False, 'name': 'john'}  
</code></div>
  
<u>List of dict:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.lperson={'*type': list, '*ltype':{  
&nbsp;&nbsp;&nbsp;&nbsp;'*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}  
}}  
wk.check( wks=p, kws={'lperson': [  
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'john', 'age': 30, 'vegan' : False},  
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'jim', 'age': 40, 'vegan' : True},  
&nbsp;&nbsp;&nbsp;&nbsp;{'name': 'lee', 'age': 50, 'vegan' : False}  
]})  
>>> p.lperson  
[{'age': 30, 'name': 'john', 'vegan': False}, {'age': 40, 'name': 'jim', 'vegan': True}, {'age': 50, 'name': 'lee', 'vegan': False}]  
>>>  
</code></div>
  
  
  
4.3/ *checkIn:  
  
checkIn checks a value in a list (or tuple).  
  
<u>Syntax:</u> <b>'*checkIn': &lt;tuple or list&lt;</b>
  

  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.color={'*type': str, '*checkIn': ('green', 'violet', 'red')}  
wk.check( wks=p, kws={'color': 'violet'} )  
>>> p.color  
'violet'  
</code></div>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.any={'*type': int, '*checkIn': (False, 5, ('hello',))}  
wk.check( wks=p, kws={'any': 5} )  
>>> p.any  
5  
</code></div>
  


  
  
4.4/ *checkXIn:  
  
*checkXIn checks the presence of all elements of a list (or tuple) into another list (or tuple).  
value must be a list ('*type': list or '*type': tuple).  
  
<u>Syntax:</u> <b>'*checkXIn': &lt;tuple or list&gt;</b>
  
  


<u>Examples:</u>
  

  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.colors={'*type': tuple, '*checkXIn': ('green', 'violet', 'red')}  
wk.check( wks=p, kws={'colors': ('violet', 'green')} )  
>>> p.colors  
('violet', 'green')  
</code></div>
  
  

  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
5/ CONVERTIONS:
</div>
  
  

Sometimes the input value for the field is not formated like you want it.  
And you migth want to apply a specific convertion to this value before to perform any control on it.  
The following wk keys stands for this purpose.  
Convertions are always performed first and Controls afterward.  
Convertions are applyied on any value or default value (*value) for a field.  
  

  
5.1/ *force_str:  
  
*force_str: force the convertion of an obj to a string.  
  
<u>Syntax:</u> <b>'force_str': True</b>
  
Note: if value is a bytes, wk will try: value.decode('utf-8') on it.  
  
<u>Examples:</u>
  
  
a) An int to a string:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.name={'*force_str': True, '*type': str} # While simple this fails: {'*type': str}  
wk.check ( wks=p, kws={'name': 123 } )  
>>> p.name  
'123'  
</code></div>
  
b) A byte to a string:  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.name={'*force_str': True, '*type': str}  
wk.check ( wks=p, kws={'name': 'abc'.encode('utf-8') } )  
>>> p.name  
'abc'  
</code></div>
  

  
  
5.2/ *withEval:  
  
  
<b>Restricted evals:</b>  
*withEval is supported by a python eval handled par the _eval function of the ct.py mofdule.  
The _eval function use a restricted eval is the sens that it uses no locals, globals no extra function or modules.  
But a restricted "__builtins__" list of base types:  
&nbsp;&nbsp;&nbsp;&nbsp;('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').  
So any of these base type can be evaluated.  
  
The field's value in input, must always be a String.  
  
<u>Syntax:</u> <b>'*withEval': True</b>
  
  

<u>Examples:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.age={'*type': int, '*withEval': True}  
wk.check (wks=p, kws={'age': '5'})  
>>> p.age  
5  
</code></div>
  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.age={'*type': float, '*withEval': True}  
wk.check (wks=p, kws={'age': '5.0'})  
>>> p.age  
5.0  
</code></div>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.cars={'*type': tuple, '*withEval': True}  
wk.check (wks=p, kws={'cars': '(1, 2, 3)'})  
>>> p.cars  
(1, 2, 3)  
</code></div>

  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.person={'*type': dict, '*withEval': True}  
wk.check (wks=p, kws={  
&nbsp;&nbsp;&nbsp;&nbsp;'person': "{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}"  
})  
>>> p.person  
{'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}  
</code></div>
  
  
And so on !  
  
  

5.3/ *withCoolTyping:  
  
*withCoolTyping is a more advanced version of *withEval.  
*withCoolTyping stands for convertion of string values with no: ' and no: " to any of the previous base types:  
&nbsp;&nbsp;&nbsp;&nbsp;('None', 'False', 'True', 'str', 'int', 'float', 'bool', 'tuple', 'list', 'dict').  
*withCoolTyping stands to be used in environment that mess with: ' and " like terminal, console, xml for example.  
  
The field's value in input, must always be a String.  
  
<u>Syntax:</u> <b>'*withCoolTyping': True</b>
  
  
<u>Examples:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.person={'*type': dict, '*withCoolTyping': True}  
wk.check (wks=p, kws={  
&nbsp;&nbsp;&nbsp;&nbsp;'person': "{name:john,age:30,adress:21 jump street,vegan:True}" # Note that all not significative spaces must be wiped off !  
})  
>>> p.person  
'name': 'john', 'age': 30, 'adress': '21 jump street', 'vegan': True}  
  
p=wk.WKS()  
p.story={'*type': tuple, '*withCoolTyping': True}  
wk.check (wks=p, kws={  
&nbsp;&nbsp;&nbsp;&nbsp;'story': "(once,upon a time,in the east)"  
})  
>>> p.story  
('once', 'upon a time', 'in the east')  
</code></div>
  
And so on !  
  
  
5.4/ *withConvert:  
  
*withConvert is to be used when none of the previous convertion method works.  
*withConvert takes a user function in its description, and calls this function on any value or default value (*value) for the field.  
The field's value in input, may be of any type.  

  
<u>Syntax:</u> <b>'*withConvert': &lt;Python function(value)&gt;</b>
  

  
<u>Examples:</u>
  
  


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.age={'*type': int, '*withConvert': lambda e: int(e)}  
wk.check (wks=p, kws={  
&nbsp;&nbsp;&nbsp;&nbsp;'age': "123"  
})  
>>> p.age  
123  
</code></div>
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.person={'*type': int, '*withConvert': lambda e: e == 'john'}  
wk.check (wks=p, kws={  
&nbsp;&nbsp;&nbsp;&nbsp;'person': "john"  
})  
>>> p.person  
True  
</code></div>
  

  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
6/ REGULAR EXPRESSION:
</div>
  
6.1/ *reg:  
*reg will check any value (or the default value) on this regular expression.  
  
<u>Syntax:</u> <b>'*reg': '&lt;regular_expression&gt;'</b>
  
  
<u>Examples:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.pc={'*reg': '^\d{5}([\-]?\d{4})?$'}  
wk.check (wks=p, kws={'pc': "12345-9876"})  
>>> p.pc  
"12345-9876"  
</code></div>
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p=wk.WKS()  
p.pc={'*reg': '^wel.+'}  
wk.check (wks=p, kws={'pc': "wel"})  
  
wkexception.wkSystemException: EWK: From class:wk, from method:__checkType SubClass:None SubMethod:None On *reg:Received Bad  value fo  
r key:pc  Value Expected: reg:dct['*reg'].  Value Received:wel, type:<class 'str'>. Your wkDefinition: {'*reg': '^wel.+'}  
  
p=wk.WKS()  
p.pc={'*reg': '^wel.+'}  
wk.check (wks=p, kws={'pc': "welcome"})  
>>> p.pc  
'welcome'  
</code></div>
  
  

  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
7/ DATE AND TIMESTAMP:
</div>
  

  
7.1/ *date:  
  
*date expects a Standard Python strptime/strftime string format  (actually based on the C standard (1989 version)).  
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at  
&nbsp;&nbsp;&nbsp;&nbsp;https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior  
  
<u>Syntax:</u>
  
Either you use '*date': <strptime string> and your date entry should match the exact pattern.  
or {'*date': None} and your date entry may be:  
&nbsp;&nbsp;&nbsp;&nbsp;- a String supported date literal  
&nbsp;&nbsp;&nbsp;&nbsp;- or an instance of datetime.date.  
  

<u>Examples :</u>
  
a) With a strptime string:  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.birthday = {'*date': '%d/%m/%Y'}  
wk.check( wks=p, kws={'birthday': '01/11/2016'})  
>>> p.birthday  
datetime.datetime(2016, 11, 1)  
  
- In fact wk do this in background:  
>>> datetime.date(*time.strptime("01/11/2016", "%d/%m/%Y")[0:3])  
datetime.date(2016, 11, 1)  
</code></div>
  
Or:  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()  
p.birthday = {'*date': '%d %b %Y'}  
wk.check(p, {'birthday': '29 Aug 2017'})  
>>> p.birthday  
datetime.date(2017, 8, 29)  
</code></div>
  
  
b) With None  
  
Value is a datetime:  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import datetime  
p = wk.WKS()  
p.birthday = {'*date': None}  
wk.check(p, {'birthday': datetime.date(2016, 11, 1)})  
>>> p.birthday  
datetime.date(2016, 11, 1)  
</code></div>
  
Value a Supported date literal:  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()  
p.birthday  = {'*date': None}  
wk.check(p, {'birthday': 'Tue Jun 16 20:18:03 1981'})  
>>> p.birthday  
datetime.date(1981, 6, 16)  
</code></div>
  

<u>DATE With Colltyping e.g.:</u>
  


  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthdate': {'*date': '%d/%m/%Y'}}, '*withCoolTyping': True}  
wk.check( wks=p, kws={'person': '{name:john,age:30,birthdate:01/11/2016}' }, strict=True )  
>>> p.person  
{'age': 30, 'birthdate': datetime.datetime(2016, 11, 1), 'name': 'john'}  
</code></div>
  
  
7.2/ *ts:  
  
*ts expects a Standard Python strptime/strftime string format  (actually based on the C standard (1989 version)).  
See for instance the grid from chapter: "8.1.8. strftime() and strptime() Behavior" at 
<a href=https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior>Python doc.</a>  
  
<u>Syntax:</u>
  
Either you use '*ts': <strptime string> and your date entry should match the exact pattern.  
or {'*ts': None} and your date entry may be:  
&nbsp;&nbsp;&nbsp;&nbsp;- a String supported date literal  
&nbsp;&nbsp;&nbsp;&nbsp;- or an instance of datetime.date.  
  
<u>Examples :</u>
  
  
a) With a strptime string:  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.birthdate = {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}  
wk.check( wks=p, kws={'birthdate': '01/11/2016 16h40mn3s'})  
>>> p.birthdate  
datetime.datetime(2016, 11, 1, 16, 40, 3)  
</code></div>
  
- Actually fact wk do this in background:  
>>> datetime.datetime(*time.strptime("01/11/2016 16h40mn3s", "%d/%m/%Y %Hh%Mmn%Ss")[0:6])  
datetime.datetime(2016, 11, 1, 16, 40, 3))  
  
Or:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()  
p.birthdate = {'*ts': '%d %b %Y:%H:%M:%S'}  
wk.check(p, {'birthdate': '29 Aug 2017:02:00:46'})  
>>> p.birthdate  
datetime.datetime(2017, 8, 29, 2, 0, 46)  
</code></div>
  

  
b) With None  
  
Value is a datetime:  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import datetime  
p = wk.WKS()  
p.birthdate = {'*ts': None}  
wk.check(p, {'birthdate': datetime.datetime(2016, 11, 1, 16, 40, 3)})  
>>> p.birthdate  
datetime.datetime(2016, 11, 1, 16, 40, 3)  
</code></div>

  
Value a Supported datetime literal:  
  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
p = wk.WKS()  
p.birthdate  = {'*ts': None}  
wk.check(p, {'birthdate': 'Tue Jun 16 20:18:03 1981'})  
>>> p.birthdate  
datetime.datetime(1981, 6, 16, 20, 18, 3)  
</code></div>
  

<u>TS With Colltyping e.g.:</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'birthday': {'*ts': '%d/%m/%Y %Hh%Mmn%Ss'}}, '*withCoolTyping': True}  
wk.check( wks=p, kws={'person': '{name:john,age:30,birthday:01/11/2016 16h40mn3s}' }, strict=True )  
>>> p.person  
{'age': 30, 'birthday': datetime.datetime(2016, 11, 1, 16, 40, 3), 'name': 'john'}  
</code></div>
  


  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
8/ MISCELLANEOUS:
</div>

  
  
8.1> *len:  

  
<u>Syntax:</u> <b>'*len': &lt;int&gt;</b>
  
  

<u>Examples :</u>
  
  


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.size={'*len': 3}  
wk.check( wks=p, kws={'size': 'abc'} ) # Will check len('abc') == 3  
>>> p.size  
'abc'  
</code></div>

  
  
8.2> *minLen, *maxLen:  
  
<u>Syntax:</u> <b>'*minLen': &lt;int&gt;</b>
  
  

<u>Examples :</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.size={'*minLen': 3}  
wk.check( wks=p, kws={'size': 'abcefg'} ) # Will check len('abc') >= 3  
>>> p.size  
'abcefg'  
</code></div>

  
  
8.3> *startsWith, *endsWith:  
  
<u>Syntax:</u> <b>'*startsWith': &lt;value&gt;</b>
  
  

<u>Examples :</u>
  
  

<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.story={'*startsWith': 'once upon'}  
wk.check( wks=p, kws={'story': 'once upon a time'} )  
>>> p.story  
'once upon a time'  
</code></div>

  
  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
9/ WKDEF ITSELF:
</div>
  
wkDef: check that value is a wk expression.  

  
<u>Syntax:</u> <b>'*type': '*wkDef'</b>
  
  
<u>Examples :</u>
  
  

a) Example with the previous one for *startsWith.  


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.wkexp={'*type': 'wkDef'}  
wk.check( wks=p, kws={'wkexp':  
&nbsp;&nbsp;&nbsp;&nbsp;{'*startsWith': 'once upon'}  
}, strict=True )  
>>> p.wkexp  
{'*startsWith': 'once upon'}  
</code></div>


  
  
b) Exemple with this wk expression from the *dict example:  
<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'><i>p.person={'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}}</i></code></div>
  
  


<div style="background: #F2F2F2;border: 1px solid black; border-radius: 10px; padding:10px;"><code  style='font-family:verdana;font-size:0.90em;'>
import wk  
p=wk.WKS()  
p.wkexp={'*type': 'wkDef', '*withCoolTyping': True}  
wk.check( wks=p, kws={'wkexp':  
&nbsp;&nbsp;&nbsp;&nbsp;{'*type': dict, '*dtype': {'name': {'*type': str}, 'age': {'*type':int}, 'vegan': {'*type': bool}}} # The CoolTyped expression.  
}, strict=True )  
p.wkexp  
{'*type': &lt;class 'dict'&gt;, '*dtype': {'name': {'*type': &lt;class 'str'&gt;}, 'age': {'*type': &lt;class 'int'&gt;}, 'vegan': {'*type': &lt;class 'bool'&gt;}}}  
</code></div>
  
  
  
<div style='with=100%;text-align:center;background:#ebdef0;border-top-style: solid;border-bottom-style:solid;border-width:0.5;'>
10> INFO KEYS:
</div>
  

Here are 3 harmless key with no associated function at all, free to be used for any purpose.  

<u>Syntax:</u>


<b>
*label: &lt;anything&gt;  
*help: &lt;anything&gt;  
*lhelp: &lt;anything&gt;  
</b>
  

</BODY>
</HTML>
