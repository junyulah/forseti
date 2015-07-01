forseti
===================================
A new way to validate a json format data.

What is json format data validation?
-----------------------------------
Assume there are a json type data, I do not know it's correct or not. So I will validate this json.
For example, I get two json fomat data:
```
A:{
	"name": "ddchen",
	"age": 25
}
```
and 
```
B:{
	"name": "ddchen",
	"age": "25"
}
```
I got a rule that age must be a number, so json A is right and json B is wrong.
This kind of procedure is json format data validation, we make a judgment that which json is right and which is wrong.

In this example, A and B are source json. "age must be a number" is a validation rule. Validation rules is a group of validation rule. Because A satisfy validation rules, so A is right. B do not satisfy one of validation rules, so B is wrong.

How forseti deal with validation?
-----------------------------------
It looks like this:
```
var jsonValidator = require("forseti");

var sourceJson = {
	name: "ddchen",
	phone: "12345678",
	hobitts: [{
		type: "sleep",
		level: 5
	}],
	next: []
}

var m = jsonValidator.createMarker(true);
var sample = {
	name: m.um("ddchen"),
	hobitts: [{
		level: m.ian(5)
	}],
	next: m.iaa([])
}

var result = jsonValidator.validate(sourceJson, sample);

console.log(result);

// console show:
// {
//    pass: true
// }

```
Let's look at the code line by line
### import forseti
```
var jsonValidator = require("forseti");
```
Before import forseti, you need to install it.
```
npm install forseti
```
### get source json
```
var sourceJson = {
	name: "ddchen",
	phone: "12345678",
	hobitts: [{
		type: "sleep",
		level: 5
	}],
	next: []
}
```
This is a definition of source json. Actually, you can get source json from any business scenarios like ajax, server layer.
### create a maker
```
var m = jsonValidator.createMarker(true);
```
Maker is a special object which used to flag attribute. When you pass a "ture" to createMarker, you got a working maker.
We will see how to use maker in the next paragraph.
### define sample
```
var sample = {
	name: m.um("ddchen"),
	hobitts: [{
		level: m.ian(5)
	}],
	next: m.iaa([])
}
```
Sample is the key to define validation rules. Sample looks similar to sourceJson, but there are some Differences.
* miss some attributes
* there are some special sentances like 'name: m.um("ddchen")', 'level: m.ian(5)'.<br>


