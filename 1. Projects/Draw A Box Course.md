---
date: 2025-02-11
tags:
  - project/draw-a-box
source: "[[Bookmarks#[Draw a box](https //drawabox.com)|Draw a Box Course]]"
---
Progress
```dataviewjs
// Get all tasks from the current file
const tasks = dv.current().file.tasks;
const total = tasks.length;
const completed = tasks.filter(task => task.completed).length;
const progressPercent = total ? Math.round((completed / total) * 100) : 0;

// Build the HTML for the progress bar
const progressBarHTML = `
<div style="width: 100%; background-color: #ddd; border-radius: 4px; overflow: hidden;">
  <div style="
      width: ${progressPercent}%;
      background-color: #4caf50;
      padding: 8px 0;
      text-align: center;
      color: white;
      font-weight: bold;
      transition: width 0.3s ease;
    ">
    ${progressPercent}%
  </div>
</div>
`;

// Render the progress bar in your note
dv.paragraph(progressBarHTML);
```
# Lesson 1: Lines, Ellipses and Boxes

## Lines 
Homework and exercises
- [x] 2 filled pages of the [Superimposed Lines](https://drawabox.com/lesson/1/superimposedlines) exercise
- [x] 1 filled page of the [Ghosted Lines](https://drawabox.com/lesson/1/ghostedlines) exercise
- [x] 2 filled pages of the [Ghosted Planes](https://drawabox.com/lesson/1/ghostedplanes) exercise

## Elipses
Homework and exercises
- [x] 2 filled pages of the [Tables of Ellipses](https://drawabox.com/lesson/1/tablesofellipses) exercise
- [x] 2 filled pages of the [Ellipses in Planes](https://drawabox.com/lesson/1/ellipsesinplanes) exercise
- [x] 1 filled page of the [Funnels](https://drawabox.com/lesson/1/funnels) exercise

## Boxes
Homework and exercises
- [ ] 1 filled page of the [Plotted Perspective](https://drawabox.com/lesson/1/plottedperspective) exercise. One page should contain three frames as shown in the exercise example.
- [ ] 2 filled pages of the [Rough Perspective](https://drawabox.com/lesson/1/roughperspective) exercise. Each page should contain three frames as shown in the exercise example.
- [ ] 1 filled page of the [Rotated Boxes](https://drawabox.com/lesson/1/rotatedboxes) exercise
- [ ] 2 filled pages of the [Organic Perspective](https://drawabox.com/lesson/1/organicperspective) exercise

# First steps
[[Drawing]]