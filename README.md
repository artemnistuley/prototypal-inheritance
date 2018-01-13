# Prototypal Inheritance in JavaScript

### Example 1
```javascript
const person = {
    firstName: 'Vasya',
    lastName: 'Pupkin',
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};

const singer = {
    firstName: 'Thom',
    lastName: 'Yorke',
};

singer.__proto__ = person;

person.getFullName(); // Vasya Pupkin
singer.getFullName(); // Thom Yorke
```

### Example 2
```javascript
const person = {
    firstName: 'Vasya',
    lastName: 'Pupkin',
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};

const Singer = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.__proto__ = person;
};

let singer = new Singer('Thom', 'Yorke');

person.getFullName(); // Vasya Pupkin
singer.getFullName(); // Thom Yorke
```

### Example 3
```javascript
const person = {
    firstName: 'Vasya',
    lastName: 'Pupkin',
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};

const Singer = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
};

Singer.prototype = person;
Singer.prototype.constructor = Singer;

let singer = new Singer("Thom", "Yorke");

person.getFullName(); // Vasya Pupkin
singer.getFullName(); // Thom Yorke
```

### Example 4
```javascript
const Person = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
};

Person.prototype.getFullName = function() {
    return `${this.firstName} ${this.lastName}`;
};

Person.prototype.sayHi = function() {
    return `${this.firstName} ${this.lastName} said hi!`;
};

let person = new Person('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin
person.sayHi(); // Vasya Pupkin said hi!
```

### Example 5
```javascript
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

console.log( person instanceof Person ); // true
console.log( person.__proto__ === Person.prototype ); // true
console.log( Person.prototype.isPrototypeOf(person) ); // true
console.log( Object.getPrototypeOf(person) === Person.prototype ); // true
console.log( person.constructor === Person.prototype.constructor ); // true

let singer = new Singer('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead

console.log( singer instanceof Person ); // true
console.log( singer instanceof Singer ); // true
console.log( singer.__proto__ === Singer.prototype ); // true
console.log( singer.__proto__.__proto__ === Person.prototype ); // true
console.log( Singer.prototype.isPrototypeOf(singer) ); // true
console.log( Object.getPrototypeOf(singer) === Singer.prototype ); // true
console.log( singer.constructor === Singer.prototype.constructor ); // true
```

### Example 6
```javascript
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

### Example 7
```javascript
const Person = {
    constructor(firstName, lastName){
        let person = Object.create(Person.prototype);
        person.firstName = firstName;
        person.lastName = lastName;
        return person;
    },
    prototype: {
        getFullName() {
            return `${this.firstName} ${this.lastName}`;
        }
    }
};

const Singer = {
    constructor(firstName, lastName, bandName){
        let proto = Object.assign(Object.create(Person.prototype), Singer.prototype);
        let singer = Object.create(proto);
        singer.firstName = firstName;
        singer.lastName = lastName;
        singer.bandName = bandName;
        return singer;
    },
    prototype: {
        getBandName(){
          return this.bandName;
        }
    }
};

let person = Person.constructor('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin

let singer = Singer.constructor('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead
```

### Example 8
```javascript
const Person =  function(firstName, lastName){
    let person = Object.create(Person.prototype);
    person.firstName = firstName;
    person.lastName = lastName;
    return person;
}

Person.prototype.getFullName = function() {
    return `${this.firstName} ${this.lastName}`;
};

const Singer = function(firstName, lastName, bandName){
    let proto = Object.assign(Object.create(Person.prototype), Singer.prototype);
    let singer = Object.create(proto);
    singer.firstName = firstName;
    singer.lastName = lastName;
    singer.bandName = bandName;
    return singer;
}

Singer.prototype.getBandName = function() {
    return this.bandName;
}

let person = Person('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin

let singer = Singer('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead
```

### Example 9
```javascript
const Person = {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        return this;
    },
    create() {
        let person = Object.create(Person).constructor(...arguments);
        return person;
    },
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}; 

const Singer = {
    constructor(firstName, lastName, bandName) {
        Person.constructor.apply(this, arguments);
        this.bandName = bandName;
        return this;
    },
    create() {
        let proto = Object.assign(Object.create(Person), Singer);
        let singer = Object.create(proto).constructor(...arguments);
        return singer;
    },
    getBandName() {
        return this.bandName;
    }
}

let person = Person.create('Vasya', 'Pupkin');
person.getFullName(); // Vasya Pupkin

let singer = Singer.create('Thom', 'Yorke', 'Radiohead');
singer.getFullName(); // Thom Yorke
singer.getBandName(); // Radiohead
```

### Example 10
```javascript
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    set fullName(newFullName) {
        [this.firstName, this.lastName] = newFullName.split(' ');
    }

    sayHi() {
        return `${this.firstName} ${this.lastName} said hi!`;
    }

    static createDefaultPerson() {
        return new Person('Default', 'Person');
    }

    static get constantValue() {
        return "This is constant value!";
    }
}

class Singer extends Person {
    constructor(firstName, lastName, bandName) {
        super(...arguments);
        this.bandName = bandName;
    }

    sayHi() {
        return super.sayHi() + ' This is extended method!';
    }

    getBandName() {
        return this.bandName;
    }

    static createDefaultSinger() {
        return new Singer('Default', 'Singer', 'Unknown Band');
    }
}


let person = new Person('Vasya', 'Pupkin');
console.log(person.fullName); // Vasya Pupkin

person.fullName = 'John Doe';
console.log(person.fullName); // John Doe

person.sayHi(); // John Doe said hi!

let defaultPerson = Person.createDefaultPerson();
console.log(defaultPerson.fullName); // Default Person
console.log(Person.constantValue); // This is constant value!


let singer = new Singer('Thom', 'Yorke', 'Radiohead');
console.log(singer.fullName); // Thom Yorke

singer.sayHi();// Thom Yorke said hi! This is extended method!
singer.getBandName(); // Radiohead

let defaultSinger = Singer.createDefaultSinger();
console.log(defaultSinger.fullName); // Default Singer
console.log(Singer.constantValue); // This is constant value!
```
