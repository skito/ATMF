# Advanced-Template-Markup-Format  / Culturral Made Easy

## What it is?
ATMF is HTML based format which is specially designed to simplify linking between the front and back end. It provides high availability of server-side data at template level.


## Why is needed?
Building of medium-high complex localized web solutions can be real time and effort consumer, especially when it comes to providing front-end templates for UI manipulation without changing the back-end. Adding multi-cultural support to the apps usually takes a lot of efforts and additional steps at development stage which make it harder to build and scale. ATMF is designed to skip all the additional “work to be done” and allow the developers to start creating localized templates with highly available back-end data immediately with ease.


## What it looks like?
The template:
```html
<html>
  <head>
    <title>{@header.title}</title>
  </head>
  <body>
    <h1>{@header.helloWorld}</h1>
    <p>{@page.todayDate} {/fdate $date}</p>
    <p>{@page.visitorNum $count} </p>
    <p>{@page.others $othersCount} </p>
  </body>
</html>
```

The output:
```html
<html>
  <head>
    <title>Hello World</title>
  </head>
  <body>
    <h1>Hello World</h1>
    <p>Today is 2020-12-04</p>
    <p>You are visitor number 7</p>
    <p>6 other visitors came before you.</p>
  </body>
</html>
```

The translation:<br>
```json
en/page.json
{
  todayDate: "Today is",
  vistorNum: "You are visitor number $0"
  others: [
    "$0 more visitors", 
    "One more visitor"
  ]
}

```

## What it takes?
It just needs server-side processor and it's ready to go. Lightweight libraries to process the template files are currently underconstruction for PHP and C#.

## Go deeper...
ATMF uses four component __SLVE (#@$/)__ syntax system.

\#   System Function<br>
@    Language Resource<br>
$    Variable<br>
/    Extension<br>

Where each action or resource is defined by its special char ``#@$/`` making template building super-fast and easy.


### \# System functions
These include: ``#if`` ``#else`` ``#endif`` ``#use``

### @ Language resources
The interpreter automatically discovers language resources target with ``@``. Translations are organized in files in special folder order:

```
culture/
|__.culture.json
|__en-US/
   |__page.json
   |__header.json
   |__settings/
      |__profile.json
|__bg-BG/
   |__page.json
   |__header.json
   |__settings/
```

Each language resource key is consisting of ``@path/file.key``, like ``@settings/profile.name``. It can be also combined with the ``#use`` for additional simplifying.

```
{#use lang settings}
{@profile.name}
```
__OR__
```
{#use lang settings/profile as p}
{@p.name}
```

The language resources can be also combined with ``$variables`` like: ``{@profile.name $fullName}``. This will pass ``$fullName`` value to ``@profile.name`` language resource. Multiple values in the translations can be captured by index variables ``$0``, ``$1``, ``$2`` etc..

```
theFox: "The $1 fox is making $0 steps" 
=> {@page.theFox $steps $color}
```

ATMF is easily handling the plural nouns.

```
theFox: ["The $1 fox made $0 steps",  "The $1 fox made one step"]
=> {@page.theFox $steps $color}
```

This will automatically decide whether single or plural noun sentence should be used based on the first parameter. If it's number and equal to 1 then it will be single noun sentence. Anything else will evaluate to plural.

```
=> {@page.theFox 1 red} => The red fox made one step
=> {@page.theFox 8 blue} => The blue fox made 8 steps
=> {@page.theFox 0 green} => The green fox made 0 steps
```

### $ Variables
They are assigned from the back-end and can be used separately or in combinations with other operators.

```
{$totalCount}
{@page.hello $fullName}
```

### / Extensions
These are third-party functions for processing the template data. Same as the system functions they accept parameters.

```
{/myformat $totalCount @page.steps}
{@pag.steps {/myFormat $totalCount}}
```



