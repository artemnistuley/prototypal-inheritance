# Prototypal Inheritance in JavaScript

### Example
```
const Person = {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
		return this;
	},
	getFullName() {
		return `${this.firstName} ${this.lastName}`;
	}
}; 

const Singer = Object.create(Person);

Singer.constructor = function(firstName, lastName, bandName) {
	Person.constructor.apply(this, arguments);
	this.bandName = bandName;
	return this;
};

Singer.getBandName = function() {
	return this.bandName;
};


let person = Object.create(Person).constructor('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin

let singer = Object.create(Singer).constructor('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead
```
