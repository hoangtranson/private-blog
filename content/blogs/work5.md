+++
showonlyimage = false
draft = false
image = "img/portfolio/content5/js1.jpeg"
date = "2018-04-1T23:00:00+07:00"
title = "Clean Code Javascript"
weight = 2
+++

Software engineering principles, from Robert C. Martin's book Clean Code.
<!--more-->

### Table of content
- [Introduction](#introduction)
- [Summary and Integrate to PR](#summary)
- [Variables](#variables)
- [Functions](#functions)
- [Classes](#classes)
- [SOLID](#solid)
- [Testing](#testing)
- [Concurrency](#concurrency)
- [Error Handling](#errorhandling)
- [Formatting](#formatting)
- [Comments](#comments)

#### Introduction <a name="introduction"></a>

This topic based on [Clean code javascript](https://github.com/ryanmcdermott/clean-code-javascript) from Ryanmc Dermott. Thanks to his great work. I created my check list for javascript from this great resource and this help me a lot in reducing bugs and optimize my code. That's why I wanna share this to persons who is working as Front-end engineering like me.

#### Summary and Integrate to PR <a name="summary"></a>

Here is check list that I created by myself.

```Markdown
Variables
- [ ] Use meaningful and pronounceable variable names
- [ ] Use the same vocabulary for the same type of variable
- [ ] Use searchable names
- [ ] Avoid Mental Mapping
- [ ] Don't add unneeded context
- [ ] Use default arguments instead of short circuiting or conditionals

Functions
- [ ] Function arguments (2 or fewer ideally)
- [ ] Functions should do one thing
- [ ] Function names should say what they do
- [ ] Remove duplicate code
- [ ] Set default objects with Object.assign
- [ ] Don't use flags as function parameters
- [ ] Favor functional programming over imperative programming
- [ ] Encapsulate conditionals
- [ ] Avoid negative conditionals
- [ ] Avoid conditionals
- [ ] Avoid type-checking
- [ ] Remove dead code

Classes
- [ ] Prefer ES2015/ES6 classes over ES5 plain functions
- [ ] Use method chaining
- [ ] Prefer composition over inheritance

SOLID
- [ ] Single Responsibility Principle (SRP)
- [ ] Open/Closed Principle (OCP)
- [ ] Liskov Substitution Principle (LSP)
- [ ] Interface Segregation Principle (ISP)
- [ ] Dependency Inversion Principle (DIP)

Testing
- [ ] Single concept per test

Concurrency
- [ ] Use Promises, not callbacks

Error Handling
- [ ] Don't ignore caught errors
- [ ] Don't ignore rejected promises

Formatting
- [ ] Use consistent capitalization
- [ ] Function callers and callees should be close

Comments
- [ ] Only comment things that have business logic complexity.
- [ ] Don't leave commented out code in your codebase
- [ ] Don't have journal comments
- [ ] Avoid positional markers
```

![image 1][1]

![image 2][2]

So I use this as check list whenever I create pull request to important branchs.

References:

1. https://blog.github.com/2016-02-17-issue-and-pull-request-templates/
2. https://help.github.com/articles/creating-a-pull-request-template-for-your-repository/

#### Variables <a name="variables"></a>

1. Use meaningful and pronounceable variable names

    BAD CODE:
    ```Javascript
    const yyyymmdstr = moment().format('YYYY/MM/DD');
    ```

    GOOD CODE:
    ```Javascript
    const currentDate = moment().format('YYYY/MM/DD'); // change var name more meaning
    ```

2. Use the same vocabulary for the same type of variable

    BAD CODE:
    ```Javascript
    getUserInfo();
    getClientData();
    getCustomerRecord();
    ```

    GOOD CODE:
    ```Javascript
    getUser();
    ```
3. Use searchable names

    We will read more code than we will ever write. It's important that the code we do write is readable and searchable. By not naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable. Tools like [buddy.js](https://github.com/danielstjules/buddy.js) and [ESLint](https://github.com/eslint) can help identify unnamed constants.

    BAD CODE:
    ```Javascript
    setTimeout(blastOff, 86400000);
    ```

    GOOD CODE:
    ```Javascript
    const MILLISECONDS_IN_A_DAY = 86400000; // create MILLISECONDS_IN_A_DAY for 86400000
    setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
    ```

4. Avoid Mental Mapping
    Explicit is better than implicit.

    BAD CODE:
    ```Javascript
    const locations = ['Austin', 'New York', 'San Francisco'];
    locations.forEach((l) => {
        doStuff();
        doSomeOtherStuff();
        dispatch(l);
    });
    ```

    GOOD CODE:
    ```Javascript
    const locations = ['Austin', 'New York', 'San Francisco'];
    locations.forEach((location) => { // change l to location
        doStuff();
        doSomeOtherStuff();
        dispatch(location);
    });
    ```

5. Don't add unneeded context
    If your class/object name tells you something, don't repeat that in your variable name.

    BAD CODE:
    ```Javascript
    const Car = {
        carMake: 'Honda',
        carModel: 'Accord',
        carColor: 'Blue'
    };

    paintCar(car) {
        car.carColor = 'Red';
    }
    ```

    GOOD CODE:
    ```Javascript
    const Car = {
        make: 'Honda',
        model: 'Accord',
        color: 'Blue'
    };

    paintCar(car) {
        car.carColor = 'Red';
    }
    ```

6. Use default arguments instead of short circuiting or conditionals
    Default arguments are often cleaner than short circuiting. Be aware that if you use them, your function will only provide default values for undefined arguments. Other "falsy" values such as '', "", false, null, 0, and NaN, will not be replaced by a default value.

    BAD CODE:
    ```Javascript
    createMicrobrewery(name) {
        const breweryName = name || 'Hipster Brew Co.';
        ...
    }
    ```

    GOOD CODE:
    ```Javascript
    createMicrobrewery(name = 'Hipster Brew Co.') {
        ...
    }
    ```

#### Functions <a name="functions"></a>

1. Function arguments (2 or fewer ideally)

    Limiting the amount of function parameters is incredibly important because it makes testing your function easier. Having more than three leads to a combinatorial explosion where you have to test tons of different cases with each separate argument.

    One or two arguments is the ideal case, and three should be avoided if possible. Anything more than that should be consolidated. Usually, if you have more than two arguments then your function is trying to do too much. In cases where it's not, most of the time a higher-level object will suffice as an argument.

    Since JavaScript allows you to make objects on the fly, without a lot of class boilerplate, you can use an object if you are finding yourself needing a lot of arguments.

    To make it obvious what properties the function expects, you can use the ES2015/ES6 destructuring syntax. This has a few advantages:

    - When someone looks at the function signature, it's immediately clear what properties are being used.
    - Destructuring also clones the specified primitive values of the argument object passed into the function. This can help prevent side effects. Note: objects and arrays that are destructured from the argument object are NOT cloned.
    - Linters can warn you about unused properties, which would be impossible without destructuring.

    BAD CODE:
    ```Javascript
    createMenu(title, body, buttonText, cancellable) {
    }
    ```

    GOOD CODE:
    ```Javascript
    createMenu({ title, body, buttonText, cancellable }) {
    }

    createMenu({
        title: 'Foo',
        body: 'Bar',
        buttonText: 'Baz',
        cancellable: true
    });
    ```

2. Functions should do one thing

    This is by far the most important rule in software engineering. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, they can be refactored easily and your code will read much cleaner. If you take nothing else away from this guide other than this, you'll be ahead of many developers.

    BAD CODE:
    ```Javascript
    emailClients(clients) {
        clients.forEach((client) => {
            const clientRecord = database.lookup(client);
            if (clientRecord.isActive()) {
                email(client);
            }
        });
    }
    ```

    GOOD CODE:
    ```Javascript
    emailActiveClients(clients) {
        clients
            .filter(isActiveClient)
            .forEach(email);
        }

    isActiveClient(client) {
        const clientRecord = database.lookup(client);
        return clientRecord.isActive();
    }
    ```

3. Function names should say what they do

    It's hard to tell from the function name what is added

    BAD CODE:
    ```Javascript
    addToDate(date, month) {}
    const date = new Date();
    addToDate(date, 1);
    ```

    GOOD CODE:
    ```Javascript
    addMonthToDate(month, date) {}
    const date = new Date();
    addMonthToDate(1, date)
    ```

4. Remove duplicate code
    Do your absolute best to avoid duplicate code. Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

    Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc. If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them. If you only have one list, there's only one place to update!

    Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.

    Getting the abstraction right is critical, that's why you should follow the SOLID principles laid out in the Classes section. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction,do it! Don't repeat yourself, otherwise you'll find yourself updating multiple places anytime you want to change one thing.

    BAD CODE:
    ```Javascript
    showDeveloperList(developers) {
        developers.forEach((developer) => {
            const expectedSalary = developer.calculateExpectedSalary();
            const experience = developer.getExperience();
            const githubLink = developer.getGithubLink();
            const data = {
            expectedSalary,
            experience,
            githubLink
            };

            render(data);
        });
    }

    showManagerList(managers) {
        managers.forEach((manager) => {
            const expectedSalary = manager.calculateExpectedSalary();
            const experience = manager.getExperience();
            const portfolio = manager.getMBAProjects();
            const data = {
            expectedSalary,
            experience,
            portfolio
            };

            render(data);
        });
    }
    ```

    GOOD CODE:
    ```Javascript
    showEmployeeList(employees) {
        employees.forEach((employee) => {
            const expectedSalary = employee.calculateExpectedSalary();
            const experience = employee.getExperience();

            const data = {
            expectedSalary,
            experience
            };

            switch (employee.type) {
            case 'manager':
                data.portfolio = employee.getMBAProjects();
                break;
            case 'developer':
                data.githubLink = employee.getGithubLink();
                break;
            }

            render(data);
        });
    }
    ```

5. Set default objects with Object.assign
    BAD CODE:
    ```Javascript
    const menuConfig = {
        title: null,
        body: 'Bar',
        buttonText: null,
        cancellable: true
    };

    createMenu(config) {
        config.title = config.title || 'Foo';
        config.body = config.body || 'Bar';
        config.buttonText = config.buttonText || 'Baz';
        config.cancellable = config.cancellable !== undefined ? config.cancellable : true;
    }

    createMenu(menuConfig);
    ```

    GOOD CODE:
    ```Javascript
    const menuConfig = {
        title: 'Order',
        buttonText: 'Send',
        cancellable: true
    };

    createMenu(config) {
        config = Object.assign({
            title: 'Foo',
            body: 'Bar',
            buttonText: 'Baz',
            cancellable: true
        }, config);
    }

    createMenu(menuConfig);
    ```
6. Don't use flags as function parameters

    Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

    BAD CODE:
    ```Javascript
    createFile(name, temp) {
        if (temp) {
            fs.create(`./temp/${name}`);
        } else {
            fs.create(name);
        }
    }
    ```

    GOOD CODE:
    ```Javascript
    createFile(name) {
        fs.create(name);
    }

    createTempFile(name) {
        createFile(`./temp/${name}`);
    }
    ```

7. Favor functional programming over imperative programming

    JavaScript isn't a functional language in the way that Haskell is, but it has a functional flavor to it. Functional languages can be cleaner and easier to test. Favor this style of programming when you can.

    BAD CODE:
    ```Javascript
    const programmerOutput = [
        {
            name: 'Uncle Bobby',
            linesOfCode: 500
        }, {
            name: 'Suzie Q',
            linesOfCode: 1500
        }, {
            name: 'Jimmy Gosling',
            linesOfCode: 150
        }, {
            name: 'Gracie Hopper',
            linesOfCode: 1000
        }
    ];

    let totalOutput = 0;

    for (let i = 0; i < programmerOutput.length; i++) {
        totalOutput += programmerOutput[i].linesOfCode;
    }
    ```

    GOOD CODE:
    ```Javascript
    const programmerOutput = [
        {
            name: 'Uncle Bobby',
            linesOfCode: 500
        }, {
            name: 'Suzie Q',
            linesOfCode: 1500
        }, {
            name: 'Jimmy Gosling',
            linesOfCode: 150
        }, {
            name: 'Gracie Hopper',
            linesOfCode: 1000
        }
    ];

    const totalOutput = programmerOutput
    .map(output => output.linesOfCode)
    .reduce((totalLines, lines) => totalLines + lines);
    ```
8. Encapsulate conditionals
    BAD CODE:
    ```Javascript
    if (fsm.state === 'fetching' && isEmpty(listNode)) {}
    ```

    GOOD CODE:
    ```Javascript
    shouldShowSpinner(fsm, listNode) {
        return fsm.state === 'fetching' && isEmpty(listNode);
    }

    if (shouldShowSpinner(fsmInstance, listNodeInstance)) {}
    ```
9. Avoid negative conditionals
    BAD CODE:
    ```Javascript
    isDOMNodeNotPresent(node) {}
    if (!isDOMNodeNotPresent(node)) {}
    ```

    GOOD CODE:
    ```Javascript
    isDOMNodePresent(node) {}

    if (isDOMNodePresent(node)) {}
    ```
10. Avoid conditionals

    This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an if statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do one thing. When you have classes and functions that have if statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

    BAD CODE:
    ```Javascript
    class Airplane {
        getCruisingAltitude() {
            switch (this.type) {
                case '777':
                    return this.getMaxAltitude() - this.getPassengerCount();
                case 'Air Force One':
                    return this.getMaxAltitude();
                case 'Cessna':
                    return this.getMaxAltitude() - this.getFuelExpenditure();
            }
        }
    }
    ```

    There are multiple issues with switch, from its procedural control flow to its non-standard-looking way it handles code blocks, the rest of JavaScript uses curly braces yet switch does not. Syntactically, it’s not one of JavaScript’s best, nor is its design. We’re forced to manually add break; statements within each case, which can lead to difficult debugging and nested errors further down the case should we forget! Douglas Crockford has written and spoken about it numerous times, his recommendations are to treat it with caution.

    We often use Object lookups for things in JavaScript, often for things we would never contemplate using switch for - so why not use an Object literal to replace switch? Objects are much more flexible, have better readability and maintainability and we don’t need to manually break; each “case”. They’re a lot friendlier on new JavaScript developers as well, as they’re standard Objects.

    As the number of “cases” increases, the performance of the object (hash table) gets better than the average cost of the switch (the order of the cases matter). The object approach is a hash table lookup, and the switch has to evaluate each case until it hits a match and a break.

    GOOD CODE:
    ```Javascript
    class Airplane {
        getCruisingAltitude (type) {
            const CruisingAltitude = {
                '777': () => {
                    return this.getMaxAltitude() - this.getPassengerCount();
                },
                'Air Force One': () => {
                    return this.getMaxAltitude();
                },
                'Cessna': () => {
                    return this.getMaxAltitude() - this.getFuelExpenditure();
                }
            };
            return CruisingAltitude[type]();
        }
    }
    ```
11. Avoid type-checking

    JavaScript is untyped, which means your functions can take any type of argument. Sometimes you are bitten by this freedom and it becomes tempting to do type-checking in your functions. There are many ways to avoid having to do this. The first thing to consider is consistent APIs.

    BAD CODE:
    ```Javascript
    travelToTexas(vehicle) {
        if (vehicle instanceof Bicycle) {
            vehicle.pedal(this.currentLocation, new Location('texas'));
        } else if (vehicle instanceof Car) {
            vehicle.drive(this.currentLocation, new Location('texas'));
        }
    }
    ```

    GOOD CODE:
    ```Javascript
    travelToTexas(vehicle) {
        vehicle.move(this.currentLocation, new Location('texas'));
    }
    ```

    If you are working with basic primitive values like strings and integers, and you can't use polymorphism but you still feel the need to type-check, you should consider using TypeScript. It is an excellent alternative to normal JavaScript, as it provides you with static typing on top of standard JavaScript syntax. The problem with manually type-checking normal JavaScript is that doing it well requires so much extra verbiage that the faux "type-safety" you get doesn't make up for the lost readability. Keep your JavaScript clean, write good tests, and have good code reviews. Otherwise, do all of that but with TypeScript (which, like I said, is a great alternative!).

    BAD CODE:
    ```Javascript
    combine(val1, val2) {
        if (typeof val1 === 'number' && typeof val2 === 'number' ||
            typeof val1 === 'string' && typeof val2 === 'string') {
            return val1 + val2;
        }
        throw new Error('Must be of type String or Number');
    }
    ```

    GOOD CODE:
    ```Javascript
    combine(val1, val2) {
        return val1 + val2;
    }
    ```

12. Remove dead code

    Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase. If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

    BAD CODE:
    ```Javascript
    oldRequestModule(url) {}
    newRequestModule(url) {}

    const req = newRequestModule;
    inventoryTracker('apples', req, 'www.inventory-awesome.io');
    ```

    GOOD CODE:
    ```Javascript
    newRequestModule(url) {}

    const req = newRequestModule;
    inventoryTracker('apples', req, 'www.inventory-awesome.io');
    ```

#### Classes <a name="classes"></a>

1. Prefer ES2015/ES6 classes over ES5 plain functions

    It's very difficult to get readable class inheritance, construction, and method definitions for classical ES5 classes. If you need inheritance (and be aware that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over classes until you find yourself needing larger and more complex objects.

    BAD CODE:
    ```Javascript
    const Animal = function(age) {
        if (!(this instanceof Animal)) {
            throw new Error('Instantiate Animal with `new`');
        }

        this.age = age;
    };

    Animal.prototype.move = function move() {};

    const Mammal = function(age, furColor) {
        if (!(this instanceof Mammal)) {
            throw new Error('Instantiate Mammal with `new`');
        }

        Animal.call(this, age);
        this.furColor = furColor;
    };

    Mammal.prototype = Object.create(Animal.prototype);
    Mammal.prototype.constructor = Mammal;
    Mammal.prototype.liveBirth = function liveBirth() {};

    const Human = function(age, furColor, languageSpoken) {
    if (!(this instanceof Human)) {
        throw new Error('Instantiate Human with `new`');
    }

    Mammal.call(this, age, furColor);
        this.languageSpoken = languageSpoken;
    };

    Human.prototype = Object.create(Mammal.prototype);
    Human.prototype.constructor = Human;
    Human.prototype.speak = function speak() {};
    ```

    GOOD CODE:
    ```Javascript
    class Animal {
        constructor(age) {
            this.age = age;
        }

        move() { /* ... */ }
    }

    class Mammal extends Animal {
        constructor(age, furColor) {
            super(age);
            this.furColor = furColor;
        }

        liveBirth() { /* ... */ }
    }

    class Human extends Mammal {
        constructor(age, furColor, languageSpoken) {
            super(age, furColor);
            this.languageSpoken = languageSpoken;
        }

        speak() { /* ... */ }
    }
    ```
2. Use method chaining

    This pattern is very useful in JavaScript and you see it in many libraries such as jQuery and Lodash. It allows your code to be expressive, and less verbose. For that reason, I say, use method chaining and take a look at how clean your code will be. In your class functions, simply return this at the end of every function, and you can chain further class methods onto it.

    BAD CODE:
    ```Javascript
    class Car {
        constructor(make, model, color) {
            this.make = make;
            this.model = model;
            this.color = color;
        }

        setMake(make) {
            this.make = make;
        }

        setModel(model) {
            this.model = model;
        }

        setColor(color) {
            this.color = color;
        }

        save() {
            console.log(this.make, this.model, this.color);
        }
    }

    const car = new Car('Ford','F-150','red');
    car.setColor('pink');
    car.save();
    ```

    GOOD CODE:
    ```Javascript
    class Car {
        constructor(make, model, color) {
            this.make = make;
            this.model = model;
            this.color = color;
        }

        setMake(make) {
            this.make = make;
            // NOTE: Returning this for chaining
            return this;
        }

        setModel(model) {
            this.model = model;
            // NOTE: Returning this for chaining
            return this;
        }

        setColor(color) {
            this.color = color;
            // NOTE: Returning this for chaining
            return this;
        }

        save() {
            console.log(this.make, this.model, this.color);
            // NOTE: Returning this for chaining
            return this;
        }
    }

    const car = new Car('Ford','F-150','red')
    .setColor('pink')
    .save();
    ```
3. Prefer composition over inheritance

    As stated famously in Design Patterns by the Gang of Four, you should prefer composition over inheritance where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition. The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

    You might be wondering then, "when should I use inheritance?" It depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

    - Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).
    - You can reuse code from the base classes (Humans can move like all animals).
    - You want to make global changes to derived classes by changing a base class. (Change the caloric expenditure of all animals when they move).

    BAD CODE:
    ```Javascript
    class Employee {
        constructor(name, email) {
            this.name = name;
            this.email = email;
        }
    }

    // Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee

    class EmployeeTaxData extends Employee {
        constructor(ssn, salary) {
            super();
            this.ssn = ssn;
            this.salary = salary;
        }
    }
    ```

    GOOD CODE:
    ```Javascript
    class EmployeeTaxData {
        constructor(ssn, salary) {
            this.ssn = ssn;
            this.salary = salary;
        }
    }

    class Employee {
        constructor(name, email) {
            this.name = name;
            this.email = email;
        }

        setTaxData(ssn, salary) {
            this.taxData = new EmployeeTaxData(ssn, salary);
        }
    }
    ```

#### SOLID <a name="solid"></a>

1. Single Responsibility Principle (SRP)
    As stated in Clean Code, "There should never be more than one reason for a class to change". It's tempting to jam-pack a class with a lot of functionality, like when you can only take one suitcase on your flight. The issue with this is that your class won't be conceptually cohesive and it will give it many reasons to change. Minimizing the amount of times you need to change a class is important. It's important because if too much functionality is in one class and you modify a piece of it, it can be difficult to understand how that will affect other dependent modules in your codebase.

    BAD CODE:
    ```Javascript
    class UserSettings {
        constructor(user) {
            this.user = user;
        }

        changeSettings(settings) {
            if (this.verifyCredentials()) {}
        }

        verifyCredentials() {}
    }
    ```

    GOOD CODE:
    ```Javascript
    class UserAuth {
        constructor(user) {
            this.user = user;
        }

        verifyCredentials() {}
    }

    class UserSettings {
        constructor(user) {
            this.user = user;
            this.auth = new UserAuth(user);
        }

        changeSettings(settings) {
            if (this.auth.verifyCredentials()) {}
        }
    }
    ```

2. Open/Closed Principle (OCP)

    As stated by Bertrand Meyer, "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." What does that mean though? This principle basically states that you should allow users to add new functionalities without changing existing code.

    BAD CODE:
    ```Javascript
    class AjaxAdapter extends Adapter {
        constructor() {
            super();
            this.name = 'ajaxAdapter';
        }
    }

    class NodeAdapter extends Adapter {
        constructor() {
            super();
            this.name = 'nodeAdapter';
        }
    }

    class HttpRequester {
        constructor(adapter) {
            this.adapter = adapter;
        }

        fetch(url) {
            if (this.adapter.name === 'ajaxAdapter') {
                return makeAjaxCall(url).then((response) => {

                    // transform response and return

                });
            } else if (this.adapter.name === 'httpNodeAdapter') {
                return makeHttpCall(url).then((response) => {

                    // transform response and return

                });
            }
        }
    }

    function makeAjaxCall(url) {

        // request and return promise

    }

    function makeHttpCall(url) {

        // request and return promise

    }
    ```

    GOOD CODE:
    ```Javascript
    class AjaxAdapter extends Adapter {
        constructor() {
            super();
            this.name = 'ajaxAdapter';
        }

        request(url) {

            // request and return promise

        }
    }

    class NodeAdapter extends Adapter {
        constructor() {
            super();
            this.name = 'nodeAdapter';
        }

        request(url) {

            // request and return promise

        }
    }

    class HttpRequester {
        constructor(adapter) {
            this.adapter = adapter;
        }

        fetch(url) {
            return this.adapter.request(url).then((response) => {

                // transform response and return

            });
        }
    }
    ```
3. Liskov Substitution Principle (LSP)

    This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.

    The best explanation for this is if you have a parent class and a child class, then the base class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

    BAD CODE:
    ```Javascript
    class Rectangle {
        constructor() {
            this.width = 0;
            this.height = 0;
        }

        setColor(color) {
        }

        render(area) {
        }

        setWidth(width) {
            this.width = width;
        }

        setHeight(height) {
            this.height = height;
        }

        getArea() {
            return this.width * this.height;
        }
    }

    class Square extends Rectangle {
        setWidth(width) {
            this.width = width;
            this.height = width;
        }

        setHeight(height) {
            this.width = height;
            this.height = height;
        }
    }

    renderLargeRectangles(rectangles) {
        rectangles.forEach((rectangle) => {
            rectangle.setWidth(4);
            rectangle.setHeight(5);
            const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.

            rectangle.render(area);

        });
    }

    const rectangles = [new Rectangle(), new Rectangle(), new Square()];
    renderLargeRectangles(rectangles);
    ```

    GOOD CODE:
    ```Javascript
    class Shape {
        setColor(color) {
        }

        render(area) {
        }
    }

    class Rectangle extends Shape {
        constructor(width, height) {
            super();
            this.width = width;
            this.height = height;
        }

        getArea() {
            return this.width * this.height;
        }
    }

    class Square extends Shape {
        constructor(length) {
            super();
            this.length = length;
        }

        getArea() {
            return this.length * this.length;
        }
    }

    renderLargeShapes(shapes) {
        shapes.forEach((shape) => {
            const area = shape.getArea();
            shape.render(area);
        });
    }

    const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
    renderLargeShapes(shapes);
    ```
4. Interface Segregation Principle (ISP)

    JavaScript doesn't have interfaces so this principle doesn't apply as strictly as others. However, it's important and relevant even with JavaScript's lack of type system.

    ISP states that "Clients should not be forced to depend upon interfaces that they do not use." Interfaces are implicit contracts in JavaScript because of duck typing.

    A good example to look at that demonstrates this principle in JavaScript is for classes that require large settings objects. Not requiring clients to setup huge amounts of options is beneficial, because most of the time they won't need all of the settings. Making them optional helps prevent having a "fat interface".

    BAD CODE:
    ```Javascript
    class DOMTraverser {
        constructor(settings) {
            this.settings = settings;
            this.setup();
        }

        setup() {
            this.rootNode = this.settings.rootNode;
            this.animationModule.setup();
        }

        traverse() {
        }
    }

    const $ = new DOMTraverser({
        rootNode: document.getElementsByTagName('body'),
        animationModule() {} // Most of the time, we won't need to animate when traversing.

    });
    ```

    GOOD CODE:
    ```Javascript
    class DOMTraverser {
        constructor(settings) {
            this.settings = settings;
            this.options = settings.options;
            this.setup();
        }

        setup() {
            this.rootNode = this.settings.rootNode;
            this.setupOptions();
        }

        setupOptions() {
            if (this.options.animationModule) {
            }
        }

        traverse() {
        }
    }

    const $ = new DOMTraverser({
        rootNode: document.getElementsByTagName('body'),
        options: {
            animationModule() {}
        }
    });
    ```
5. Dependency Inversion Principle (DIP)

    This principle states two essential things:
    - High-level modules should not depend on low-level modules. Both should depend on abstractions.
    - Abstractions should not depend upon details. Details should depend on abstractions.

    This can be hard to understand at first, but if you've worked with AngularJS, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.

    As stated previously, JavaScript doesn't have interfaces so the abstractions that are depended upon are implicit contracts. That is to say, the methods and properties that an object/class exposes to another object/class. In the example below, the implicit contract is that any Request module for an InventoryTracker will have a requestItems method.

    BAD CODE:
    ```Javascript
    class InventoryRequester {
        constructor() {
            this.REQ_METHODS = ['HTTP'];
        }

        requestItem(item) {}
    }

    class InventoryTracker {
        constructor(items) {
            this.items = items;

            // BAD: We have created a dependency on a specific request implementation.
            // We should just have requestItems depend on a request method: `request`

            this.requester = new InventoryRequester();
        }

        requestItems() {
            this.items.forEach((item) => {
            this.requester.requestItem(item);
            });
        }
    }

    const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
    inventoryTracker.requestItems();
    ```

    GOOD CODE:
    ```Javascript
    class InventoryTracker {
        constructor(items, requester) {
            this.items = items;
            this.requester = requester;
        }

        requestItems() {
            this.items.forEach((item) => {
            this.requester.requestItem(item);
            });
        }
    }

    class InventoryRequesterV1 {
        constructor() {
            this.REQ_METHODS = ['HTTP'];
        }

        requestItem(item) {}
    }

    class InventoryRequesterV2 {
        constructor() {
            this.REQ_METHODS = ['WS'];
        }

        requestItem(item) {}
    }

    // By constructing our dependencies externally and injecting them, we can easily
    // substitute our request module for a fancy new one that uses WebSockets.

    const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
    inventoryTracker.requestItems();
    ```

#### Testing <a name="testing"></a>

1. Single concept per test

    BAD CODE:
    ```Javascript
    import assert from 'assert';

    describe('MakeMomentJSGreatAgain', () => {
        it('handles date boundaries', () => {
            let date;

            date = new MakeMomentJSGreatAgain('1/1/2015');
            date.addDays(30);
            assert.equal('1/31/2015', date);

            date = new MakeMomentJSGreatAgain('2/1/2016');
            date.addDays(28);
            assert.equal('02/29/2016', date);

            date = new MakeMomentJSGreatAgain('2/1/2015');
            date.addDays(28);
            assert.equal('03/01/2015', date);
        });
    });
    ```

    GOOD CODE:
    ```Javascript
    import assert from 'assert';

    describe('MakeMomentJSGreatAgain', () => {
        it('handles 30-day months', () => {
            const date = new MakeMomentJSGreatAgain('1/1/2015');
            date.addDays(30);
            assert.equal('1/31/2015', date);
        });

        it('handles leap year', () => {
            const date = new MakeMomentJSGreatAgain('2/1/2016');
            date.addDays(28);
            assert.equal('02/29/2016', date);
        });

        it('handles non-leap year', () => {
            const date = new MakeMomentJSGreatAgain('2/1/2015');
            date.addDays(28);
            assert.equal('03/01/2015', date);
        });
    });
    ```

#### Concurrency <a name="concurrency"></a>

1. Use Promises, not callbacks

    Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6, Promises are a built-in global type. Use them!

    BAD CODE:
    ```Javascript
    import { get } from 'request';
    import { writeFile } from 'fs';

    get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
        if (requestErr) {
            console.error(requestErr);
        } else {
            writeFile('article.html', response.body, (writeErr) => {
            if (writeErr) {
                console.error(writeErr);
            } else {
                console.log('File written');
            }
            });
        }
    });
    ```

    GOOD CODE:
    ```Javascript
    import { get } from 'request';
    import { writeFile } from 'fs';

    get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
    .then((response) => {
        return writeFile('article.html', response);
    })
    .then(() => {
        console.log('File written');
    })
    .catch((err) => {
        console.error(err);
    });
    ```

#### Error Handling <a name="errorhandling"></a>

1. Don't ignore caught errors

    Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (console.log) isn't much better as often times it can get lost in a sea of things printed to the console. If you wrap any bit of code in a try/catch it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

    BAD CODE:
    ```Javascript
    try {
        functionThatMightThrow();
    } catch (error) {
        console.log(error);
    }
    ```

    GOOD CODE:
    ```Javascript
    try {
        functionThatMightThrow();
    } catch (error) {
        // One option (more noisy than console.log):

        console.error(error);
        // Another option:

        notifyUserOfError(error);
        // Another option:

        reportErrorToService(error);
        // OR do all three!

    }
    ```
2. Don't ignore rejected promises

    For the same reason you shouldn't ignore caught errors from try/catch.

    BAD CODE:
    ```Javascript
    getdata()
    .then((data) => {
        functionThatMightThrow(data);
    })
    .catch((error) => {
        console.log(error);
    });
    ```

    GOOD CODE:
    ```Javascript
    getdata()
    .then((data) => {
        functionThatMightThrow(data);
    })
    .catch((error) => {
        // One option (more noisy than console.log):

        console.error(error);
        // Another option:

        notifyUserOfError(error);
        // Another option:

        reportErrorToService(error);
        // OR do all three!

    });
    ```

#### Formatting <a name="formatting"></a>

1. Use consistent capitalization

    JavaScript is untyped, so capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just be consistent.

    BAD CODE:
    ```Javascript
    const DAYS_IN_WEEK = 7;
    const daysInMonth = 30;

    const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
    const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

    eraseDatabase() {}
    restore_database() {}

    class animal {}
    class Alpaca {}
    ```

    GOOD CODE:
    ```Javascript
    const DAYS_IN_WEEK = 7;
    const DAYS_IN_MONTH = 30;

    const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
    const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

    eraseDatabase() {}
    restoreDatabase() {}

    class Animal {}
    class Alpaca {}
    ```

2. Function callers and callees should be close

    If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee. We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

    BAD CODE:
    ```Javascript
    class PerformanceReview {
        constructor(employee) {
            this.employee = employee;
        }

        lookupPeers() {
            return db.lookup(this.employee, 'peers');
        }

        lookupManager() {
            return db.lookup(this.employee, 'manager');
        }

        getPeerReviews() {
            const peers = this.lookupPeers();
            // ...
        }

        perfReview() {
            this.getPeerReviews();
            this.getManagerReview();
            this.getSelfReview();
        }

        getManagerReview() {
            const manager = this.lookupManager();
        }

        getSelfReview() {
        }
    }

    const review = new PerformanceReview(employee);
    review.perfReview();
    ```

    GOOD CODE:
    ```Javascript
    class PerformanceReview {
        constructor(employee) {
            this.employee = employee;
        }

        perfReview() {
            this.getPeerReviews();
            this.getManagerReview();
            this.getSelfReview();
        }

        getPeerReviews() {
            const peers = this.lookupPeers();
            // ...
        }

        lookupPeers() {
            return db.lookup(this.employee, 'peers');
        }

        getManagerReview() {
            const manager = this.lookupManager();
        }

        lookupManager() {
            return db.lookup(this.employee, 'manager');
        }

        getSelfReview() {
        }
    }

    const review = new PerformanceReview(employee);
    review.perfReview();
    ```

#### Comments <a name="comments"></a>

1. Only comment things that have business logic complexity.

    Comments are an apology, not a requirement. Good code mostly documents itself.

    BAD CODE:
    ```Javascript
    hashIt(data) {
        // The hash

        let hash = 0;

        // Length of string

        const length = data.length;

        // Loop through every character in data

        for (let i = 0; i < length; i++) {
            // Get character code.

            const char = data.charCodeAt(i);
            // Make the hash

            hash = ((hash << 5) - hash) + char;
            // Convert to 32-bit integer

            hash &= hash;
        }
    }
    ```

    GOOD CODE:
    ```Javascript
    hashIt(data) {
        let hash = 0;
        const length = data.length;

        for (let i = 0; i < length; i++) {
            const char = data.charCodeAt(i);
            hash = ((hash << 5) - hash) + char;
            // Convert to 32-bit integer

            hash &= hash;
        }
    }
    ```
2. Don't leave commented out code in your codebase

    Version control exists for a reason. Leave old code in your history.

    BAD CODE:
    ```Javascript
    doStuff();
    // doOtherStuff();
    // doSomeMoreStuff();
    // doSoMuchStuff();
    ```

    GOOD CODE:
    ```Javascript
    doStuff();
    ```
3. Don't have journal comments

    Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use git log to get history!

    BAD CODE:
    ```Javascript

        /**
        * 2016-12-20: Removed monads, didn't understand them (RM)
        * 2016-10-01: Improved using special monads (JP)
        * 2016-02-03: Removed type-checking (LI)
        * 2015-03-14: Added combine with type-checking (JR)
        */
        combine(a, b) {
            return a + b;
        }
    ```

    GOOD CODE:
    ```Javascript
    combine(a, b) {
        return a + b;
    }
    ```

4. Avoid positional markers

    They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.

    BAD CODE:
    ```Javascript
    ////////////////////////////////////////////////////////////////////////////////
    // Scope Model Instantiation
    ////////////////////////////////////////////////////////////////////////////////

    $scope.model = {
        menu: 'foo',
        nav: 'bar'
    };

    ////////////////////////////////////////////////////////////////////////////////
    // Action setup
    ////////////////////////////////////////////////////////////////////////////////

    const actions = function() {
    };
    ```

    GOOD CODE:
    ```Javascript
    $scope.model = {
        menu: 'foo',
        nav: 'bar'
    };

    const actions = function() {
    };
    ```

[1]: /my-blog/img/portfolio/content5/img1.jpg
[2]: /my-blog/img/portfolio/content5/img2.jpg