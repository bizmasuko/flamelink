> All the methods that you would need to work with the "Schemas" Flamelink data is available on the `app.schemas` namespace.

---

## .get()

To either retrieve a single schema entry or all the schemas once, ie. Give me my "Product Categories" schema.

This method does not *watch* for real-time db changes, but is intended to retrieve your schema once. If you are looking for real-time methods, take a look at the [`app.schemas.subscribe()`](/schemas?id=subscribe) method below.

*To get a specific schema:*

```javascript
app.schemas.get('product-categories')
  .then(schema => console.log('Product Categories Schema:', schema))
  .catch(error => console.error('Something went wrong while retrieving the schema. Details:', error));
```

*or to get all schemas (with options):*

```javascript
app.schemas.get({ fields: [ 'title', 'description', 'fields' ] })
  .then(allSchemas => console.log('All schemas with options applied:', allSchemas))
  .catch(error => console.error('Something went wrong while retrieving the entry. Details:', error));
```

### Input parameters

| Type   | Variable    | Required | Description                                            |
| ------ | ----------- | -------- | ------------------------------------------------------ |
| String | `schemaKey` | optional | The schema database key/reference you want to retrieve |
| Object | `options`   | optional | Additional options                                     |

?> **Tip:** Leave the schema key out or set to `null` to retrieve all schemas

#### Available Options

The following options can be specified when retrieving your schema(s):

##### Fields

- `fields` **{Array}** - A list of fields to be plucked from an/each schema entry.

*Example*

To retrieve the `'product-categories'` schema, but only the `title`, `description` and `fields` properties.

```javascript
app.schemas.get('product-categories', { fields: [ 'title', 'description', 'fields' ] })
```

##### Event

- `event` **{String}** - The Firebase child event to retrieve data for. By default, the event is `value`, which is used for retrieving the entire schema object at the given reference(s) path.

*Example*

The allowed child event options are: `value`, `child_added`, `child_changed`, `child_removed` and `child_moved`.

> To read more about these events, see the [Firebase docs](https://firebase.google.com/docs/database/web/lists-of-data#listen_for_child_events).

```javascript
app.schemas.get('product-categories', { event: 'child_changed' })
```

### Return value

A `Promise` that resolves to the reference `{Object}` on success or will reject with an error if the request fails.

---

## .getFields()

To retrieve only the `fields` array for either a single schema entry or all the schemas once, ie. Give me the fields for my "Product Categories" schema.

This method does not *watch* for real-time db changes, but is intended to retrieve your schema once. If you are looking for real-time methods, take a look at the [`app.schemas.subscribe()`](/schemas?id=subscribe) method below.

*To get the fields for a specific schema:*

```javascript
app.schemas.getFields('product-categories')
  .then(schema => console.log('Product Categories Schema Fields:', schema))
  .catch(error => console.error('Something went wrong while retrieving the schema fields. Details:', error));
```

*or to get the fields for all schemas (with options):*

```javascript
app.schemas.getFields({ fields: [ 'title', 'description' ] })
  .then(allSchemasFields => console.log('Fields for all schemas with options applied:', allSchemasFields))
  .catch(error => console.error('Something went wrong while retrieving the fields. Details:', error));
```

### Input parameters

| Type   | Variable    | Required | Description                                            |
| ------ | ----------- | -------- | ------------------------------------------------------ |
| String | `schemaKey` | optional | The schema database key/reference you want to retrieve |
| Object | `options`   | optional | Additional options                                     |

?> **Tip:** Leave the schema key out or set to `null` to retrieve all schemas

#### Available Options

The following options can be specified when retrieving your schema(s):

##### Fields

- `fields` **{Array}** - A list of fields to be plucked from an/each field entry.

*Example*

To retrieve the `'product-categories'` schema fields, but only the `title` and `description` properties.

```javascript
app.schemas.getFields('product-categories', { fields: [ 'title', 'description' ] })
```

### Return value

A `Promise` that resolves to the fields `{Array}` for single schemas or `{Object}` for all schemas on success or will reject with an error if the request fails.

---

## .subscribe()

This method is similar to the `app.schemas.get()` method, except that where the `.get()` method returns a `Promise` resolving to the once-off value, this method subscribes to either a single schema entry or all the schemas for real-time updates. A callback method should be passed as the last argument which will be called each time the data changes in your Firebase db.

If you are looking for retrieving data once, take a look at the [`app.schemas.get()`](/schemas?id=get) method above.

*To subscribe to a specific individual schema:*

```javascript
app.schemas.subscribe('product-categories', function(error, schema) {
  if (error) {
    return console.error('Something went wrong while retrieving all the schema. Details:', error);
  }
  console.log('The product categories schema:', schema);
});
```

*To subscribe to the `child_added` child event for a specific schema:*

```javascript
app.schemas.subscribe('product-categories', { event: 'child_added' }, function(error, schema) {
  if (error) {
    return console.error('Something went wrong while retrieving the schema property that got added. Details:', error);
  }
  console.log('The product categories schema that got added:', schema);
});
```

*To subscribe to all schemas (with options):*

```javascript
app.schemas.subscribe({ fields: [ 'title', 'description', 'fields' ] }, function(error, schemas) {
  if (error) {
    return console.error('Something went wrong while retrieving the schemas. Details:', error);
  }
  console.log('All schemas with options applied:', schemas);
});
```

?> **HOT TIP:** If you are using [RxJS Observables](http://reactivex.io/rxjs/) and you don't like callbacks, turn this `subscribe` method into an **Observable** like this:
```javascript
const getSchemaObservable = Rx.Observable.bindCallback(app.schemas.subscribe);
getSchemaObservable('product-categories').subscribe()
```

### Input parameters

Parameters should be passed in the order of the following table. If an optional parameter, like the `options` are left out, the following parameter just moves in its place.

| Type     | Variable    | Required | Description                                                           |
| -------- | ----------- | -------- | --------------------------------------------------------------------- |
| String   | `schemaKey` | optional | The schema database key or reference you want to retrieve             |
| Object   | `options`   | optional | Additional options                                                    |
| Function | `callback`  | required | Function called once when subscribed and when subscribed data changes |

#### Available Options

The following options can be specified when retrieving your schema data:

##### Fields

- `fields` **{Array}** - A list of fields to be plucked from schemas.

*Example*

To retrieve your product categories schema, but only the `title`, `description` and `fields` property.

```javascript
app.schemas.subscribe('product-categories', { fields: [ 'title', 'description', 'fields' ] }, function(error, schema) {
  // Handle callback
});
```

##### Event

- `event` **{String}** - The Firebase child event to retrieve data for. By default, the event is `value`, which is used for retrieving the entire schema object at the given reference(s) path.

*Example*

The allowed child event options are: `value`, `child_added`, `child_changed`, `child_removed` and `child_moved`.

> To read more about these events, see the [Firebase docs](https://firebase.google.com/docs/database/web/lists-of-data#listen_for_child_events).

```javascript
app.schemas.subscribe({ event: 'child_changed' }, function(error, schemas) {
  // Handle callback
})
```

### Return value

This method has no return value, but makes use of an [error-first callback](https://www.google.com/search?q=error-first+callback&oq=javascript+error-first+callback) function that should be passed as the last argument.

---

## .unsubscribe()

This method is used to unsubscribe from previously subscribed schema updates or other child events. It is the equivalent of Firebase's `.off()` method and taking the Flamelink data structures into consideration.

*To unsubscribe from a specific schema:*

```javascript
app.schemas.unsubscribe('product-categories');
```

*To unsubscribe from the `child_removed` event for a specific schema:*

```javascript
app.schemas.unsubscribe('product-categories', 'child_removed');
```

*To unsubscribe from all events for the schemas:*

```javascript
app.schemas.unsubscribe();
```

### Input parameters

All parameters are optional and calling this method without options will unsubscribe from all callbacks.

| Type     | Variable    | Required | Description                                                    |
| -------- | ----------- | -------- | -------------------------------------------------------------- |
| String   | `schemaKey` | optional | The schema key or reference to unsubscribe from                |
| String   | `event`     | optional | The child event to unsubscribe from (see allowed child events) |

### Return value

This method has no return value.

---

## .set()

> This is a more advanced API method, that for most use cases will not be necessary. Only use it if you know what you are doing. If you mess this up, you might break your CMS. It is strongly advised to use backups.

This method can be used to save data and overwrite the whole object for a given schema.

!> **FIRE RISK WARNING:** Using `set()` overwrites all the data for the specified schema(s), including any child nodes. For this reason, this method can only be used to update an individual schema and not all schemas at once. It is generally safer to use the `app.schemas.update()` method to patch only the specified properties. **With great power comes great responsibility, Peter Parker.**

```javascript
app.schemas.set('product-categories', { id: 'product-categories', title: 'Product Categories', ...All other properties... })
  .then(() => console.log('Setting the schema data succeeded'))
  .catch(() => console.error('Something went wrong while setting the schema data.'));
```

?> It is important to note that this method will set the entry's `id` as well as the `createdBy` and `createdDate` meta data for you.

### Input parameters

| Type   | Variable    | Required | Description                                                |
| ------ | ----------- | -------- | ---------------------------------------------------------- |
| String | `schemaKey` | required | The schema key or reference for the schema you want to set |
| Object | `payload`   | required | Payload object to set at the given schema reference        |

### Return value

A `Promise` that resolves when the payload is set or will reject with an error if the request fails.

---

## .update()

> **FIRE RISK WARNING:** This is a more advanced API method, that for most use cases will not be necessary. Only use it if you know what you are doing. If you mess this up, you might break your CMS. It is strongly advised to use backups.

This method can be used to save data for a single given schema without overwriting other child properties.

!> This method can only be used to update the data for an individual schema at a time and not to update all the schemas.

```javascript
app.schemas.update('product-categories', { id: 'product-categories', title: 'Product Categories', ...Optional other properties... })
  .then(() => console.log('Updating the schema succeeded'))
  .catch(() => console.error('Something went wrong while updating the schema.'));
```

?> It is important to note that this method will set the entry's `id` as well as the `lastModifiedBy` and `lastModifiedDate` meta data for you.

### Input parameters

| Type   | Variable    | Required | Description                                                   |
| ------ | ----------- | -------- | ------------------------------------------------------------- |
| String | `schemaKey` | required | The schema key or reference for the schema you want to update |
| Object | `updates`   | required | Payload object to update at the given schema's reference      |

### Return value

A `Promise` that resolves when the payload is update or will reject with an error if the request fails.

---

## .remove()

> **FIRE RISK WARNING:** This is a more advanced API method, that for most use cases will not be necessary. Only use it if you know what you are doing. If you mess this up, you might break your CMS. It is strongly advised to use backups.

This method can only be used to remove an individual schema.

```javascript
app.schemas.remove('product-categories')
  .then(() => console.log('Removing the schema succeeded'))
  .catch(() => console.error('Something went wrong while removing the schema.'));
```

?> **Tip:** A schema can also be removed by passing `null` as the payload to the `app.schemas.set()` or `app.schemas.update()` methods. Be careful!

### Input parameters

| Type   | Variable    | Required | Description                           |
| ------ | ----------- | -------- | ------------------------------------- |
| String | `schemaKey` | required | The schema key or reference to remove |

### Return value

A `Promise` that resolves when the schema is removed or will reject with an error if the request fails.

---

## .transaction()

> **FIRE RISK WARNING:** This is a more advanced API method, that for most use cases will not be necessary. Only use it if you know what you are doing. If you mess this up, you might break your CMS. It is strongly advised to use backups.

If you need to update a schema whose data could be corrupted by concurrent changes, Firebase allows us to perform a "transaction" update that updates data based on the existing data/state.

> Read more about transactions in the [Firebase docs](https://firebase.google.com/docs/reference/js/firebase.database.Reference#transaction).

```javascript
app.schemas.transaction(
  'product-categories',
  function updateFn(schema) {
    // Take in the existing state (schema) and return the new state
    return schema;
  },
  function callback() {
    // Transaction finished
  }
);
```

### Input parameters

| Type     | Variable    | Required | Description                                                            |
| -------- | ----------- | -------- | ---------------------------------------------------------------------- |
| String   | `schemaKey` | required | The schema key or reference for the schema you want to update          |
| Function | `updateFn`  | required | The update function that will be called with the existing schema state |
| Function | `callback`  | optional | The callback function that will be called when transaction finishes    |

### Return value

This method has no return value. Use the optional `callback` function to determine when the transaction succeeded.

---

## .ref()

> **FIRE RISK WARNING:** This is a more advanced API method, that for most use cases will not be necessary.

To retrieve a context aware (environment and locale) reference to any node/location within your "Schemas" data.

```javascript
app.schemas.ref('your-reference')
  .then(reference => console.log('The reference:', reference)
  .catch(error => console.error('Something went wrong while retrieving the reference. Details:', error);
```

### Input parameters

The `.ref()` method takes a single parameter

| Type   | Variable    | Required | Description                         |
| ------ | ----------- | -------- | ----------------------------------- |
| String | `reference` | optional | The reference you want to retrieve. |

?> **Tip:** Leave the `reference` out or set to `null` to get a reference to all the schemas

### Return value

A `Promise` that resolves to the reference `{Object}` on success or will reject with an error if the request fails.

---

Next up: [Storage/Media](/storage)

> 🔥🔥🔥 **Feel the Burn!!! Feel the Deep Burn!!** 🔥🔥🔥