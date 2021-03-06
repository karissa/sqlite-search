# sqlite-search

[![NPM](https://nodei.co/npm/sqlite-search.png)](https://nodei.co/npm/sqlite-search/)

## api

### require('sqlite-search')(opts, readyCallback)

`opts`:

- `path` - **required** - the path to where the sqlite db should be stored/read from
- `columns` - **required** - an array of columns, e.g. `['foo', 'bar']` to use as the schema for the search table
- `primaryKey` - **required** - the primary key that will be passed to `since` and will by default order the column

### `readyCallback`
Will get called with `err, instance`
```js
var sqliteSearch = require('sqlite-search')
sqliteSearch(opts, function (err, searcher) {
  searcher.createSearchStream(searchOpts)
})
```


### instance.createWriteStream()

returns a writable object stream. objects written into this stream will be stored in the search table. make sure the keys of your object were included in the `columns` array you passed into the constructor above

### instance.createSearchStream(opts)

returns a readable object stream that emits search result objects

`opts`:

- `field` - **required** - which column to search against
- `query` - **required** - the search query string, passed to `MATCH`
- `select` - an array of strings to use as the select arguments, e.g. `['foo', 'bar']` tranlates into `SELECT foo, bar`. defaults to `*` if not specified
- `order` - `ORDER BY ?` (default primaryKey)
- `since` - if supplied will put in an `AND ? > ?` with `[since, primaryKey]` to the query
- `limit` - `LIMIT ?`
- `offset` - `OFFSET ?`
- `statement` - optionally you can specify a full SQL statement to run as the query. if specified all other options will be ignored
- `formatType` - if set to 'object' will format the search response as JSON for streaming. See https://github.com/maxogden/npm-readme-search for an example.

