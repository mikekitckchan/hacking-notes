## Prototype Pollution

### Introduction
- In Javascript, every objects are collections of key pair. Each pairs are what we called properties in Javascript. For example:

```js
function User(username, password){
    this.username=username;
    this.password=password;
}
```

- In above example, an User is an object and its properties are username and password. To create a new user, we can simply input:

```js
var userA = new User("John", "12345678");

console.log("User "+userA.username + " is created"); //output: User John is created

```

- To change the object properties, we can make a function to change the value of its property(ies). For example, if we want to create a function to change user password, we can make a function like below:

```js
User.prototype.change_password = function(var new_pw){
return this.password=new_pw;
}

userA.change_password("1234"); 

console.log("User "+userA.name+" password now is "+userA.password; //User John passowrd now is 1234
```
- In the above code, a new word prototype came up. Prototype instruction the change_password function pointed back to User object and changed its underlying property(ies). In the example above, we used ```this.password=new_pw;``` to instruct the function to change userA's password. 

### First Exploit using prototype pollution

- Taking the same example, if we create an userB object like below:-

```js

var userB = new User("Sally", "alive");

``` 

- Considering we have another code below:

```js

userB.constructor.prototype.is_admin = "yes";

```

- According to the code above, constructor means it pointed back to the object of userB (i.e. User) and add a property to User called is_admin with default value "yes". As this changed apply to the object User, if we print out userA's is_admin property, we would find out that it has changed userA as well:

```js
console.log(userA.is_admin); //yes
```

Similary, ```constructor.prototype``` can also be presented as ```__proto__``` like below:-

```js

userB.__proto__.is_admin = "yes";
```


### Prototype pollution using Merge

- Consider 2 objects as below:

```js
var userA = {"name":"John" , "password":"123", "admin":"True"};
var userB = {"name":"Peter", "password":"234"};

```
- Now, we write a function to merge these 2 objects
```js
const isObject = obj => obj && obj.constructor && obj.constructor === Object;
function merge(dest, src) {
    for (var attr in src) {
        if (isObject(dest[attr]) && isObject(src[attr])) {
            merge(dest[attr], src[attr]);
        } else {
            dest[attr] = src[attr];
        }
    }
    return dest
}
```
- In above code, it basically just merge 2 objects into 1. So, if we run the code below:-

```js
var c=merge(userA, userB);

console.log(c); //{name:"Peter",password:"234",admin:"True"}
```

- Now, if we add below code:
```js
var userC = JSON.parse('{"__proto__":{"is_hacked":"True"}}');
var c=merge(userA, userC);

console.log(userB.is_hacked); //True
```

- You can see that if we create userC, we pass a property called __proto__ to the new userC. Then, if we merge this with userA, the new property would also affect userB that is supposed not to be affected in this.





