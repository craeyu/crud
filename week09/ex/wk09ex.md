# Week 09 Exercise

## Part 1: Setup

This week we wll use **MongoDB**. Download the `wk09ex.zip` file from CCLE and unzip it. Then open a command line terminal and `cd` to the `wk09ex` directory. You should see the following files in the folder:
- `stack.yml`
- `wk09ex.js`
- `wk09ex.md`

You can initiate the database server with the following Docker command:

        docker-compose -f stack.yml up -d

Once it is up and running, log in to the server:

        docker exec -it wk09ex_mongo_1 bash

Then log in to Mongo itself:

        mongo admin -u root -p

The password is `exercise`

Now declare the name of the database you want to use:

        use wk09ex

OK, your MongoDB database is ready to use!

***Important!*** Copy all the operations you write for Part 2 and Part 3 into the `wk09ex.js` file and submit it via CCLE.

## Part 2: Mongo CRUD

### Part 2a: Creating Data

OK, now try inserting some data. All commands are in JavaScript. You can insert with the following syntax: `db.collection.insert(object)`. The `collection` is a placeholder for a collection name of your own making. You do not have to declare it before using it. The `object` is a JSON object containing anything you want. There is not schema to conform to.

**Insert Op 1**

Insert the following document into a collection named `books`.

```javascript
{
    title: "Zen and the Art of Motorcycle Maintenance",
    author: "Pirsig, Robert M.",
    year: 1974,
    classification: {
        subject: [
            {heading:"Philosophy"},
            {heading:"Pirsig, Robert M.", source:"LCSH"},
            {heading:"Pirsig, Robert M., -- 1928-2017.", source:"LCSH"},
            {heading:"Fathers and sons -- United States.", source:"LCSH"},
            {heading:"Self.", source:"LCSH"},
            {heading:"Fathers and sons.", source:"LCSH"},
            {heading:"United States.", source:"LCSH"}
        ],
        genre: "Fiction"
    }
}
```

You have just inserted one JSON document to the new `books` collection. You can see that the collection has been created by issuing the following command:

        show collections

And you can look up the document you just inserted by issuing the following command:

        db.books.find()

That should return a result like this (but not pretty-printed):

```javascript
{ 
    "_id" : ObjectId("5b0d694cd3497284d9b88f1e"), 
    "title" : "Zen and the Art of Motorcycle Maintenance", 
    "author" : "Pirsig, Robert M.", 
    "year" : 1974, 
    "classification" : { 
        "subject" : [ 
            { "heading" : "Philosophy" }, 
            { "heading" : "Pirsig, Robert M.", "source" : "LCSH" }, 
            { "heading" : "Pirsig, Robert M., -- 1928-2017.", "source" : "LCSH" },
            { "heading" : "Fathers and sons -- United States.", "source" : "LCSH" },
            { "heading" : "Self.", "source" : "LCSH" }, 
            { "heading" : "Fathers and sons.", "source" : "LCSH" }, 
            { "heading" : "United States.", "source" : "LCSH" } 
        ], 
        "genre" : "Fiction" 
    } 
}
```

***Tip:*** You can make Mongo pretty-print the results of a command by adding `.pretty()` to the end. e.g. `db.books.find().pretty()`.

The `ObjectId` will be different, as each ID is generated with a combination of timestamp, machine ID, process ID, and an incremental counter. However, you can override the autogenerated IDs and provide one of your own making. Let"s try that now by inserting some more books, but use the ISBN number as the ID.

**Insert Op 2**

Insert the following documents into the `books` collection.

```javascript
{
    _id: "0-553-34584-2",
    title: "The Mind's I",
    author: ["Dennet, Daniel", "Hofstadter, Douglas"],
    year: 1981,
    classification: {
        subject: [
            {heading:"Consciousness", source:"Wikipedia"},
            {heading:"Intellect", source:"Wikipedia"},
            {heading:"Philosophy", source:"Wikipedia"},
            {heading:"Mind-body dichotomy", source:"Wikipedia"},
            {heading:"Cognitive psychology", source:"Wikipedia"},
            {heading:"Theology", source:"Wikipedia"},
            {heading:"Self (philosophy)", source:"Wikipedia"},
            {heading:"Soul (spirit)", source:"Wikipedia"},
            {heading:"Consciousness", source:"LCSH"},
            {heading:"Consciousness -- Literary collections", source:"LCSH"},
            {heading:"Intellect", source:"LCSH"},
            {heading:"Intellect -- Literary collections", source:"LCSH"},
            {heading:"Self (Philosophy)", source:"LCSH"},
            {heading:"Self (Philosophy) -- Literary collections", source:"LCSH"},
            {heading:"Soul", source:"LCSH"},
            {heading:"Soul -- Literary collections", source:"LCSH"}
        ],
        genre: "Nonfiction"
    }
}
```

```javascript
{
    _id: "0-688-12141-1",
    title: "The Language Instinct",
    author: "Pinker, Stephen",
    year: 1994,
    classification: {
        subject: [
            {heading:"Psycholinguistics", source:"Wikipedia"},
            {heading:"Evolutionary psychology", source:"Wikipedia"},
            {heading:"Evolutionary psychology of language", source:"Wikipedia"},
            {heading:"Linguistics", source:"Wikipedia"},
            {heading:"Biolinguistics.", source:"LCSH"},
            {heading:"Language and languages.", source:"LCSH"}
        ],
        genre: "Nonfiction"
    }
}
```

```javascript
{
    _id: "0-19-857519-X",
    title: "The Selfish Gene",
    author: "Dawkins, Richard",
    year: 1976,
    classification: {
        subject: [
            {heading:"Evolutionary Biology", source:"Wikipedia"},
            {heading:"Evolution (Biology) -- Popular works", source:"LCSH"},
            {heading:"Natural selection -- Popular works", source:"LCSH"},
            {heading:"Evolution (Biology)", source:"LCSH"},
            {heading:"Natural Selection", source:"LCSH"}
        ],
        genre: "Nonfiction"
    }
}
```

```javascript
{
    _id: "0-393-03891-2",
    title: "Guns, Germs, and Steel",
    author: "Diamond, Jared",
    year: 1997,
    classification: {
        subject: [
            {heading:"Geography", source:"Wikipedia"},
            {heading:"social evolution", source:"Wikipedia"},
            {heading:"ethnology", source:"Wikipedia"},
            {heading:"cultural diffusion", source:"Wikipedia"},
            {heading:"Social evolution.", source:"LCSH"},
            {heading:"Civilization -- History.", source:"LCSH"},
            {heading:"Ethnology.", source:"LCSH"},
            {heading:"Human beings -- Effect of environment on.", source:"LCSH"},
            {heading:"Culture diffusion.", source:"LCSH"},
            {heading:"Civilization.", source:"LCSH"},
        ],
        genre: "Nonfiction"
    }
}
```

### Part 2b: Retrieving Data

Great, now try retrieving one of those records using the `find()` method.

**Find Op 1:** Retrieve the record with the id "0-688-12141-1"

That should return the entire document for "The Language Instinct".

---

What if we didn"t want the entire record, but just one or two fields? The second parameter of the `find()` method is an object of fields and a `1` or `0` to signify if you want it or not.

**Find Op 2:** Perform the same search as Find Op 1, but display only the `title` and `author` fields.

*Expected Result:*

```json
{ 
    "_id" : "0-688-12141-1", 
    "title" : "The Language Instinct", 
    "author" : "Pinker, Stephen" 
}
```

Notice that it still returned the ID. To omit that you must expressly declare so.

---

**Find Op 3:** Perform the same search as Find Op 2, but omit the `_id` field.

*Expected Result:*

```json
{ "title" : "The Language Instinct", "author" : "Pinker, Stephen" }
```

---

Instead of listing all the fields we want, we could just set the fields we don"t want to `0` and it will automatically return the rest.

**Find Op 4:** Perform the same search as Find Op 1, but omit the `_id`, `author`, and `year` fields from the results display.

*Expected Result:*

```json
{ 
    "title" : "The Language Instinct", 
    "classification" : { 
        "subject" : [ 
            { "heading" : "Psycholinguistics", "source" : "Wikipedia" }, 
            { "heading" : "Evolutionary psychology", "source" : "Wikipedia" },
            { "heading" : "Evolutionary psychology of language", "source" : "Wikipedia" }, 
            { "heading" : "Linguistics", "source" : "Wikipedia" }, 
            { "heading" : "Biolinguistics.", "source" : "LCSH" }, 
            { "heading" : "Language and languages.", "source" : "LCSH" } 
        ], 
    "genre" : "Nonfiction" } 
}
```

---

Instead of looking up by a specific ID, you can provide a combination of search parameters and also use regular expressions and other operators.

**Find Op 5:** Find all the books with a publication year greater than or equal to 1980 and with the the word "The" in the title. Display only the `title` and `year` fields.

*Expected Result:*

```json
{ "title" : "The Mind's I", "year" : 1981 }
{ "title" : "The Language Instinct", "year" : 1994 }
```

---

The parameter for a single field can be complex, rather than a single criteria. 

**Find Op 6:** Find all the books with a publication date between 1980 and 1990, inclusive. Display only the titles and years.

*Expected Result:* Just one hit

```json
{ "title" : "The Mind's I", "year" : 1981 }
```


---
What about fields that have multiple values, like subject or genre?

**Find Op 7:** Find all the books that have "Biology" in a subject heading. Display only the title and subjects in the results. 

The `heading` field is actually a part of a nested document. In order to drill down we have to use the dot notation of `"classification.subject.heading"` and wrap it in quotes.

*Expected Result:*

```json
{ 
    "title" : "The Selfish Gene", 
    "year" : 1976, 
    "classification" : { 
        "subject" : [ 
            { "heading" : "Evolutionary Biology", "source" : "Wikipedia" }, 
            { "heading" : "Evolution (Biology) -- Popular works", "source" : "LCSH" }, 
            { "heading" : "Natural selection -- Popular works", "source" : "LCSH" }, 
            { "heading" : "Evolution (Biology)", "source" : "LCSH" }, 
            { "heading" : "Natural Selection", "source" : "LCSH" } 
        ] 
    } 
}
```

---

**Find Op 8:** Find all books with a genre of fiction. Display the title, year, and genre fields.

*Expected Result:*

```json
{
    "title" : "Zen and the Art of Motorcycle Maintenance",
    "year" : 1974,
    "classification" : {
        "genre" : "Fiction"
    }
}
```

---

**Find Op 9**: Use the `$all` operator to find books that have both "Philosophy" and "Psychology" in their subject headings. Display the title, year, and subjects.

*Expected Result:*

```json
{ 
    "title" : "The Mind's I", 
    "year" : 1981, 
    "classification" : { 
        "subject" : [ 
            { "heading" : "Consciousness", "source" : "Wikipedia" }, 
            { "heading" : "Intellect", "source" : "Wikipedia" }, 
            { "heading" : "Philosophy", "source" : "Wikipedia" }, 
            { "heading" : "Mind-body dichotomy", "source" : "Wikipedia" }, 
            { "heading" : "Cognitive psychology", "source" : "Wikipedia" }, 
            { "heading" : "Theology", "source" : "Wikipedia" }, 
            { "heading" : "Self (philosophy)", "source" : "Wikipedia" }, 
            { "heading" : "Soul (spirit)", "source" : "Wikipedia" }, 
            { "heading" : "Consciousness", "source" : "LCSH" }, 
            { "heading" : "Consciousness -- Literary collections", "source" : "LCSH" }, 
            { "heading" : "Intellect", "source" : "LCSH" }, 
            { "heading" : "Intellect -- Literary collections", "source" : "LCSH" }, 
            { "heading" : "Self (Philosophy)", "source" : "LCSH" }, 
            { "heading" : "Self (Philosophy) -- Literary collections", "source" : "LCSH" }, 
            { "heading" : "Soul", "source" : "LCSH" }, 
            { "heading" : "Soul -- Literary collections", "source" : "LCSH" } 
        ] 
    } 
}
```

---

**Find Op 10:** Perform the same operation as Find op 9, but find all books that have *either* term in their list of subjects. Use the `$in` operator.


*Expected Result:*

```json
{
    "title" : "Zen and the Art of Motorcycle Maintenance",
    "year" : 1974,
    "classification" : {
        "subject" : [
            {"heading" : "Philosophy"},
            {"heading" : "Pirsig, Robert M.", "source" : "LCSH"},
            {"heading" : "Pirsig, Robert M., -- 1928-2017.", 
                "source" : "LCSH"},
            {"heading" : "Fathers and sons -- United States.", 
                "source" : "LCSH"},
            {"heading" : "Self.", "source" : "LCSH"},
            {"heading" : "Fathers and sons.", "source" : "LCSH"},
            {"heading" : "United States.", "source" : "LCSH"}
        ]
    }
}
{
    "title" : "The Mind's I",
    "year" : 1981,
    "classification" : {
        "subject" : [
            {"heading" : "Consciousness", "source" : "Wikipedia"},
            {"heading" : "Intellect", "source" : "Wikipedia"},
            {"heading" : "Philosophy", "source" : "Wikipedia"},
            {"heading" : "Mind-body dichotomy", "source" : "Wikipedia"},
            {"heading" : "Cognitive psychology", "source" : "Wikipedia"},
            {"heading" : "Theology", "source" : "Wikipedia"},
            {"heading" : "Self (philosophy)", "source" : "Wikipedia"},
            {"heading" : "Soul (spirit)", "source" : "Wikipedia"},
            {"heading" : "Consciousness", "source" : "LCSH"},
            {"heading" : "Consciousness -- Literary collections", 
                "source" : "LCSH"},
            {"heading" : "Intellect", "source" : "LCSH"},
            {"heading" : "Intellect -- Literary collections", 
                "source" : "LCSH"},
            {"heading" : "Self (Philosophy)", "source" : "LCSH"},
            {"heading" : "Self (Philosophy) -- Literary collections", 
                "source" "LCSH"},
            {"heading" : "Soul", "source" : "LCSH"},
            {"heading" : "Soul -- Literary collections", "source" : "LCSH"}
        ]
    }
}
{
    "title" : "The Language Instinct",
    "year" : 1994,
    "classification" : {
        "subject" : [
            {"heading" : "Psycholinguistics", "source" : "Wikipedia"},
            {"heading" : "Evolutionary psychology", "source" : "Wikipedia"},
            {"heading" : "Evolutionary psychology of language", 
                "source" : "Wikipedia"},
            {"heading" : "Linguistics", "source" : "Wikipedia"},
            {"heading" : "Biolinguistics.", "source" : "LCSH"},
            {"heading" : "Language and languages.", "source" : "LCSH"}
        ]
    }
}
```

---

***Tip:*** Here is a list of all the available ***query selectors and operators***: https://docs.mongodb.com/manual/reference/operator/query/#query-selectors 

---

### Part 2c: Updating data

What if we want to add, remove, or change the value within a document? To do that, we use the `update` command like so: `db.collection.update(criteria, operation)`. 

**Update Op 1:** Add "autobiography" to the "genre" field for "Zen and the Art of Motorcycle Maintenance". You will need to use the `$set` operator. 

***Note!*** If you simply put `{"classification.genre": ["Fiction", "Autobiography"]}` in the `update()` command without using `$set` Mongo will replace the entire document! The following example is ***wrong!***

```javascript
// Don"t do this!!!
db.books.update(
    {title: "Zen and the Art of Motorcycle Maintenance"},
    {"classification.genre": ["fiction", "autobiography"]}
)
```

You should see the following response: `WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })`

---

***Tip:*** Here are the available ***update operators***: https://docs.mongodb.com/manual/reference/operator/update/

---

We should change the ID for "Zen and the Art of Motorcycle Maintenance" to be its ISBN number like the other books.

Try this:

```javascript
db.books.update(
    {title: "Zen and the Art of Motorcycle Maintenance"},
    {$set: {_id: "0-688-00230-7"}}
)
```

What happened?! You likely got an error that says `Performing an update on the path "_id" would modify the immutable field "_id"`

To update an `_id` field you will have to copy the data to a new entry and delete the old one.

---

### Part 2d: Deleting Data

We can use the `remove()` method to delete a document. But first we should copy the data and re-enter it with the OCLC number as the ID.

Do this:

```javascript
var doc = db.books.findOne({title: "Zen and the Art of Motorcycle Maintenance"})

var old_id = doc._id

doc._id = "0-688-00230-7"

db.books.insert(doc)
```

You should now see ***6*** books in the collection. Try this:

```javascript
db.books.count()
```

**Remove Op 1:** Now, remove the old document.

This should result in the following response: `WriteResult({ "nRemoved" : 1 })`, and you should now see ***5*** books in the collection.


## Part 3: Advanced Usage

### Part 3a: Element Match

What if we wanted to find a book that has a subject heading of "philosophy" from the LCSH terms?

Try this query:

```javascript
db.books.find(
    {
        "classification.subject.heading": /[pP]hilosophy/,
        "classification.subject.source": "LCSH"
    },
    {
        "_id": 0,
        "title": 1,
        "classification.subject": 1
    }
).pretty()
```

What happened?
- This returned two books. 
- But "Zen and the Art of Motorcycle Maintenance" does not have a heading of "Philosophy" from LCSH. 
- Both criteria are correct, when taken independently from each other. 
- What we really want is to match a document in which a single "subject" sub-document has "LCSH" as the source and "Philosophy" as the heading. 
- To do this we need to use **$elemMatch**.

**Adv Op 1:** Use `$elemMatch` to find books that have "philosophy" (case insensitive) in their subject headings, but only if the heading is from LCSH.

**Expected Result:** That should return only 1 book: "The Mind's Eye"

---

### Part 3b: References

MongoDB does not support joins like SQL, but it does have a feature similar to a foreign key, called a *reference*. 

Create a second collection of documents called `reviews`, which have references back to the `books` collection.

```javascript
db.reviews.insert(
    {
        rating: 5,
        review: "Mind-blowing! This book initiated my interest in cognitive science",
        book: {$ref:"books", $id:"0-553-34584-2"}
    }
)

db.reviews.insert(
    {
        rating: 5,
        review: "This books makes me want to improve the quality of my work in everything I do",
        book: {$ref:"books", $id:"0-688-00230-7"}
    }
)
```

**Adv Op 2:** Now execute `db.reviews.find()` to see the `_id` values and copy the first one. The perform a two step lookup of the review and the book it references.

---

### Part 3c: Aggregations

Just like SQL provides aggregate functions, so does MongoDB. For instance, if you want to get the count of the number of results, you can simply use the `count()` method instead of `find()`.

**Adv Op 3:** Get the count of books that have "philosophy" (case insensitive) in their subject headings"

*Expected Result:* **2**

---

**Adv Op 4:** Use the `distinct()` function to get a list of distinct values for the subject source.

*Expected Result:* `[ "LCSH", "Wikipedia" ]`

---

You can also use the method `aggregate()` to perform more complex tasks. Within it you can set up "stages" like `$match`, `$sort`, `$group`, and `$project` to perform functions similar to the `SELECT`, `ORDER BY` and `GROUP BY` clauses in SQL.

**Adv Op 5:** Use `$match`, `$sort`, and `$project` to sort the nonfiction books by publication year in descending order.

*Expected Result:*

```json
{ "title" : "Guns, Germs, and Steel", "year" : 1997 }
{ "title" : "The Language Instinct", "year" : 1994 }
{ "title" : "The Mind's I", "year" : 1981 }
{ "title" : "The Selfish Gene", "year" : 1976 }
```

---

**Adv Op 6:** Use `$group` and `$avg` to get the average publication year of all Nonfiction books.

*Expected Result:*

```json
{ "_id" : "averagePublication", "avgPub" : 1987 }
```

---

You can see the list of possible aggregation stages here: https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/

And the list of available aggregation operations here: https://docs.mongodb.com/manual/reference/operator/aggregation/

---

***Tip!*** Mongo Express is a graphical interface to Mongo. It is already running as part of the Docker stack you spun up. You can try it out at http://localhost:8082

---

## Part 4: Submission

Submit your `wk09ex.js` file via CCLE.