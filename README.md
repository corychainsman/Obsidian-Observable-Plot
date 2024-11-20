# An Obsidian Plugin for creating Observable Plots
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/L4L216D6FC)

Created by [Cory Chainsman](https://x.com/corychainsman)

This plugin allows you to create data visualizations in your Obsidian notes using [Observable Plot](https://observablehq.com/plot) and [D3.js](https://d3js.org/). 

## Features

- Create plots using Observable Plot's intuitive API
- Transform and manipulate data using D3.js
- Full JavaScript environment for data processing
- Customizable styling and layout options
- Responsive SVG output

## Installation

1. Open Obsidian Settings
2. Go to Community Plugins and disable Safe Mode
3. Click Browse and search for "Obsidian-Plot"
4. Install the plugin and enable it

## Basic Usage

Create a code block in your note with the language set to `plot`. The code block should contain a JSON object with:
- `data`: An array of data points
- `code` (optional): JavaScript code that returns an Observable plot specification

### Simple Plot

The simplest plot requires only data:

````
```plot
{
  "data": [
    {"x": 1, "y": 2},
    {"x": 2, "y": 4},
    {"x": 3, "y": 6}
  ]
}
```
````

This will create a scatter plot using default settings.

### Custom Plot

Add custom styling and configuration:

````
```plot
{
  "data": [
    {"x": 1, "y": 2},
    {"x": 2, "y": 4},
    {"x": 3, "y": 6}
  ],
  "code": "
    return {
      marks: [
        Plot.line(data, {x: 'x', y: 'y', stroke: 'blue', strokeWidth: 2}),
        Plot.dot(data, {x: 'x', y: 'y', fill: 'red', r: 5})
      ],
      grid: true,
      x: {label: 'X Axis'},
      y: {label: 'Y Axis'}
    };
  "
}
```
````

## Advanced Features

### Data Transformation with D3

Use D3 or normal javascript and return the Plot:

````
```plot
{
  "data": [
    {"group": "A", "values": [1, 2, 3, 4, 5, 6, 7]},
    {"group": "B", "values": [2, 4, 6, 8, 10, 12, 14]}
  ],
  "code": "
    // Calculate statistics for each group
    const stats = data.map(g => {
      const values = g.values;
      return {
        group: g.group,
        min: d3.min(values),
        q1: d3.quantile(values, 0.25),
        median: d3.median(values),
        q3: d3.quantile(values, 0.75),
        max: d3.max(values),
        mean: d3.mean(values)
      };
    });

    return {
      width: 400,
      height: 300,
      margin: 40,
      marks: [
        // Box plot
        Plot.ruleX(stats, {
          x1: 'min',
          x2: 'max',
          y: 'group'
        }),
        Plot.rectX(stats, {
          x1: 'q1',
          x2: 'q3',
          y: 'group',
          height: 20,
          fill: 'steelblue'
        }),
        Plot.tickX(stats, {
          x: 'median',
          y: 'group',
          stroke: 'white',
          strokeWidth: 2
        }),
        // Mean point
        Plot.dot(stats, {
          x: 'mean',
          y: 'group',
          fill: 'red',
          r: 4
        })
      ],
      y: {
        label: 'Group'
      },
      x: {
        grid: true,
        label: 'Value'
      }
    };
  "
}
```
````

### Custom Styling

Apply custom styles to your plot with D3:

````
```plot
{
  "data": [
    {"x": 1, "y": 2, "category": "A"},
    {"x": 2, "y": 4, "category": "B"},
    {"x": 3, "y": 6, "category": "A"}
  ],
  "code": "
    // Custom color scale
    const colorScale = d3.scaleOrdinal()
      .domain(['A', 'B'])
      .range(['#ff6b6b', '#4ecdc4']);

    return {
      width: 500,
      height: 300,
      margin: 40,
      style: {
        background: '#f8f9fa',
        color: '#212529',
        fontFamily: 'system-ui, sans-serif'
      },
      marks: [
        Plot.line(data, {
          x: 'x',
          y: 'y',
          stroke: d => colorScale(d.category),
          strokeWidth: 3
        }),
        Plot.dot(data, {
          x: 'x',
          y: 'y',
          fill: d => colorScale(d.category),
          r: 8
        })
      ],
      x: {
        grid: true,
        label: 'X Axis',
        labelOffset: 30
      },
      y: {
        grid: true,
        label: 'Y Axis',
        labelOffset: 30
      }
    };
  "
}
```
````

## API Reference

### Plot Configuration

The plot code block accepts a JSON object with the following properties:

- `data` (required): Array of data objects
- `code` (optional): JavaScript code that returns a plot specification

### Available Libraries

Your plot code has access to:

- `Plot`: The [Observable Plot](https://github.com/observablehq/plot) library
- `d3`: The [D3.js](https://d3js.org/) library
- `data`: Your input data array

### Plot Specification

The plot specification object can include all of the standard Observable Plot features

- `width`: Plot width in pixels
- `height`: Plot height in pixels
- `margin`: Margin size in pixels
- `style`: CSS styles for the plot
- `marks`: Array of Plot mark objects
- `x`: X-axis configuration
- `y`: Y-axis configuration
- `color`: Color scale configuration
- `grid`: Boolean or grid configuration object
- etc...

## Development

1. Clone this repository
2. Run `npm install`
3. Run `npm run dev` to start compilation in watch mode

### Building

- `npm run build` will create a production build

### Installing for Development

1. Copy `main.js`, `manifest.json`, and `styles.css` to your Obsidian plugins folder
2. Enable the plugin in Obsidian's settings

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Observable Plot](https://github.com/observablehq/plot)
- [D3.js](https://d3js.org/)
- [Obsidian](https://obsidian.md/)
