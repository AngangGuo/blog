---
title: "Notes   Tabulator Usage"
date: 2021-02-19T23:09:51-08:00
draft: true
---
Tabulator allows you to create interactive tables in seconds from any HTML Table, JavaScript Array,
AJAX data source or JSON formatted data.

### Editing
See [Tabulator Editing](http://tabulator.info/docs/4.9/edit)
* The `input` editor allows entering of a single line of **plain text**.
* The `textarea` editor allows entering of multiple lines of plain text
* The `number` editor allows for numeric entry with a number type input element with increment and decrement buttons.
* The `range` editor allows for numeric entry with a range type input element.
* The `tickCross` editor allows for boolean values using a **checkbox** type input element.
* The `select` editor creates a dropdown select box to allow the user to select from some predefined options passed into the values property of the editorParams option.

### How to get table data as JSON data?
Get all data including not shown out
```javascript
let data = table.getData();
let json = JSON.stringify(data)
```

To get visible data only, pass `true` as argument:
```javascript
let visibleData = table.getData(true);
```

### Ajax Data
You must at least setup the columns, otherwise the data will not show out
```javascript
var show_table = function(){
  var columns=[
        {title:"name", field:"name"},
        {title:"date", field:"date"},
    ];
  var table = new Tabulator("#table", {
    ajaxURL: "ajax.json",
    columns:columns
  });
}
```

### Title
It is important to note that the title parameter for each column definition must match the text in the table element's 
header cell exactly for the imported data to link to the correct column.
```javascript
var table = new Tabulator("#example-table", {
    tooltips:true, //example option (enable tooltips on all cells)
    columns:[ //set column definitions for imported table data
        {title:"Age", sorter:"number"},
        {title:"Height", sorter:"number"},
        {title:"Date of Birth", sorter:"date"},
    ],
});
```

### Load Data
JavaScript:
```javascript
async function getdata() {
    const response = await fetch("http://localhost:8080/json")
    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json()
    let table = new Tabulator("#example-table", {
        data: data,
        autoColumns: true,
    });
}
```

### Get table reference
Tabulator keeps track of all tables that it creates and you can use the `findTable` function on the Tabulator prototype 
to lookup the table object for any existing table using the element they were created on.

The `findTable` function will accept a valid CSS selector string or a DOM node for the table as its first argument.
```javascript
var table = Tabulator.prototype.findTable("#example-table")[0]; // find table object for table with id of example-table
```
The findTable function will return an array of matching tables. If no match is found it will return false

### Save
```javascript
async function savedata(){
    let data=window.myTable.getData()
    const response = await fetch('http://localhost:8080/save', {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    });

    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }

    const content = await response.json();
    console.log(content)
    return
}
```