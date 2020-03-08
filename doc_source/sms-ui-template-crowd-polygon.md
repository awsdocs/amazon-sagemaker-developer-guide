# crowd\-polygon<a name="sms-ui-template-crowd-polygon"></a>

A widget for drawing polygons on an image and assigning a label to the portion of the image that is enclosed in each polygon\.

### Attributes<a name="polygon-attributes"></a>

The following attributes are supported by this element\.

#### header<a name="polygon-attributes-header"></a>

The text to display above the image\. This is typically a question or simple instruction for the worker\.

#### labels<a name="polygon-attributes-labels"></a>

A JSON formatted array of strings, each of which is a label that a worker can assign to the image portion enclosed by a polygon\.

#### name<a name="polygon-attributes-name"></a>

The name of this widget\. It's used as a key for the widget's input in the form output\.

#### src<a name="polygon-attributes-src"></a>

The URL of the image on which to draw polygons\. 

#### initial\-value<a name="polygon-attributes-initialValue"></a>

An array of JSON objects, each of which defines a polygon to be drawn when the component is loaded\. Each JSON object in the array contains the following properties\.
+ **label** – The text assigned to the polygon as part of the labeling task\. This text must match one of the labels defined in the *labels* attribute of the <crowd\-polygon> element\.
+ **vertices** – An array of JSON objects\. Each object contains an x and y coordinate value for a point in the polygon\.

**Example**  
An `initial-value` attribute might look something like this\.  

```
initial-value = 
  '[
     {
     "label": "dog",
     "vertices": 
       [
         {
            "x": 570,
            "y": 239
         },
        ... 
         {
            "x": 759,
            "y": 281
         }
       ]
     }
  ]'
```
Because this will be within an HTML element, the JSON array must be enclosed in single or double quotes\. The example above uses single quotes to encapsulate the JSON and double quotes within the JSON itself\. If you must mix single and double quotes inside your JSON, replace them with their HTML entity codes \(`&quot;` for double quote, `&#39;` for single\) to safely escape them\.

### Element Hierarchy<a name="polygon-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [full\-instructions](#polygon-regions-full-instructions), [short\-instructions](#polygon-regions-short-instructions)

### Regions<a name="polygon-regions"></a>

The following regions are required\.

#### full\-instructions<a name="polygon-regions-full-instructions"></a>

General instructions about how to draw polygons\.

#### short\-instructions<a name="polygon-regions-short-instructions"></a>

Important task\-specific instructions that are displayed in a prominent place\.

### Output<a name="polygon-output"></a>

The following output is supported by this element\.

#### polygons<a name="polygon-output-boundingBoxes"></a>

An array of JSON objects, each of which describes a polygon that has been created by the worker\. Each JSON object in the array contains the following properties\.
+ **label** – The text assigned to the polygon as part of the labeling task\.
+ **vertices** – An array of JSON objects\. Each object contains an x and y coordinate value for a point in the polygon\. The top left corner of the image is 0,0\.

#### inputImageProperties<a name="polygon-output-inputImageProperties"></a>

A JSON object that specifies the dimensions of the image that is being annotated by the worker\. This object contains the following properties\.
+ **height** – The height, in pixels, of the image\.
+ **width** – The width, in pixels, of the image\.

**Example : Sample Element Outputs**  
The following are samples of outputs from common use scenarios for this element\.  
**Single Label, Single Polygon**  

```
{
    "annotatedResult": 
    {
      "inputImageProperties": {
        "height": 853,
        "width": 1280
      },
      "polygons": 
      [
        {
          "label": "dog",
          "vertices": 
          [
            {
              "x": 570,
              "y": 239
            },
            {
              "x": 603,
              "y": 513
            },
            {
              "x": 823,
              "y": 645
            },
            {
              "x": 901,
              "y": 417
            },
            {
              "x": 759,
              "y": 281
            }
          ]
        }
      ]
    }
  }
]
```
**Single Label, Multiple Polygons**  

```
[
  {
    "annotatedResult": {
      "inputImageProperties": {
        "height": 853,
        "width": 1280
      },
      "polygons": [
        {
          "label": "dog",
          "vertices": [
            {
              "x": 570,
              "y": 239
            },
            {
              "x": 603,
              "y": 513
            },
            {
              "x": 823,
              "y": 645
            },
            {
              "x": 901,
              "y": 417
            },
            {
              "x": 759,
              "y": 281
            }
          ]
        },
        {
          "label": "dog",
          "vertices": [
            {
              "x": 870,
              "y": 278
            },
            {
              "x": 908,
              "y": 446
            },
            {
              "x": 1009,
              "y": 602
            },
            {
              "x": 1116,
              "y": 519
            },
            {
              "x": 1174,
              "y": 498
            },
            {
              "x": 1227,
              "y": 479
            },
            {
              "x": 1179,
              "y": 405
            },
            {
              "x": 1179,
              "y": 337
            }
          ]
        }
      ]
    }
  }
]
```
**Multiple Labels, Multiple Polygons**  

```
[
  {
    "annotatedResult": {
      "inputImageProperties": {
        "height": 853,
        "width": 1280
      },
      "polygons": [
        {
          "label": "dog",
          "vertices": [
            {
              "x": 570,
              "y": 239
            },
            {
              "x": 603,
              "y": 513
            },
            {
              "x": 823,
              "y": 645
            },
            {
              "x": 901,
              "y": 417
            },
            {
              "x": 759,
              "y": 281
            }
          ]
        },
        {
          "label": "cat",
          "vertices": [
            {
              "x": 870,
              "y": 278
            },
            {
              "x": 908,
              "y": 446
            },
            {
              "x": 1009,
              "y": 602
            },
            {
              "x": 1116,
              "y": 519
            },
            {
              "x": 1174,
              "y": 498
            },
            {
              "x": 1227,
              "y": 479
            },
            {
              "x": 1179,
              "y": 405
            },
            {
              "x": 1179,
              "y": 337
            }
          ]
        }
      ]
    }
  }
]
```
You could have many labels available, but only the ones that are used appear in the output\.

### See Also<a name="polygon-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth for Labeling](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)