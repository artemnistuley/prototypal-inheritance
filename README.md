# Prototypal Inheritance in JavaScript

### Example
```JavaScript
const Person = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
};

Person.prototype.getFullName = function() {
    return `${this.firstName} ${this.lastName}`;
};

const Singer = function(firstName, lastName, bandName) {
    Person.apply(this, arguments);
    this.bandName = bandName;
};

Singer.prototype = Object.create(Person.prototype);
Singer.prototype.constructor = Singer;
// or Object.setPrototypeOf(Singer.prototype, Person.prototype)

Singer.prototype.getBandName = function() {
    return this.bandName;
};


let person = new Person('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin

let person2 = new person.constructor('John','Doe');
person2.getFullName(); // John Doe

console.log(person instanceof Person); // true
console.log(person.__proto__ === Person.prototype); // true
console.log(Person.prototype.isPrototypeOf(person)); // true
console.log(Object.getPrototypeOf(person) === Person.prototype); // true
console.log(person.constructor === Person.prototype.constructor); // true

let singer = new Singer('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead

console.log(singer instanceof Person); // true
console.log(singer instanceof Singer); // true
console.log(singer.__proto__ === Singer.prototype); // true
console.log(singer.__proto__.__proto__ === Person.prototype); // true
console.log(Singer.prototype.isPrototypeOf(singer)); // true
console.log(Object.getPrototypeOf(singer) === Singer.prototype); // true
console.log(singer.constructor === Singer.prototype.constructor); // true
```

### Example
```JavaScript
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
