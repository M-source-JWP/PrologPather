# PrologPather

PrologPather is a pathfinding application designed to help the user find the quickest path from point A to point B (As long as points A and B are on UBC campus), implemented entirely in Prolog. It works by comparing time taken values of individual sections of routes, and also features options such as including a scenic route, or stopping for a cafe/restaurant.

Programming by: Michael Pennefather

Project description by: Michael Pennefather, Phillipe Chapedelaine, Yash Malholtra

Database Entries by: Michael Pennefather, Phillipe Chapedelaine, Yash Malholtra 

## Our product is ready for action!

Post lockdown, many students are returning to UBC campus. For both new students and returning students, navigating around UBC campus efficiently can be a real challenge, with many unnecessarily long trips to get to classes. Therefore, we have created PrologPather, a pathfinding application designed to help the user find the quickest path from point A to point B (As long as points A and B are on UBC campus), implemented entirely in Prolog.

Our goal was to use Prolog’s strength in logical programming to recommend the shortest possible path through UBC campus, based on a knowledge base filled with UBC’s many pedestrian paths and their respective times. As in our proposal, we used the CLPFD/Constraint Logical Programming package to compare numerical time values of different routes and select the shortest ones. However, we also decided to use and traverse complex data structures by representing our map with a weighted, undirected graph in our project (explained in detail below).

Prolog allowed us to establish a detailed knowledge base by defining connections beforehand, making for easy comparisons to find the best path. Since there might be many paths to the same destination, it makes it much easier to have the results be based on logical criteria either built into the program or established by the user (shortest path, more scenery, public transport only, etc).

## New Learning / Changes From Our Original Proposal
In our original project proposal, we decided to use the CLPFD/Constraint Logical Programming package to compare numerical time values of different routes and select the shortest ones, in order to fulfill our "new element of the language" criteria. However, in our final project, we additionally chose to represent our map data with a undirected weighted graph. This new element was essential to our project because it was a more efficient way to represent a map of UBC, while still being fairly straightforward to implement.

Each "node" in our graph could be one of (1) a destination, (2) an intersection.

```
destination(x, "String represenatation of the full name of the destination").
intersection(y, "String represenatation of the full name of the intersection").
```

Each "edge" in our graph is a connection of two nodes, as well as a time taken to get from one node to the other.

```
edge(x, y, num).
```

Each destination would also be given at least one filter tag clause, which would be one of:
```
sports(x).
food(x).
library(y).
gym(x).
housing(x).
study_space(z).
lecture_hall(w).
scenic(x). 
parking(x).
museum(x)
medical(x)
```

For example, in order to represent [this segment](https://github.students.cs.ubc.ca/mpennefa/PrologProject/edit/master/Pictures/stadiumRd.PNG?raw=true) of Stadium Road, as well as St. John Hospice:

We defined St. John Hospice as
```
destination(sJohnHospice,"St. John Hospice").
    medical(sJohnHospice).
```

We defined each intersection as
```
intersection(sWMa_stadRd,"South West Marine Dr and Stadium Rd").
intersection(wMall_stadRd,"West Mall and Stadium Rd").
```

We then connected each node together using edge clauses.
```
edge(wMall_stadRd, sWMa_stadRd, 2).
edge(sJohnHospice, wMall_stadRd, 0.5).
```

We also decided to use the CLP package in order to compare the times of different paths to get to a destination. This made implementation very straightforward, as can be seen in our shortest path function below.

```
shortest_path(A,B,Path,Time) :-
    %Return the path from A to B who's time is =< all other path's times
    path_to(A,B,Path,Time),
    \+ (path_to(A,B,_,Time2), (Time > Time2)).
```

## Project Features

Our original MVP goals were:
- Given a starting location A and a destination location B, be able to return the shortest (in terms of time) fully connected path between A and B, and complete directions.  
  - The [goto/1](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L59) and [goto_dest/2](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L78) functions handle the user dual inputs and [printing the directions](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L34), while the [shortest_path/4](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L208) returns the shortest path between the points.
- Contains an extensive local database of all major locations on UBC campus and their appropriate directions and estimated travel times. 
  - The extensive database is located [here.](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L224)
- Allows users to print different locations from said database by filtering by a category: E.g. “show Libraries” returns IKB, Koerner, Woodward, etc...
We have fulfilled each one as follows: 
  - The user can [display options from any category and view categories](https://github.students.cs.ubc.ca/mpennefa/PrologProject/blob/404d2c0ec27055e62a72cfc6fc25489e4b7cb4b9/prolog/prologproject.pl#L116) at any time during the input process without losing previous input 

Given these goals, our project satisfies what we initially laid out in our MVP.

## How to use
To use the MVP - load the prologproject.pl file with the compiler of your choice, then enter the query "start." and follow the instructions on screen. 
 - You will first be prompted to select your starting point, or display destination options
 - Entering "options" at any time in the pathing process will display all the destination categories (e.g. libraries, lecture halls, food)
    - Entering any of the categories, **case sensitive**, will display all of the destinations in that category (e.g. "Food" will display The Nest, Kinton Ramen, Browns etc...)
    - Users do not have to enter "options", then a category in that order - any query can be made at any time
 - Once the user has selected their starting location, **case sensitive**, they will then be prompted to enter their end destination, display "options" again, or enter "back" to select a new starting location.
 - After the user has selected a starting and ending location, the program will find the shortest path between the two locations, print the path they need to take, and the time taken.

