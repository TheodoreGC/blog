# JS Field Guide

## RegEx

### Named Capture Groups

```js
const regex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const [match, year, month, day] = regex.exec('2021-01-01');

console.log(year);
console.log(month);
console.log(day);

// Results
2021
01
01
```

## Loading data

### Cancel a call when using `fetch` API

```js
const controller = new AbortController();
const signal = controller.signal;

const successHandler = ({ json }) => json();
const errorHandler = ({ message }) => `Error: ${message}`;

const download = () =>
  fetch('https://my-url/path/to/resource', { signal })
    .then(successHandler)
	.catch(errorHandler);

const abort = () => {
  controller.abort();
};

const dlBtn = document.querySelector('.download-button');
const abtBtn = document.querySelector('.abort-button');

dlBtn.addEventListener('click', download);
abtBtn.addEventListener('click', abort);
```

## Lazy-loading

### Intersection Observer API

*[TBD](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)*

## Transform data

### Transform a dictionary (`Object`) into an array

```js
const foo = {
  a: { value: 'toto' },
  b: { value: 'tata' },
  c: { value: 'titi' },
  d: { value: 'tutu' },
};

const bar = Object.keys(foo).map(key => ({ key, ...foo[key] }));

console.log(bar);

// Result
[
  {
    "key": "a",
    "value": "toto"
  },
  {
    "key": "b",
    "value": "tata"
  },
  {
    "key": "c",
    "value": "titi"
  },
  {
    "key": "d",
    "value": "tutu"
  }
]
```

### Merging 2 dictionnary

```js
const foo = {
  a: { value: 'toto' },
  b: { value: 'tata' },
  c: { value: 'titi' },
  d: { value: 'tutu' },
};

const bar = {
  a: { value: 'toto' },
  b: { value: 'tete' },
  e: { value: 'titi' },
  f: { value: 'tutu' },
};

const merge = (arr1, arr2) => {
  const mergedKeys = [
    ...new Set([
      ...Object.keys(arr1),
      ...(Object.keys(arr2))
    ])
  ];

  const reducer = (acc, key) => ({
    ...acc,
    [key]: arr1[key] && arr2[key] && JSON.stringify(arr1[key]) !== JSON.stringify(arr2[key]) ?
      [
        arr1[key],
        arr2[key]
      ] :
      {
        ...arr1[key],
        ...arr2[key]
      }
    });

  return mergedKeys.reduce(reducer, {});
};

const mergedDictionnary = merge(foo, bar);
console.log(mergedDictionnary);

// Result
{
  a: { value: 'toto' },
  b: [{ value: 'tata' }, { value: 'tete' }],
  c: { value: 'titi' },
  d: { value: 'tutu' },
  e: { value: 'titi' },
  f: { value: 'tutu' }
}
```

### Serialising fetched data

> If the `reviver` returns `undefined` or no value, the property is deleted from the object.

```js
function reviver(key, value) {
  switch (key) {
    case 'comments':
      return value.map(comment => ({
        ...comment,
        author: `${comment.author.username} <${comment.author.email}>`
      }));
    case 'created_at':
    case 'updated_at':
      delete Object.assign(this, { [key.replace('_', '-')]: new Date(value) })[key];
    case 'firstname':
    case 'lastname':
    case 'requested_at':
    case 'published_at':
      return undefined;
    default:
      return value;
  }
}

// Blog post example
const blogPostData = JSON.stringify({
  "id": 123456,
  "title": "Random Article",
  "description": "A brief summary of what the article is dealing with",
  "content": "[...]",
  "comments": [
    {
      "author": {
      "username": "bob123",
      "email": "bob.doe@domain.com",
      "firstname": "Bob",
      "lastname": "Doe"
      },
      "title": "bla bla",
      "content": "[...]",
      "created_at": 1619802500521,
      "updated_at": 1619802500521
    }
  ],
  "created_at": 1619801719649,
  "updated_at": 1619801719649,
  "requested_at": 1619804176597,
  "published_at": 1619801719649
});

const presentationalData = JSON.parse(blogPostData, reviver);
console.log(presentationalData);

// Result
{
  "id": 123456,
  "title": "Random Article",
  "description": "A brief summary of what the article is dealing with",
  "content": "[...]",
  "comments": [
    {
      "author": "bob123 <bob.doe@domain.com>",
      "title": "bla bla",
      "content": "[...]",
      "created-at": "2021-04-30T17:08:20.521Z",
      "updated-at": "2021-04-30T17:08:20.521Z"
    }
  ],
  "created-at": "2021-04-30T16:55:19.649Z",
  "updated-at": "2021-04-30T16:55:19.649Z"
}
```
