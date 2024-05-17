## Stremoid_Assigments

## HTML Canavas Drawing Application

This project is a drawing application built using HTML, CSS, JavaScript, and the Fabric.js library. The application allows users to draw shapes on a canvas, fill them with color, move and scale shapes, group them together, and provides undo/redo functionality.

## Features

1. **Basic Drawing Features**
   - Get basic shapes such as rectangles, circles, and triangles on the canvas.
   - Fill the shapes with different colors using a color picker when you pick the color selected okay then click on chnage the color.

2. **Undo/Redo Functionality**
   - Undo and redo actions performed on the canvas.

3. **Move and Scale**
   - Move shapes around the canvas by Selecting them.
   - Resize shapes using Fabric.js's built-in scaling features.

4. **Grouping**
   - Group multiple shapes together to manipulate them simultaneously.

## Installation

1. Clone the repository:
    sh
    git clone https://github.com/Nggohel/Stremoid_Assigments.git
    cd Stremoid_Assigments
    

2. Open `index.html` in your web browser.

## Usage

- **Drawing Shapes:**
  - Click the "Rectangle", "Circle", or "Triangle" button.
  - Click on the canvas to place the selected shape.

- **Changing Color:**
  - Select a color using the color picker.
  - Select a shape on the canvas and click the "Change Color" button to apply the new color.

- **Undo/Redo:**
  - Click the "Undo" button to undo the last action.
  - Click the "Redo" button to redo the last undone action.

- **Grouping Shapes:**
  - Select multiple shapes by holding down the shift key and clicking on the shapes.
  - Click the "Group" button to group the selected shapes together.

## File Structure

- `index.html`: Contains the HTML structure of the application.
- `style.css`: Contains the CSS for styling the application.
- `script.js`: Contains the JavaScript code for the application's functionality.

## Dependencies

- [Fabric.js](https://cdnjs.cloudflare.com/ajax/libs/fabric.js/4.5.0/fabric.min.js): A powerful and simple JavaScript canvas library.

## Another Example code in Konva.js but it don't support undo/redo functionality 
```
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/konva@8.3.5/konva.min.js"></script>
  <meta charset="utf-8" />
  <title>Using Konva Canvas Features</title>
  <style>
    #toolbar {
      display: flex;
      margin-bottom: 10px;
    }
    #container {
      border: 1px solid #ccc;
      width: 800px;
      height: 600px;
      margin: auto;
    }
  </style>
</head>
<body>
  <div id="toolbar">
    <button onclick="drawShape('rectangle')">Rectangle</button>
    <button onclick="drawShape('circle')">Circle</button>
    <button onclick="drawShape('triangle')">Triangle</button>
    <input type="color" id="fillColor" value="#00D2FF" />
    <button onclick="undo()">Undo</button>
    <button onclick="redo()">Redo</button>
  </div>
  <div id="container"></div>
  <script>
    var stage = new Konva.Stage({
      container: "container",
      width: 800,
      height: 600,
    });

    var layer = new Konva.Layer();
    stage.add(layer);

    var shapes = [];
    var history = [];
    var historyStep = -1;

    function addToHistory(action) {
      history = history.slice(0, historyStep + 1);
      history.push(action);
      historyStep++;
    }

    function undo() {
      if (historyStep >= 0) {
        var action = history[historyStep];
        if (action.type === "add") {
          action.shape.remove();
        } else if (action.type === "remove") {
          layer.add(action.shape);
        } else if (action.type === "transform") {
          action.shape.attrs = action.oldAttrs;
          action.shape.getLayer().batchDraw();
        }
        historyStep--;
        layer.batchDraw();
      }
    }

    function redo() {
      if (historyStep < history.length - 1) {
        historyStep++;
        var action = history[historyStep];
        if (action.type === "add") {
          layer.add(action.shape);
        } else if (action.type === "remove") {
          action.shape.remove();
        } else if (action.type === "transform") {
          action.shape.attrs = action.newAttrs;
          action.shape.getLayer().batchDraw();
        }
        layer.batchDraw();
      }
    }

    function drawShape(type) {
      var fillColor = document.getElementById("fillColor").value;
      var shape;
      if (type === "rectangle") {
        shape = new Konva.Rect({
          x: Math.random() * stage.width(),
          y: Math.random() * stage.height(),
          width: 100,
          height: 50,
          fill: fillColor,
          stroke: "black",
          strokeWidth: 2,
          draggable: true,
        });
      } else if (type === "circle") {
        shape = new Konva.Circle({
          x: Math.random() * stage.width(),
          y: Math.random() * stage.height(),
          radius: 50,
          fill: fillColor,
          stroke: "black",
          strokeWidth: 2,
          draggable: true,
        });
      } else if (type === "triangle") {
        shape = new Konva.RegularPolygon({
          x: Math.random() * stage.width(),
          y: Math.random() * stage.height(),
          sides: 3,
          radius: 50,
          fill: fillColor,
          stroke: "black",
          strokeWidth: 2,
          draggable: true,
        });
      }

      shapes.push(shape);
      layer.add(shape);
      layer.draw();

      addToHistory({ type: "add", shape: shape });

      shape.on("transformend dragend", function () {
        addToHistory({ type: "transform", shape: shape, oldAttrs: shape._lastPos, newAttrs: shape.attrs });
      });

      shape.on("dblclick", function () {
        var group = new Konva.Group({
          draggable: true,
        });
        shapes.forEach(function (s) {
          if (s.isDragging()) {
            group.add(s);
          }
        });
        layer.add(group);
        group.draw();
      });
    }

    var tr = new Konva.Transformer();
    layer.add(tr);
    stage.on("click", function (e) {
      if (e.target === stage) {
        tr.nodes([]);
        layer.draw();
        return;
      }
      tr.nodes([e.target]);
      layer.draw();
    });

    layer.draw();
  </script>
</body>
</html>

```