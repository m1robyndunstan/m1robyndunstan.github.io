---
layout: post
title:  "Virtual Escape Room"
date:   2022-09-30
author: Robyn
tags: escape-room christmas
published: true
---
In my research into escape rooms from the previous post, I also came across virtual escape rooms. Many of them are made by libraries, and almost all of them use Google Forms as a framework. My first thought was how neat of an idea this is. My second that was that I can definitely code something that will look better than Google Forms. (Sorry!) I decided to take the snowman escape room I described in the last post and turn it into a virtual experience.

# Design

Because it's what I use at work, I decided to build the website in Angular. (I can use the practice.) Other design considerations are: 

 - This is already on a computer, so answers can be typed in to be verified
 - There are no physical components, so puzzles that require flipping objects over or moving stuff around needs to be rethought to work on a computer screen
 - Future plans are to make multiple virtual escape rooms, so make as much code as possible reusable

# Development

To make it easy to add future virtual escape rooms, I started with a page that lets the user select which room to play. Because this is separate from the individual escape rooms, it is in a module by itself. 

For the actual escape room, I created two modules. One module is for code specific to this escape room, and the second module is for shared code. The shared module will contain components that can be easily separated from the main escape room code and generalized enough to be useful in other escape rooms.

## Build a Snowman

Building the snowman isn't required for the puzzles, but it is a fun bit of flavor. Also, I remember seeing many drag-and-drop dollmakers in the past. Building a drag-and-drop snowman maker sounded cool and would have the same kind of interaction as building the snowman out of paper pieces. I initially tried to use JQuery UI for the drag-and-drop functionality, but that doesn't interact well with Angular. After a lot of googling and banging my head against it, I came across someone's JQuery UI &quot;why isn't this working&quot; question where the answer was &quot;use Angular Material&quot;. 

Angular Material makes drag-and-drop easy! And, it can almost all be done in the HTML. 

1. Import `DragDropModule` into the component's parent module. This is the only typescript code needed.
1. Put property `cdkDrag` on draggable HTML tags.
1. Put attribute `cdkDragBoundary="#parentHtmlId"` on the draggable tag to limit where it can be dragged.

I put draggable on images of the different snowman pieces and the boundary as a parent div with a visible border to designate the build area. The user can drag the pieces anywhere inside the build area but can't lose them over the rest of the page. 

For this page, I also built my first shared component - an answer verifier, which is exactly what I didn't want to use for the physical puzzles. For the virtual version, type in an answer and check if it's correct is how most of the puzzles will be solved, so this is ideal functionality to make reusable. This component needs to:

 - Let the user submit an answer
 - Check the submitted answer against the correct response
 - Display clues if the submitted answer is wrong
 - Notify the parent component if the submitted answer is correct
 
The list of clues is input as an array, so it can have any length. I decided to use 2 clues for each puzzle. For each wrong answer, another clue is displayed. When all the clues are displayed, the last clue tells the user the answer, so they can continue with the game.

## Select the Magic Hat

Unfortunately, I couldn't quite figure out how to make a shared component that takes both the column class (from Bootstrap) and the list of image filenames as input, so this component is unigue to the snowman module. One difference between the physical puzzle and the virtual one is that I took free images of multiple types of hats instead of slightly modifying one image. I had to change the clues to match the new selection of hats. 

## Build a Fire

The second shared component is a picture puzzle. For the physical version, I cut the picture into strips, and verifying the answer was done by turning it over to show another picture also put together. This clearly won't work in the virtual version. 

When I was looking at Angular Material drag-and-drop for the snowman, I also noticed an example for changing the order of list items with some neat drag-and-drop effects. This is what I used as a basis for building the picture puzzle component. I cut the picture into horizontal strips and had each list item display one piece of the picture. Just add the drag-and-drop properties and copy the example CSS, and I have a working picture puzzle. 

For verifying the answer, there is no turning this picture over. And, dragging list items in the UI does not change the order of the items in the input array in typescript. So, I can either code something to update the typescript array when items are moved to use completing the picture as the answer, or I can change the way the answer is determined. I chose to modify the picture to have a word printed down a vertical side. The letters are sized so that there isn't more than one complete letter on a strip and some of the strip breaks go through the middle of a letter. That way, it's harder for the user to guess what the word is from looking at the mixed up pieces. When the pieces are in the correct order, the word is easy to read, and that word is what is entered for the answer. 

## Reward

Just having a paragraph of text to read didn't seem like enough of a reward for having solved all the puzzles. Inspired by some of the puzzle solving awards I've seen in the past (back when adopt-the-cute-graphic was a thing), I put together a quick little reward of my own. It's a picture of a snowman with Santa Claus and the text &quot;I saved Frosty the Snowman&quot;. 

# Deployment

Now, to share my nifty new website. For my previous game, it would run from just opening the built HTML file. Not so for this one. This site required being deployed to a server to work properly. I spent quite a while trying to find build settings that would make it work. Then, I discovered GitHub Pages... 

[Click here to play my virtual escape room](https://m1robyndunstan.github.io/VirtualEscapeRooms/)
