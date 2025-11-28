---
title: 'Exgen'
description: 'Framework for dynamic, backend-driven HTML generation using LLMs'
pubDate: 'Nov 01 2024'
github: 'https://github.com/alpharaoh/exgen'
---

A framework for dynamic, backend-driven HTML generation using Large Language Models to construct web applications. It integrates LLM capabilities directly into backend processes rather than maintaining separate frontend/backend architectures.

## The Concept

Traditional web architecture separates concerns:

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

Exgen collapses this:

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

## Request Flow

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

## Component Example

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

## Caching Strategy

```
+------------------+     +------------------+
|   force-cache    |     |      none        |
+------------------+     +------------------+
|                  |     |                  |
| Static content   |     | Dynamic content  |
| Generated once   |     | Regenerated per  |
| Cached forever   |     | request          |
|                  |     |                  |
| e.g. navigation, |     | e.g. user data,  |
| footer, branding |     | live feeds       |
+------------------+     +------------------+
```

## Tech Stack

- **Language:** TypeScript (94%)
- **Build:** Babel
- **Package Manager:** pnpm
- **Runtime:** Node.js

## Why This Approach?

```
+---------------------------------+
|  LLM handles simultaneously:    |
+---------------------------------+
|  [x] Backend logic              |
|  [x] Data retrieval             |
|  [x] HTML generation            |
|  [x] UI decisions               |
+---------------------------------+
         |
         v
+---------------------------------+
|  Result:                        |
+---------------------------------+
|  - No frontend framework needed |
|  - No API layer to maintain     |
|  - Dynamic UI from descriptions |
+---------------------------------+
```
