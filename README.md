# Advanced-Template-Markup-Format (ATMF) [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Build%20complex%20localized%20web%20solutions%20and%20apps%20with%20%23ATMF%0A%0A&url=https://github.com/skito/ATMF)

__CULTURAL MADE EASY__

## What it is?
ATMF is extendable format which is specially designed to simplify linking between the front and back end. It provides high availability of back end data at template level and integrates very well with HTML, CSS and other markup languages. It also gives added value to event-driven architectures by creating a live bond between controls and dynamic data.


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
  "todayDate": "Today is",
  "vistorNum": "You are visitor number $0",
  "others": [
    "$0 more visitors came before you", 
    "One more visitor came before you"
  ]
}

```

## What it takes?
It just needs server-side processor and it's ready to go. Lightweight libraries to process the template files are currently underconstruction for [PHP](https://github.com/skito/ATMF-PHP), Swift, C# and [Javascript](https://github.com/skito/ATMF-JS).

[Translations manager for Windows](https://github.com/skito/ATMF-TranslationsTool-Windows) is also available.

## Go deeper...
ATMF uses four component __SLVE (#@$/)__ syntax system.

\#   System Function<br>
@    Language Resource<br>
$    Variable<br>
/    Extension<br>

Where each action or resource is defined by its special char ``#@$/`` making template building super-fast and easy.


### \# System functions
These include: ``#if`` ``#else`` ``#end`` ``#use`` ``#template`` ``#label``

```html
<html>
  <head>
    <title>{@header.title}</title>
  </head>
  <body>
    <!-- Nested templates - handle master/page views -->
    <div>
      {#if $loggedIn}
        {#template header logged_in}
      {#else}
        {#template header login}
      {#endif}
    </div>
    <div>{#template page}</div>
    <div>{#template footer}</div>
  </body>
</html>
```

```html
<!-- Inside header template -->
<!-- Label different code snippets to use them with {#template path/name label} -->
{#label login}
  <div>
    <a href="/login">Login</a> or <a href="/signup">Sign up</a>
  </div>
{#endlabel}

{#label logged_in}
  <div>
    <a href="/settings">Settings</a>  
    <a href="/logout">Logout</a>
  </div>
{#endlabel}
```

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
{#use lang settings/profile}
{@name}
```
__OR__
```
{#use lang settings/profile as p}
{@p.name}
```

The language resources can be also combined with ``$variables`` like: ``{@profile.name $fullName}``. This will pass ``$fullName`` value to ``@profile.name`` language resource. Multiple values in the translations can be captured by index variables ``$0``, ``$1``, ``$2`` etc..

```html
<!-- Translation resource -->
theFox: "The $1 fox made $0 steps" 

<!-- Usage -->
{@page.theFox $steps $color}
```

ATMF is easily handling the plural nouns.

```html
<!-- Translation resource -->
theFox: ["The $1 fox made $0 steps",  "The $1 fox made one step"]

<!-- Usage -->
{@page.theFox $steps $color}
```

This will automatically decide whether single or plural noun sentence should be used based on the first parameter. If it's number and equal to 1 then it will be single noun sentence. Anything else will evaluate to plural.

```html
<!-- Usage -->
{@page.theFox 1 red}   <!-- Output: The red fox made one step -->
{@page.theFox 8 blue}  <!-- Output: The blue fox made 8 steps -->
{@page.theFox 0 green} <!-- Output: The green fox made 0 steps -->
```

### $ Variables
They are assigned from the back-end and can be used separately or in combinations with other operators.

```
{$totalCount}
{@page.hello $fullName}
```

### / Extensions
These are third-party (custom) functions for processing the template data. Same as the system functions they accept parameters.

```
{/myformat $totalCount @page.steps}
{@page.steps {/myFormat $totalCount}}
```



