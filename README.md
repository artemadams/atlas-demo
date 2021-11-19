# Atlas Demo Code Snippets

__Code snippets for the exercises__

---

## Atlas Search

__1. Stage `$search` Operator__

```javascript
{
    search: {
        path: 'fullplot',
        query: 'werewolves and vampires'
    },
    highlight: {
        path: 'fullplot'
    }
}
```

__2. Stage `$project` Operator__

```javascript
{
    title: 1,
    _id: 0,
    year: 1,
    fullplot: 1,
    score: {
        $meta: 'searchScore'
    },
    highlight: {
        $meta: 'searchHighlights'
    }
}
```
__3. Stage `$limit` Operator__

```javascript
15
```

## Atlas Triggers

__Trigger Function__

```javascript
exports = function (changeEvent) {
    const fullDocument = changeEvent.fullDocument;
    const docId = changeEvent.documentKey._id;
    const relatedMovieId = fullDocument.movie_id;
    console.log('New comment doc inserted for Movie: ' + relatedMovieId);
    const collection = context.services.get("demo").db("sample_mflix").collection("movies");
    const doc = collection.updateOne({
        _id: relatedMovieId
    }, {
        $inc: {
            num_mflix_comments: 1
        }
    });
};
```

__Trigger Test__

```javascript
{
    "title": "Blacksmith Scene"
}
```

__Add Document__

Adding the document will activate the trigger function.
Note: `movie_id` matches the `_id` of “Blacksmith Scene" movie. If you selected another movie for this exercise make sure you use its `_id`.

```javascript
{
    "name": "Nice User",
    "email": "nice.user@fakegmail.com",
    "movie_id": {
        "$oid": "573a1390f29313caabcd4135"
    },
    "text": "What a great film!",
    "date": "2020-09-09T12:00:00.000+00:00"
}
```
