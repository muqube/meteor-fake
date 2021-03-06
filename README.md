Notes for updating to Meteor 1.3.3
----------------------------------

This package is now called `muqube:fake`.



# Fake text and data generator for Meteor.js [![Build Status](https://travis-ci.org/muqube/meteor-fake.svg?branch=master)](https://travis-ci.org/muqube/meteor-fake)

From an article about *Lorem ipsum*:

> It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text (...)

And here's the problem: since *Lorem ipsum* is so widely used among developers and designers, it became recognizable and is as distracting as plain English text.

Random text doesn't have this problem. Every time you fill database with fake data, this package creates text randomly generated from most popular English syllabes. The result is a not distracting, random Latin-looking piece of text that still has frequencies and qualities of a typical English article.

Additional utility function allow you to create a fake user object.

If you need more functionality, like fake web address or geographical location, please extend the package and send me a pull request.

# Usage

    meteor add muqube:fake


# API


&nbsp;

&nbsp;

### `Fake.word()`

Returns a random word.

*Sample result:*

> Ingbelytion


&nbsp;

&nbsp;

### `Fake.sentence([length])`

*Sample result:*

> Perexnation tily ly someveraticmer sontionamagment inesersinar.

*Optional params:*

- `length`: number of words in the sentence


&nbsp;

&nbsp;

### `Fake.paragraph([length])`


*Sample result:*

> Agepro aldeadunar cones mere exintyecar ofsub refi icerse a muter. Terasedersmag citeden tertionscomiest com ed termenttain lotione uper terpre. Bleday fiu so manen ri. Extionoter ing dity artal erexi edi. De alre ieran altic dianati com. Bani naetordecom reerly comture u reti. Oyoraing con laenter perer com titer. Lyrec tlelec mened tionfulto condide enper miscom terty tionco enterthe ertle. Etuex ingtiesrily troi inter rilow exdisuget ingre lycon tanuperpro ialardisto exes. Inuausry allyaca disna ence aget. Af atias oalthe ac nessence dyob tiveraedi enmu conni. Addytionli bleestydi mocom com naoes. Ryture deu inter mansub.

*Optional params:*

- `length`: number of sentences in the paragraph



&nbsp;

&nbsp;

### `Fake.user([params])`

*Sample result:*

    {
      "name": "Kate",
      "surname": "Belyto",
      "fullname": "Kate Belyto",
      "email": "kate@proespa.com",
    }

*Optional params:*
 
- `fields`: Decide what fields will be included in the result object. Possible values:
    - name
    - surname
    - fullname
    - username
    - email
    - emails.address,
    - profile.name

*Example:*

        Fake.user({
            fields: ['name', 'username', 'emails.address', 'profile.name'],
        });

&nbsp;

&nbsp;

### `Fake.color()`

Returns a random named html color.

*Sample result:*

> navajowhite


&nbsp;

&nbsp;

### `Fake.fromArray(customArray)`

Selects a random element from provided array.

*Example:*

    fake.fromArray(['banana', 'apple', 'strawberry', 'raspberry', 'pear']);
    // 'pear'


&nbsp;

&nbsp;

### `Fake.simpleSchemaDoc(schema, [preset])`

Returns a random object created from [SimpleSchema](http://github.com/aldeed/meteor-simple-schema) definition. If a preset is provided, then fields are pre-populated with values in the preset.

Supported types:
* `Number`
* `String`
* `Boolean`
* `Date`
* `Object`
* `[Object]` SimpleSchema object also 

These options are supported for `Number` type:

* `decimal`
* `max` 
* `min`

These options are supported for `String` type:

* `max` 
* `min`
* `allowedValues`

*Example:*

Generating a simple schema documents

    var BookSchema = new SimpleSchema({
        title: {
            type: String
        },
        summary: {
            type: String
            max: 1000,
            min: 100
        }
        pages: {
            type: Number
        },
        available: {
            type: Boolean
        },
        price: {
            type: Number,
            max: 100,
            min: 25
        },
        promotion: {
            type: String,
            allowedValues: [
                "featured",
                "promoted",
                "editor's pick",
                "popular"
            ]
        }
    });
    var fakeDoc = Fake.simpleSchemaDoc(BookSchema);

output:

    { 
        "title": "Orlycon ingcal tion comingedthe modecomal detion reed n.",
        "summary": "Ybecom perdythe cona ananed re de esni modiloalse.Ygen teres ble nesso ic.E es conesmo acor tyex.", // max and min limit respected
        "pages": 5721755277461235, // very big number is likely to appear since max and min limits were not specified for this field
        "available": true,
        "price": 93 // max and min limit respected,
        "promotion": "popular" // a value from the allowedValues array
    }

Generating a document with preset

    var WeatherSchema = new SimpleSchema({
        temperature: {
            type: Number,
            min: -100,
            max: 100
        },
        location: {
            type: String
        },
        weatherType: {
            type: String,
            allowedValues: [
                'sunny',
                'rainy',
                'cloudy',
                'chilly',
                'stormy'
            ]
        }
    });
    var fakeWeather = Fake.simpleSchemaDoc(WeatherSchema, {temperature: 11});

output:

    {
        "temperature": 11,
        "location": "evelo",
        "weatherType": "stormy"
    }

&nbsp;

&nbsp;

### `Fake.simpleSchemaArray(schema, [length], [initialDoc])`

Returns an array of random objects created from [SimpleSchema](http://github.com/aldeed/meteor-simple-schema) definition. If a preset is provided, then fields are pre-populated with values in the preset.
of use cycle of Fake.simpleSchemaDoc instead; 


### "fake" field at schema object

You can add some custom fake data for field using "fake" field when you create SimpleSchema object.

Code bottom set value of field "user" with real user id from Meteor.users.
```
    const SchemaObject = new SimpleSchema({
        value: {
            optional: false,
            type: String
        },
        user: {
            optional: false,
            type: Object,
            blackbox: true,
            fake: () => {
                let u = Meteor.users.findOne();
                const {_id, profile: {firstName, lastName}} = u;
                let res = {_id, firstName, lastName};
                return res;
            }
        }
    });
```

## History

This package is forked from `anti:fake`. I forked it and added some modifications to support simple schema docs generation.
