<p align="center">
  <img src="https://raw.githubusercontent.com/meilisearch/integration-guides/main/assets/logos/meilisearch_js.svg" alt="Meilisearch-JavaScript" width="200" height="200" />
</p>

<h1 align="center">Meilisearch JavaScript</h1>

<h4 align="center">
  <a href="https://github.com/meilisearch/meilisearch">Meilisearch</a> |
  <a href="https://docs.meilisearch.com">Documentation</a> |
  <a href="https://slack.meilisearch.com">Slack</a> |
  <a href="https://roadmap.meilisearch.com/tabs/1-under-consideration">Roadmap</a> |
  <a href="https://www.meilisearch.com">Website</a> |
  <a href="https://docs.meilisearch.com/faq">FAQ</a>
</h4>

<p align="center">
  <a href="https://www.npmjs.com/package/meilisearch"><img src="https://img.shields.io/npm/v/meilisearch.svg" alt="npm version"></a>
  <a href="https://github.com/meilisearch/meilisearch-js/actions"><img src="https://github.com/meilisearch/meilisearch-js/workflows/Tests/badge.svg" alt="Tests"></a>
  <a href="https://github.com/prettier/prettier"><img src="https://img.shields.io/badge/styled_with-prettier-ff69b4.svg" alt="Prettier"></a>
  <a href="https://github.com/meilisearch/meilisearch-js/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-informational" alt="License"></a>
  <a href="https://ms-bors.herokuapp.com/repositories/10"><img src="https://bors.tech/images/badge_small.svg" alt="Bors enabled"></a>
</p>

<p align="center">⚡ The Meilisearch API client written for JavaScript</p>

**Meilisearch JavaScript** is the Meilisearch API client for JavaScript developers.

**Meilisearch** is an open-source search engine. [Learn more about Meilisearch.](https://github.com/meilisearch/meilisearch)

## Table of Contents <!-- omit in toc -->

- [📖 Documentation](#-documentation)
- [🔧 Installation](#-installation)
- [🎬 Getting started](#-getting-started)
- [🤖 Compatibility with Meilisearch](#-compatibility-with-meilisearch)
- [💡 Learn more](#-learn-more)
- [⚙️ Contributing](#️-contributing)
- [📜 API resources](#-api-resources)

## 📖 Documentation

This readme contains all the documentation you need to start using this Meilisearch SDK.

For general information on how to use Meilisearch—such as our API reference, tutorials, guides, and in-depth articles—refer to our [main documentation website](https://docs.meilisearch.com/).

## 🔧 Installation

We recommend installing `meilisearch-js` in your project with your package manager of choice.

If you use `npm`:

```sh
npm install meilisearch
```

If you prefer `yarn`:

```sh
yarn add meilisearch
```

`meilisearch-js` officially supports `node` versions >= 12 and <= 16.

Instead of using a package manager, you may also import the library directly into your [HTML via a CDN](#include-script-tag).

### Run Meilisearch <!-- omit in toc -->

To use one our SDKS, you must first have a running Meilisearch instance. Consult our documentation for [instructions on how to download and launch Meilisearch](https://docs.meilisearch.com/reference/features/installation.html#download-and-launch).

### Import <!-- omit in toc -->

After installing `meilisearch-js`, you must import it into your application. There are many ways of doing that depending on your development environment.

#### `import` syntax <!-- omit in toc -->

Usage in an ES module environment:

```javascript
import { MeiliSearch } from 'meilisearch'

const client = new MeiliSearch({
  host: 'http://127.0.0.1:7700',
  apiKey: 'masterKey',
})
```

#### `<script>` tag <!-- omit in toc -->

Usage in an HTML (or alike) file:

```html
<script src='https://cdn.jsdelivr.net/npm/meilisearch@latest/dist/bundles/meilisearch.umd.js'></script>
<script>
  const client = new MeiliSearch({
    host: 'http://127.0.0.1:7700',
    apiKey: 'masterKey',
  })
</script>
```

#### `require` syntax <!-- omit in toc -->

Usage in a back-end node.js or another environment supporting CommonJS modules:

```javascript
const { MeiliSearch } = require('meilisearch')

const client = new MeiliSearch({
  host: 'http://127.0.0.1:7700',
  apiKey: 'masterKey',
})
```

#### React Native <!-- omit in toc -->

To use `meilisearch-js` with React Native, you must also install [react-native-url-polyfill](https://www.npmjs.com/package/react-native-url-polyfill).

#### Deno<!-- omit in toc -->

Usage in a Deno environment:

```js
import { MeiliSearch } from "https://esm.sh/meilisearch"

const client = new MeiliSearch({
  host: 'http://127.0.0.1:7700',
  apiKey: 'masterKey',
})
```

## 🎬 Getting started

### Add documents <!-- omit in toc -->

```js
const { MeiliSearch } = require('meilisearch')
// Or if you are in a ES environment
import { MeiliSearch } from 'meilisearch'

;(async () => {
  const client = new MeiliSearch({
    host: 'http://127.0.0.1:7700',
    apiKey: 'masterKey',
  })

  // An index is where the documents are stored.
  const index = client.index('movies')

  const documents = [
      { id: 1, title: 'Carol', genres: ['Romance', 'Drama'] },
      { id: 2, title: 'Wonder Woman', genres: ['Action', 'Adventure'] },
      { id: 3, title: 'Life of Pi', genres: ['Adventure', 'Drama'] },
      { id: 4, title: 'Mad Max: Fury Road', genres: ['Adventure', 'Science Fiction'] },
      { id: 5, title: 'Moana', genres: ['Fantasy', 'Action']},
      { id: 6, title: 'Philadelphia', genres: ['Drama'] },
  ]

  // If the index 'movies' does not exist, Meilisearch creates it when you first add the documents.
  let response = await index.addDocuments(documents)

  console.log(response) // => { "uid": 0 }
})()
```

Tasks such as document addition always return a unique identifier. You can use this identifier `uid` to check the status (`enqueued`, `processing`, `succeeded` or `failed`) of a [task](https://docs.meilisearch.com/reference/api/tasks.html#get-task).

### Basic search <!-- omit in toc -->

```javascript
// Meilisearch is typo-tolerant:
const search = await index.search('philoudelphia')
console.log(search)
```

Output:

```json
{
  "hits": [
    {
      "id": "6",
      "title": "Philadelphia",
      "genres": ["Drama"]
    }
  ],
  "offset": 0,
  "limit": 20,
  "nbHits": 1,
  "processingTimeMs": 1,
  "query": "philoudelphia"
}
```

### Using search parameters <!-- omit in toc -->

`meilisearch-js` supports all [search parameters](https://docs.meilisearch.com/reference/features/search_parameters.html) described in our main documentation website.

```javascript
await index.search(
  'wonder',
  {
    attributesToHighlight: ['*']
  }
)
```

```json
{
  "hits": [
    {
      "id": 2,
      "title": "Wonder Woman",
      "genres": ["Action", "Adventure"],
      "_formatted": {
        "id": 2,
        "title": "<em>Wonder</em> Woman",
        "genres": ["Action", "Adventure"]
      }
    }
  ],
  "offset": 0,
  "limit": 20,
  "nbHits": 1,
  "processingTimeMs": 0,
  "query": "wonder"
}
```

### Custom search with filters <!-- omit in toc -->

To enable filtering, you must first add your attributes to the [`filterableAttributes` index setting](https://docs.meilisearch.com/reference/api/filterable_attributes.html).

```js
await index.updateAttributesForFaceting([
    'id',
    'genres'
  ])
```

You only need to perform this operation once per index.

Note that Meilisearch rebuilds your index whenever you update `filterableAttributes`. Depending on the size of your dataset, this might take considerable time. You can track the process using the [tasks API](https://docs.meilisearch.com/reference/api/tasks.html#get-task)).

After you configured `filterableAttributes`, you can use the [`filter` search parameter](https://docs.meilisearch.com/reference/api/search.html#filter) to refine your search:

```js
await index.search(
  'wonder',
  {
    filter: ['id > 1 AND genres = Action']
  }
)
```

```json
{
  "hits": [
    {
      "id": 2,
      "title": "Wonder Woman",
      "genres": ["Action","Adventure"]
    }
  ],
  "offset": 0,
  "limit": 20,
  "nbHits": 1,
  "processingTimeMs": 0,
  "query": "wonder"
}
```

### Placeholder search <!-- omit in toc -->

Placeholder search makes it possible to receive hits based on your parameters without having any query (`q`). For example, in a movies database you can run an empty query to receive all results filtered by `genre`.

```javascript
await index.search(
  '',
  {
    filter: ['genres = fantasy'],
    facetsDistribution: ['genres']
  }
)
```

```json
{
  "hits": [
    {
      "id": 2,
      "title": "Wonder Woman",
      "genres": ["Action","Adventure"]
    },
    {
      "id": 5,
      "title": "Moana",
      "genres": ["Fantasy","Action"]
    }
  ],
  "offset": 0,
  "limit": 20,
  "nbHits": 2,
  "processingTimeMs": 0,
  "query": "",
  "facetsDistribution": {
    "genres": {
      "Action": 2,
      "Fantasy": 1,
      "Adventure": 1
    }
  }
}
```

Note that to enable faceted search on your dataset you need to add `genres` to the `filterableAttributes` index setting. For more information on filtering and faceting, [consult our documentation settings](https://docs.meilisearch.com/reference/features/faceted_search.html#setting-up-facets).

#### Abortable search <!-- omit in toc -->

You can abort a pending search request by providing an [AbortSignal](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal) to the request.

```js
const controller = new AbortController()

index
  .search('wonder', {}, {
    signal: controller.signal,
  })
  .then((response) => {
    /** ... */
  })
  .catch((e) => {
    /** Catch AbortError here. */
  })

controller.abort()
```

## 🤖 Compatibility with Meilisearch

This package only guarantees the compatibility with the [version v0.27.0 of Meilisearch](https://github.com/meilisearch/meilisearch/releases/tag/v0.27.0).

## 💡 Learn more

The following sections in our main documentation website may interest you:

- **Manipulate documents**: see the [API references](https://docs.meilisearch.com/reference/api/documents.html) or read more about [documents](https://docs.meilisearch.com/learn/core_concepts/documents.html).
- **Search**: see the [API references](https://docs.meilisearch.com/reference/api/search.html) or follow our guide on [search parameters](https://docs.meilisearch.com/reference/features/search_parameters.html).
- **Manage the indexes**: see the [API references](https://docs.meilisearch.com/reference/api/indexes.html) or read more about [indexes](https://docs.meilisearch.com/learn/core_concepts/indexes.html).
- **Configure the index settings**: see the [API references](https://docs.meilisearch.com/reference/api/settings.html) or follow our guide on [settings parameters](https://docs.meilisearch.com/reference/features/settings.html).

This repository also contains [more examples](./examples).

## ⚙️ Contributing

We welcome all contributions, big and small! If you want to know more about this SDK's development workflow or want to contribute to the repo, please visit our [contributing guidelines](/CONTRIBUTING.md) for detailed instructions.

## 📜 API resources

### Search <!-- omit in toc -->

#### [Make a search request](https://docs.meilisearch.com/reference/api/search.html)

```ts
client.index<T>('xxx').search(query: string, options: SearchParams = {}, config?: Partial<Request>): Promise<SearchResponse<T>>
```

#### [Make a search request using the GET method (slower than the search method)](https://docs.meilisearch.com/reference/api/search.html#search-in-an-index-with-get-route)

```ts
client.index<T>('xxx').searchGet(query: string, options: SearchParams = {}, config?: Partial<Request>): Promise<SearchResponse<T>>
```

### Documents <!-- omit in toc -->

#### [Add or replace multiple documents](https://docs.meilisearch.com/reference/api/documents.html#add-or-replace-documents)

```ts
client.index.addDocuments(documents: Document<T>[]): Promise<EnqueuedTask>
```

#### [Add or replace multiple documents in batches](https://docs.meilisearch.com/reference/api/documents.html#add-or-replace-documents)

```ts
client.index.addDocumentsInBatches(documents: Document<T>[], batchSize = 1000): Promise<EnqueuedTask[]>
```

#### [Add or update multiple documents](https://docs.meilisearch.com/reference/api/documents.html#add-or-update-documents)

```ts
client.index.updateDocuments(documents: Array<Document<Partial<T>>>): Promise<EnqueuedTask>
```

#### [Add or update multiple documents in batches](https://docs.meilisearch.com/reference/api/documents.html#add-or-update-documents)

```ts
client.index.updateDocumentsInBatches(documents: Array<Document<Partial<T>>>, batchSize = 1000): Promise<EnqueuedTask[]>
```

#### [Get Documents](https://docs.meilisearch.com/reference/api/documents.html#get-documents)

```ts
client.index.getDocuments(params: getDocumentsParams): Promise<Document<T>[]>
```

#### [Get one document](https://docs.meilisearch.com/reference/api/documents.html#get-one-document)

```ts
client.index.getDocument(documentId: string): Promise<Document<T>>
```

#### [Delete one document](https://docs.meilisearch.com/reference/api/documents.html#delete-one-document)

```ts
client.index.deleteDocument(documentId: string | number): Promise<EnqueuedTask>
```

#### [Delete multiple documents](https://docs.meilisearch.com/reference/api/documents.html#delete-documents)

```ts
client.index.deleteDocuments(documentsIds: string[] | number[]): Promise<EnqueuedTask>
```

#### [Delete all documents](https://docs.meilisearch.com/reference/api/documents.html#delete-all-documents)

```ts
client.index.deleteAllDocuments(): Promise<Types.EnqueuedTask>
```

### Tasks <!-- omit in toc -->

#### [Get task info using the client](https://docs.meilisearch.com/reference/api/tasks.html#get-all-tasks)

##### Task list
```ts
client.getTasks(): Promise<Result<Task[]>>
```

##### One task
```ts
client.getTask(uid: number): Promise<Task>
```

#### [Get task info using the index](https://docs.meilisearch.com/reference/api/tasks.html#get-all-tasks-by-index)

##### Task list

```ts
client.index.getTasks(): Promise<Result<Task[]>>
```

##### One task

```ts
client.index.getTask(uid: number): Promise<Task>
```

#### Wait for one task

##### Using the client

```ts
client.waitForTask(uid: number, { timeOutMs?: number, intervalMs?: number }): Promise<Task>
```

##### Using the index

```ts
client.index.waitForTask(uid: number, { timeOutMs?: number, intervalMs?: number }): Promise<Task>
```

#### Wait for multiple tasks

##### Using the client

```ts
client.waitForTasks(uids: number[], { timeOutMs?: number, intervalMs?: number }): Promise<Result<Task[]>>
```

##### Using the index

```ts
client.index.waitForTasks(uids: number[], { timeOutMs?: number, intervalMs?: number }): Promise<Result<Task[]>>
```

### Indexes <!-- omit in toc -->

#### [Get all indexes in Index instances](https://docs.meilisearch.com/reference/api/indexes.html#list-all-indexes)

```ts
client.getIndexes(): Promise<Index[]>
```

#### [Get raw indexes in JSON response from Meilisearch](https://docs.meilisearch.com/reference/api/indexes.html#list-all-indexes)

```ts
client.getRawIndexes(): Promise<IndexResponse[]>
```

#### [Create a new index](https://docs.meilisearch.com/reference/api/indexes.html#create-an-index)

```ts
client.createIndex<T>(uid: string, options?: IndexOptions): Promise<EnqueuedTask>
```

#### Create a local reference to an index

```ts
client.index<T>(uid: string): Index<T>
```

#### [Get an index instance completed with information fetched from Meilisearch](https://docs.meilisearch.com/reference/api/indexes.html#get-one-index)

```ts
client.getIndex<T>(uid: string): Promise<Index<T>>
```

#### [Get the raw index JSON response from Meilisearch](https://docs.meilisearch.com/reference/api/indexes.html#get-one-index)

```ts
client.getRawIndex(uid: string): Promise<IndexResponse>
```

#### [Get an object with information about the index](https://docs.meilisearch.com/reference/api/indexes.html#get-one-index)

```ts
client.index.getRawInfo(): Promise<IndexResponse>
```

#### [Update Index](https://docs.meilisearch.com/reference/api/indexes.html#update-an-index)

##### Using the client

```ts
client.updateIndex(uid: string, options: IndexOptions): Promise<EnqueuedTask>
```

##### Using the index object

```ts
client.index.update(data: IndexOptions): Promise<EnqueuedTask>
```

#### [Delete index](https://docs.meilisearch.com/reference/api/indexes.html#delete-an-index)

##### Using the client
```ts
client.deleteIndex(uid): Promise<void>
```

##### Using the index object
```ts
client.index.delete(): Promise<void>
```

#### [Get specific index stats](https://docs.meilisearch.com/reference/api/stats.html#get-stat-of-an-index)

```ts
client.index.getStats(): Promise<IndexStats>
```

##### Return Index instance with updated information

```ts
client.index.fetchInfo(): Promise<Index>
```

##### Get Primary Key of an Index

```ts
client.index.fetchPrimaryKey(): Promise<string | undefined>
```

### Settings <!-- omit in toc -->

#### [Get settings](https://docs.meilisearch.com/reference/api/settings.html#get-settings)

```ts
client.index.getSettings(): Promise<Settings>
```

#### [Update settings](https://docs.meilisearch.com/reference/api/settings.html#update-settings)

```ts
client.index.updateSettings(settings: Settings): Promise<EnqueuedTask>
```

#### [Reset settings](https://docs.meilisearch.com/reference/api/settings.html#reset-settings)

```ts
client.index.resetSettings(): Promise<EnqueuedTask>
```

### Synonyms <!-- omit in toc -->

#### [Get synonyms](https://docs.meilisearch.com/reference/api/synonyms.html#get-synonyms)

```ts
client.index.getSynonyms(): Promise<object>
```

#### [Update synonyms](https://docs.meilisearch.com/reference/api/synonyms.html#update-synonyms)

```ts
client.index.updateSynonyms(synonyms: Synonyms): Promise<EnqueuedTask>
```

#### [Reset synonyms](https://docs.meilisearch.com/reference/api/synonyms.html#reset-synonyms)

```ts
client.index.resetSynonyms(): Promise<EnqueuedTask>
```

### Stop-words <!-- omit in toc -->

#### [Get stop-words](https://docs.meilisearch.com/reference/api/stop_words.html#get-stop-words)

```ts
client.index.getStopWords(): Promise<string[]>
```

#### [Update stop-words](https://docs.meilisearch.com/reference/api/stop_words.html#update-stop-words)

```ts
client.index.updateStopWords(stopWords: string[] | null ): Promise<EnqueuedTask>
```

#### [Reset stop-words](https://docs.meilisearch.com/reference/api/stop_words.html#reset-stop-words)

```ts
client.index.resetStopWords(): Promise<EnqueuedTask>
```

### Ranking rules <!-- omit in toc -->

#### [Get ranking rules](https://docs.meilisearch.com/reference/api/ranking_rules.html#get-ranking-rules)

```ts
client.index.getRankingRules(): Promise<string[]>
```

#### [Update ranking rules](https://docs.meilisearch.com/reference/api/ranking_rules.html#update-ranking-rules)

```ts
client.index.updateRankingRules(rankingRules: string[] | null): Promise<EnqueuedTask>
```

#### [Reset ranking rules](https://docs.meilisearch.com/reference/api/ranking_rules.html#reset-ranking-rules)

```ts
client.index.resetRankingRules(): Promise<EnqueuedTask>
```

### Distinct Attribute <!-- omit in toc -->

#### [Get distinct attribute](https://docs.meilisearch.com/reference/api/distinct_attribute.html#get-distinct-attribute)

```ts
client.index.getDistinctAttribute(): Promise<string | void>
```

#### [Update distinct attribute](https://docs.meilisearch.com/reference/api/distinct_attribute.html#update-distinct-attribute)

```ts
client.index.updateDistinctAttribute(distinctAttribute: string | null): Promise<EnqueuedTask>
```

#### [Reset distinct attribute](https://docs.meilisearch.com/reference/api/distinct_attribute.html#reset-distinct-attribute)

```ts
client.index.resetDistinctAttribute(): Promise<EnqueuedTask>
```

### Searchable attributes <!-- omit in toc -->

#### [Get searchable attributes](https://docs.meilisearch.com/reference/api/searchable_attributes.html#get-searchable-attributes)

```ts
client.index.getSearchableAttributes(): Promise<string[]>
```

#### [Update searchable attributes](https://docs.meilisearch.com/reference/api/searchable_attributes.html#update-searchable-attributes)

```ts
client.index.updateSearchableAttributes(searchableAttributes: string[] | null): Promise<EnqueuedTask>
```

#### [Reset searchable attributes](https://docs.meilisearch.com/reference/api/searchable_attributes.html#reset-searchable-attributes)

```ts
client.index.resetSearchableAttributes(): Promise<EnqueuedTask>
```

### Displayed attributes <!-- omit in toc -->

#### [Get displayed attributes](https://docs.meilisearch.com/reference/api/displayed_attributes.html#get-displayed-attributes)

```ts
client.index.getDisplayedAttributes(): Promise<string[]>
```

#### [Update displayed attributes](https://docs.meilisearch.com/reference/api/displayed_attributes.html#update-displayed-attributes)

```ts
client.index.updateDisplayedAttributes(displayedAttributes: string[] | null): Promise<EnqueuedTask>
```

#### [Reset displayed attributes](https://docs.meilisearch.com/reference/api/displayed_attributes.html#reset-displayed-attributes)

```ts
client.index.resetDisplayedAttributes(): Promise<EnqueuedTask>
```

### Filterable attributes <!-- omit in toc -->

#### [Get filterable attributes](https://docs.meilisearch.com/reference/api/filterable_attributes.html#get-filterable-attributes)

```ts
client.index.getFilterableAttributes(): Promise<string[]>
```

#### [Update filterable attributes](https://docs.meilisearch.com/reference/api/filterable_attributes.html#update-filterable-attributes)

```ts
client.index.updateFilterableAttributes(filterableAttributes: string[] | null): Promise<EnqueuedTask>
```

#### [Reset filterable attributes](https://docs.meilisearch.com/reference/api/filterable_attributes.html#reset-filterable-attributes)

```ts
client.index.resetFilterableAttributes(): Promise<EnqueuedTask>
```

### Sortable attributes <!-- omit in toc -->

#### [Get sortable attributes](https://docs.meilisearch.com/reference/api/sortable_attributes.html#get-sortable-attributes)

```ts
client.index.getSortableAttributes(): Promise<string[]>
```

#### [Update sortable attributes](https://docs.meilisearch.com/reference/api/sortable_attributes.html#update-sortable-attributes)

```ts
client.index.updateSortableAttributes(sortableAttributes: string[] | null): Promise<EnqueuedTask>
```

#### [Reset sortable attributes](https://docs.meilisearch.com/reference/api/sortable_attributes.html#reset-sortable-attributes)

```ts
client.index.resetSortableAttributes(): Promise<EnqueuedTask>
```

### Typo tolerance <!-- omit in toc -->

#### [Get typo tolerance](https://docs.meilisearch.com/reference/api/typo_tolerance.html#get-typo-tolerance)

```ts
client.index.getTypoTolerance(): Promise<TypoTolerance>
```

#### [Update typo tolerance](https://docs.meilisearch.com/reference/api/typo_tolerance.html#update-typo-tolerance)

```ts
client.index.updateTypoTolerance(typoTolerance: TypoTolerance | null): Promise<EnqueuedTask>
```

#### [Reset typo tolerance](https://docs.meilisearch.com/reference/api/typo_tolerance.html#reset-typo-tolerance)

```ts
client.index.resetTypoTolerance(): Promise<EnqueuedTask>
```

### Keys <!-- omit in toc -->

#### [Get keys](https://docs.meilisearch.com/reference/api/keys.html#get-all-keys)

```ts
client.getKeys(): Promise<Result<Key[]>>
```

#### [Get one key](https://docs.meilisearch.com/reference/api/keys.html#get-one-key)

```ts
client.getKey(key: string): Promise<Key>
```

#### [Create a key](https://docs.meilisearch.com/reference/api/keys.html#create-a-key)

```ts
client.createKey(options: KeyPayload): Promise<Key>
```

#### [Update a key](https://docs.meilisearch.com/reference/api/keys.html#update-a-key)

```ts
client.updateKey(key: string, options: KeyPayload): Promise<Key>
```

#### [Delete a key](https://docs.meilisearch.com/reference/api/keys.html#delete-a-key)

```ts
client.deleteKey(key: string): Promise<void>
```

### `isHealthy` <!-- omit in toc -->

#### [Return `true` or `false` depending on the health of the server](https://docs.meilisearch.com/reference/api/health.html#get-health)

```ts
client.isHealthy(): Promise<boolean>
```

### Health <!-- omit in toc -->

#### [Check if the server is healthy](https://docs.meilisearch.com/reference/api/health.html#get-health)

```ts
client.health(): Promise<Health>
```

### Stats <!-- omit in toc -->

#### [Get database stats](https://docs.meilisearch.com/reference/api/stats.html#get-stats-of-all-indexes)

```ts
client.getStats(): Promise<Stats>
```

### Version <!-- omit in toc -->

#### [Get binary version](https://docs.meilisearch.com/reference/api/version.html#get-version-of-meilisearch)

```ts
client.getVersion(): Promise<Version>
```

### Dumps <!-- omit in toc -->

#### [Trigger a dump creation process](https://docs.meilisearch.com/reference/api/dump.html#create-a-dump)

```ts
client.createDump(): Promise<Types.EnqueuedDump>
```

#### [Get the status of a dump creation process](https://docs.meilisearch.com/reference/api/dump.html#get-dump-status)

```ts
client.getDumpStatus(dumpUid: string): Promise<Types.EnqueuedDump>
```

---

Meilisearch provides and maintains many SDKs and integration tools like this one. We want to provide everyone with an **amazing search experience for any kind of project**. For a full overview of everything we create and maintain, take a look at the [integration-guides](https://github.com/meilisearch/integration-guides) repository.
