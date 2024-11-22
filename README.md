# 🎨 pptxtojson
A JavaScript library that runs in the browser and converts .pptx files to readable JSON data.

> The main differences from other pptx file parsing tools are:
> 1. Runs directly in the browser;
> 2. The parsing result is **readable** JSON data, not just a direct translation of XML content into hard-to-understand JSON.

Online DEMO: https://pipipi-pikachu.github.io/pptxtojson/

# 🪧 Notes
### ⚒️ Use Cases
This repository was created for the project [PPTist](https://github.com/pipipi-pikachu/PPTist), aiming to provide a reference example for its "import .pptx file feature". However, as of now, there are still many differences in style between the parsed PPT information and the source file, making it not yet suitable for direct use in production environments.

But if you only need to extract text content and media resource information from PPT files, and do not have high requirements for layout accuracy/style information, pptxtojson might be helpful for you.

### 📏 Length Unit
In the output JSON, all length values are in `pt` (point).
> Note: In version 0.x, all output length values were in px (pixels).

# 🔨 Installation
```
npm install pptxtojson
```

# 💿 Usage
```html
<input type="file" accept="application/vnd.openxmlformats-officedocument.presentationml.presentation"/>
```

```js
import { parse } from 'pptxtojson'

document.querySelector('input').addEventListener('change', evt => {
	const file = evt.target.files[0]
	
	const reader = new FileReader()
	reader.onload = async e => {
		const json = await parse(e.target.result)
		console.log(json)
	}
	reader.readAsArrayBuffer(file)
})
```

```js
// Output Example
{
	"slides": {
		"fill": {
			"type": "color",
			"value": "#FF0000"
		},
		"elements": [
			{
				"left":	0,
				"top": 0,
				"width": 72,
				"height":	72,
				"borderColor": "#1f4e79",
				"borderWidth": 1,
				"borderType": "solid",
				"borderStrokeDasharray": 0,
				"fillColor": "#5b9bd5",
				"content": "<p style=\"text-align: center;\"><span style=\"font-size: 18pt;font-family: Calibri;\">TEST</span></p>",
				"isFlipV": false,
				"isFlipH": false,
				"rotate": 0,
				"vAlign": "mid",
				"name": "Rectangle 1",
				"type": "shape",
				"shapType": "rect"
			},
			// more...
		],
	},
	"size": {
		"width": 960,
		"height": 540
	}
}
```

# 📕 Feature Support

### Slide Size
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| width                  | number                         | Width            
| height                 | number                         | Height  

### Slide Background
| prop                   | type                            | Description            
|------------------------|---------------------------------|---------------
| type                   | 'color' 丨 'image' 丨 'gradient' | Background type            
| value                  | SlideColorFill 丨 SlideImageFill 丨 SlideGradientFill| Background value  

### Slide Elements
#### Text
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'text'                         | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| borderColor            | string                         | Border color          
| borderWidth            | number                         | Border width          
| borderType             | 'solid' 丨 'dashed' 丨 'dotted' | Border type          
| borderStrokeDasharray  | string                         | Non-solid border style       
| shadow                 | Shadow                         | Shadow            
| fillColor              | string                         | Fill color           
| content                | string                         | Text content (HTML rich text) 
| isFlipV                | boolean                        | Vertical flip          
| isFlipH                | boolean                        | Horizontal flip          
| rotate                 | number                         | Rotation angle          
| vAlign                 | string                         | Vertical alignment        
| isVertical             | boolean                        | Is vertical text        
| name                   | string                         | Element name  

#### Image
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'image'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| src                    | string                         | Image source (base64)    
| rotate                 | number                         | Rotation angle  

#### Shape
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'shape'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| borderColor            | string                         | Border color          
| borderWidth            | number                         | Border width          
| borderType             | 'solid' 丨 'dashed' 丨 'dotted' | Border type          
| borderStrokeDasharray  | string                         | Non-solid border style       
| shadow                 | Shadow                         | Shadow            
| fillColor              | string                         | Fill color           
| content                | string                         | Text content (HTML rich text) 
| isFlipV                | boolean                        | Vertical flip          
| isFlipH                | boolean                        | Horizontal flip          
| rotate                 | number                         | Rotation angle          
| shapType               | string                         | Shape type          
| vAlign                 | string                         | Vertical alignment        
| path                   | string                         | Path (only for custom shapes)         
| name                   | string                         | Element name   

#### Table
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'table'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height               
| borderColor            | string                         | Border color          
| borderWidth            | number                         | Border width          
| borderType             | 'solid' 丨 'dashed' 丨 'dotted' | Border type           
| data                   | TableCell[][]                  | Table data

#### Chart
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'chart'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| data                   | ChartItem[] 丨 ScatterChartData | Chart data    
| chartType              | ChartType                      | Chart type    
| barDir                 | 'bar' 丨 'col'                  | Bar chart direction    
| marker                 | boolean                        | With data markers    
| holeSize               | string                         | Doughnut chart size    
| grouping               | string                         | Grouping mode    
| style                  | string                         | Chart style 

#### Video
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'video'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| blob                   | string                         | Video blob    
| src                    | string                         | Video source 

#### Audio
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'audio'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| blob                   | string                         | Audio blob   

#### SmartArt
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'diagram'                      | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| elements               | (Shape 丨 Text)[]               | Child elements  

#### Grouped Elements
| prop                   | type                           | Description            
|------------------------|--------------------------------|---------------
| type                   | 'group'                        | Type            
| left                   | number                         | Horizontal position          
| top                    | number                         | Vertical position          
| width                  | number                         | Width            
| height                 | number                         | Height            
| elements               | Element[]                      | Child elements  

### For more types, please refer to 👇
[https://github.com/pipipi-pikachu/pptxtojson/blob/master/dist/index.d.ts](https://github.com/pipipi-pikachu/pptxtojson/blob/master/dist/index.d.ts)

# 🙏 Acknowledgements
This repository heavily references the implementations of [PPTX2HTML](https://github.com/g21589/PPTX2HTML) and [PPTXjs](https://github.com/meshesha/PPTXjs).
> Unlike them, PPTX2HTML and PPTXjs convert PPT files into runnable HTML pages, while pptxtojson converts PPT files into clean JSON data.

# 📄 License
MIT License | Copyright © 2020-PRESENT [pipipi-pikachu](https://github.com/pipipi-pikachu)