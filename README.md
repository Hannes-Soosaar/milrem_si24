# Milrem Summer Intern IU trial work 

The following application is a simple UI made with Vue.js with Typescript the Leaflet map framework.
The application is on a single page with no logon or menus. The project is not meant to be deployed hence it is to be run under development mode.

## Navigating the rover
To start driving the rove switch on the engine from the Start Engine button from the top right operations area "Start Engine" button.
use the keyboard A, W, S, and D keys to navigate the rover on the map.
To switch off the engine press the "Stop Engine" button on the top right of the screen in the operations area. 

*It is advised to turn off the rover when there is an operational pause, to conserve batter battery *

## Map Navigation
* while on the map, you can use the right mouse button to grab and drag the map.
* the middle mouse button wheel to zoom in and out

**known issue**
1) The Waypoint Menu does not make the map inactive
2) Panning the map for over 0.5s will activate the Waypoint Menu
3) The rover icon does not show the movement direction

## Adding Waypoints
While on the map holding down the right mouse button for 0.5 seconds prompts the waypoint menu where you can choose:

1) Drive - fast-travel the rover to the location
2) Save - saves the waypoint to the list of way-points or
3) Discard -Cancel the operation by clicking

 **known issue** 
1) when selecting discard, the waypoint will not be removed until there a a new waypoint operation*

## The Waypoint list
The Waypoints List can be toggled from the rights side operations area by clicking "Show Waypoints/Hide Waypoints" 
this provides table view of all saved waypoints with the following options

1) Delete - removes the waypoint from the Waypoints List
2) Rename - Enter the desired new name in the text box and click "Rename" to rename the waypoint  
3) Drive  - fast-travels the rove to the location

**known issue**  
1) the fist adding of a waypoint creates two waypoints in the waypoints list
2) re-naming requires a new map action to take effect
3) the Waypoint menu and Waypoint list can be opened at the same time
4) the list pushes the map and operations area

## Project Setup

On the first run please user npm install do install all the dependencies

```sh
npm install
```

### Compile and Hot-Reload for Development

Compiling and running the website will ba available on  http://localhost:5173/

```sh
npm run dev
```

## Summary

The product is not ready for release further development is needed.

Please provide feed-back or send any new discovered bug reports or errors to hsoosaar@gmail.com 
