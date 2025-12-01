---
title: 'Exgen'
description: 'The first AI full-stack framework for dynamic, backend-driven HTML generation using LLMs'
pubDate: 'Nov 01 2024'
github: 'https://github.com/alpharaoh/exgen'
---

In the modern age if you want to build a webapp, the structure you'd find is something like this: 

```
Traditional Architecture:
+-------------------+         +-------------------+
|     Frontend      |  <--->  |     Backend       |
+-------------------+         +-------------------+
|  - React/Vue/etc  |   API   |  - Express/etc    |
|  - HTML/CSS/JS    |  calls  |  - Database       |
|  - State mgmt     |         |  - Business logic |
+-------------------+         +-------------------+
```

This new framework, Exgen, collapses this to:

```
Exgen Architecture:
+-------------------------------------------------------+
|                    LLM Backend                        |
+-------------------------------------------------------+
|  - Receives component specification                   |
|  - Executes tools (database, APIs)                    |
|  - Generates HTML directly                            |
|  - Returns complete markup to client                  |
+-------------------------------------------------------+
                           |
                           v
                    +-------------+
                    |   Browser   |
                    | (HTML only) |
                    +-------------+
```

So essentially the HTML that is rendered to the user is fully AI generated from the server. In fact, there is not even a "backend". It's all LLMs, with tool calls to the database or whatever else. 

## Why?

Throughout my career, giving more agency to LLMs (especially smart ones), gives better outputs in general than a strict system of prompts. Imagine if the LLM had full agency over how the backend should behave, and also how data should be represented to the user.

The best UX in modern times is just the one that most people are "fine" with, it's the best average experience across all users in other words. 

No average user gets blown away by good UX - it's more of a default and expectation, and they only pay attention if it's bad. Imagine a world where the webapp you are using molds into the most optimal user experience for YOU. 

Let's say you are using financial software, you only care about your spend in the last 3 months, well in that case you don't care about all the bells and whistles. Another user could be a poweruser that wants to use the same software in a completely different way - Exgen could gain this context and show a completely different interface for each user. Heck, if you're a star wars fan, Exgen could return the webapp in a star wars theme.

## Technical details

Not going to go deep into the technicals, it's a simple, working PoC so feel free to browse the code. 

`-------------------------------------------------------------------------------`

I love JSX, so this framework has it's own JSX style syntax built on babel that looks like this:

```
<application>
  <header output="Navigation bar with logo" cache="force-cache" />

  <table
    output="User list with name and email columns"
    tools={{
      databaseUrl: 'postgres://localhost:5432/mydb',
      schemaDescription: 'users table with id, name, email'
    }}
    cache="none"
  />

  <footer output="Copyright 2024" cache="force-cache" />
</application>
```

This is not HTML tags, but any custom tags you want to define. Imagine you want a table, instead of important a whole component library or using the `<table/>`, `<tr/>` (and the other many table specific tags), you can just define a `<Table />` component, and pass in any props or tools that matter. The name can be anything, as long as it's clear with the output definition. It's kind of like play-pretend, just imagine that you have created such a component. 

Exgen _does_ support importing other Exgen components, of course. 

Now that you've defined this simple component, the LLM will generate it as you defined. You can even try fun things like 

```
<MagicWand 
    output="An image of a wand that, on hover, turns your screen bright yellow" 
/>`
```

There are also different caching methods that may be useful. For example, if the headers and footer do not need to be regenerated all the time, you can define `cache="force-cache"`. If you want the user profile to never be cached and always regenerate for every request, then you can define `cache="none"`. 

## Reactivity

If I would continue exploring this PoC, I would enable clicks on the page to send an event to the server running Exgen, which can be interpreted by the LLM to generate the respective HTML back. Ideally this is done in a smart way using a reference to the HTML element that should be replaced, as done in React SSR. 

To underline this flow with an example, a user clicks on a settings button, that sends a request to the Exgen server, the HTML interprets this as the user wanting to see the settings, so calls the database tool to get any information needed, and then generates HTML for the settings page with that data.

## Exgen Server Flow

Here is a quick diagram of what happens on the Exgen server.

```
+------------------+     +------------------+     +------------------+
|                  |     |                  |     |                  |
|  Component Spec  | --> |   LLM Process    | --> |   HTML Output    |
|  (JSX-like)      |     |                  |     |                  |
+------------------+     +--------+---------+     +------------------+
                                  |
                    +-------------+-------------+
                    |             |             |
                    v             v             v
              +---------+   +---------+   +---------+
              |  Tools  |   |  Cache  |   | Context |
              | (DB,API)|   |  Check  |   |  Data   |
              +---------+   +---------+   +---------+
```


