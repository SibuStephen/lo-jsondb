# Lo JsonDB [![Code Climate](https://codeclimate.com/github/renatorib/lo-jsondb/badges/gpa.svg)](https://codeclimate.com/github/renatorib/lo-jsondb) [![Build Status](https://travis-ci.org/renatorib/lo-jsondb.svg?branch=master)](https://travis-ci.org/renatorib/lo-jsondb)

Save your data in .json files and query it with create, delete, update and find. Thanks to **lodash** :)

## Why?
Just for fun.  
Thinking about performance, it's not very useful for large amounts of data.  
But, jsondb don't need to install anything, and it's easy to start to use.  
You can use to prototyping a project or to save a small data.  

## Getting started

#### Install:
```
npm install lo-jsondb --save
```
#### Collection:
```js
var jsondb = require('lo-jsondb');
var pokemons = jsondb('pokemons', {pretty: true});  
```
It will use a `pokemons.json` file to store data.  
If file does not exist, it will be created.  

You can create with deep path too, example:  
`jsondb('my/list/of/pokemons')`   

Also, you can pass .json directly, if you want for something reason:  
`jsondb('my/pokemons.json')`  

`{pretty: true}` means that file will be saved in readable json (with tabs)

#### Create:
```js
pokemons.create({name: 'Rattata', types: ['normal']});

pokemons.create([
    {name: 'Pikachu', types: ['electric']},
    {name: 'Bulbasaur', types: ['grass', 'poison']}
]);

pokemons.create(function(){
    return {name: 'Exeggcute', types: ['grass', 'psychic']};
});
```
Don't care about IDS!  
JsonDB uses auto incremention to generate ids, like mysql (1, 2, 3...);

#### Find:
```js
var grassPokemons = pokemons.find({types: ['grass']});
console.log(grassPokemons);
// [ {name: 'Bulbasaur', types: ['grass', 'poison'], id: 3},
//   {name: 'Exeggcute', types: ['grass', 'psychic'] id: 4} ]

var normalPokemons = pokemons.find({types: ['normal']})
console.log(normalPokemons);
// [ {name: 'Rattata', ...} ]

var normalPokemon = pokemons.findOne({types: ['normal']})
console.log(normalPokemon);
// {name: 'Rattata', ...}

var byId = pokemons.findOne(2);
// {name: 'Pikachu', ...}

var byIds = pokemons.find([1, 2]);
// [ {name: 'Rattata', ...},
//   {name: 'Pikachu', ...} ]

var byFunc = pokemons.find(function(pokemon){
    return pokemon.id == 1 || pokemon.name == 'Pikachu'
});
// [ {name: 'Rattata', ...},
//   {name: 'Pikachu', ...} ]
```

`.find()` always return an array of documents. Even if it's an id.  
`.findOne()` always return the document. If query match more than one, it will return the first only.  

You can find with Int (document id), Object (query) or a Function that return an id or query; Also, you can pass an Array with Objects (two or more queries), or array with Ids  

#### Update:
```js
pokemons.update({name: 'Pikachu'}, {name: 'Pika Pika', foo: 'bar'});
// -> {id: 2, name: 'Pika Pika', types: ['electric'], foo: 'bar'}

pokemons.update({name: 'Rattata'}, {foo: 'bar'}, true);
// -> {id: 2, foo: 'bar'}

pokemons.update({types: ['grass']}, {foo: 'bar'});
// -> {id: 3, name: 'Bulbasaur', types: ['grass', 'poison'], foo: 'bar'}
// -> {id: 4, name: 'Exeggcute', types: ['grass', 'psychic'], foo: 'bar'}

pokemons.update(1, {foo: 'bar'});
// -> {id: 1, name: 'Rattata', types: ['normal'], foo: 'bar'}
```

You can also query with functions and arrays:
```js
pokemons.update(function(pokemon){
    return pokemon.name == 'Rattata' || pokemon.id = 2
}, {...});
// updates Rattata and Pikachu

pokemons.update([1, 2], {...});
// updates Rattata and Pikachu

pokemons.update([{name: 'Rattata'}, {name: 'Pikachu'}], {...});
// updates Rattata and Pikachu
```

#### Delete:
```js
pokemons.delete(1); // Rattata
pokemons.delete([1, 2]); // Rattata and Pikachu
pokemons.delete({types: ['grass']}); // Bulbasaur and Exeggcute
pokemons.delete([{name: 'Rattata'}, {types: ['grass']}]); // Bulbasaur, Exeggcute and Rattata
pokemons.delete(function(pokemon){
    return pokemon.id == 2
}); // Pikachu
```

## Contributions:
Feel free to send pull requests.  
If you add more features, please don't forget to add tests in `test/test.js`
