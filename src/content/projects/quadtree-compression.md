---
title: 'Quadtree Compression'
description: 'Image compression using quadtree data structures for recursive spatial partitioning'
pubDate: 'Mar 22 2021'
github: 'https://github.com/alpharaoh/Quadtree-Compression'
---

An image compression utility that leverages quadtree data structures to reduce image file sizes while maintaining visual quality through recursive spatial partitioning.

## How It Works

The algorithm progressively subdivides an image into four quadrants at each level. Subdivisions continue if the depth limit hasn't been exceeded or the quadrant contains excessive detail.

```
+------------------+         +--------+--------+
|                  |         |        |        |
|                  |         |   Q1   |   Q2   |
|     Original     |  --->   +--------+--------+
|      Image       |         |        |        |
|                  |         |   Q3   |   Q4   |
+------------------+         +--------+--------+

                    Further subdivision based on detail:

+--------+--------+         +----+---+--------+
|        |        |         | a | b |        |
|   Q1   |   Q2   |  --->   +---+---+   Q2   |
+--------+--------+         | c | d |        |
|        |        |         +---+---+--------+
|   Q3   |   Q4   |         |        |        |
+--------+--------+         |   Q3   |   Q4   |
                            +--------+--------+
```

## Algorithm Flow

```
                    +-------------------+
                    |   Load Image      |
                    +--------+----------+
                             |
                             v
                    +-------------------+
                    | Calculate Detail  |
                    | for Region        |
                    +--------+----------+
                             |
              +--------------+--------------+
              |                             |
              v                             v
    +-------------------+         +-------------------+
    | Detail > Threshold|         | Detail <= Threshold|
    | AND Depth < Max   |         | OR Depth >= Max   |
    +--------+----------+         +--------+----------+
             |                             |
             v                             v
    +-------------------+         +-------------------+
    | Subdivide into    |         | Store average     |
    | 4 quadrants       |         | color for region  |
    +--------+----------+         +-------------------+
             |
             v
    +-------------------+
    | Recurse on each   |
    | quadrant          |
    +-------------------+
```

## Quadtree Data Structure

```
                        [Root]
                       /  |  \  \
                      /   |   \  \
                   [Q1] [Q2] [Q3] [Q4]
                   /|\
                  / | \
               [a][b][c][d]

Each node stores:
- Bounding box coordinates
- Average color (RGB)
- Detail level
- Child nodes (if subdivided)
```

## Tech Stack

- **Language:** Python
- **Image Processing:** PIL (Python Imaging Library)

## Features

- Recursive quadtree-based compression algorithm
- Visual representation of compression progress (animated GIFs)
- Adjustable depth level and detail threshold parameters
- Generates both compressed output and visualization with quadrant boundaries

Built with @tanneryork
