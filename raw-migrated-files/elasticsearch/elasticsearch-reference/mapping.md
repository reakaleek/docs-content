# Mapping [mapping]

Mapping is the process of defining how a document, and the fields it contains, are stored and indexed.

Each document is a collection of fields, which each have their own [data type](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html). When mapping your data, you create a mapping definition, which contains a list of fields that are pertinent to the document. A mapping definition also includes [metadata fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-fields.html), like the `_source` field, which customize how a document’s associated metadata is handled.

Use *dynamic mapping* and *explicit mapping* to define your data. Each method provides different benefits based on where you are in your data journey. For example, explicitly map fields where you don’t want to use the defaults, or to gain greater control over which fields are created. You can then allow {{es}} to add other fields dynamically.

::::{note}
Before 7.0.0, the mapping definition included a type name. {{es}} 7.0.0 and later no longer accept a *default* mapping. See [*Removal of mapping types*](../../../manage-data/data-store/mapping/removal-of-mapping-types.md).
::::


::::{admonition} Experiment with mapping options
[Define runtime fields in a search request](../../../manage-data/data-store/mapping/define-runtime-fields-in-search-request.md) to experiment with different mapping options, and also fix mistakes in your index mapping values by overriding values in the mapping during the search request.

::::



## Dynamic mapping [mapping-dynamic]

When you use [dynamic mapping](../../../manage-data/data-store/mapping/dynamic-field-mapping.md), {{es}} automatically attempts to detect the data type of fields in your documents. This allows you to get started quickly by just adding data to an index. If you index additional documents with new fields, {{es}} will add these fields automatically. You can add fields to the top-level mapping, and to inner [`object`](https://www.elastic.co/guide/en/elasticsearch/reference/current/object.html) and [`nested`](https://www.elastic.co/guide/en/elasticsearch/reference/current/nested.html) fields.

Use [dynamic templates](../../../manage-data/data-store/mapping/dynamic-templates.md) to define custom mappings that are applied to dynamically added fields based on the matching condition.


## Explicit mapping [mapping-explicit]

Use [explicit mapping](../../../manage-data/data-store/mapping/explicit-mapping.md) to define exactly how data types are mapped to fields, customized to your specific use case.

Defining your own mappings enables you to:

* Define which string fields should be treated as full-text fields.
* Define which fields contain numbers, dates, or geolocations.
* Use data types that cannot be automatically detected (such as `geo_point` and `geo_shape`.)
* Choose date value [formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html), including custom date formats.
* Create custom rules to control the mapping for [dynamically added fields](../../../manage-data/data-store/mapping/dynamic-mapping.md).
* Optimize fields for partial matching.
* Perform language-specific text analysis.

::::{tip}
It’s often useful to index the same field in different ways for different purposes. For example, you might want to index a string field as both a text field for full-text search and as a keyword field for sorting or aggregating your data. Or, you might choose to use more than one language analyzer to process the contents of a string field that contains user input.

::::


Use [runtime fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-fields.html) to make schema changes without reindexing. You can use runtime fields in conjunction with indexed fields to balance resource usage and performance. Your index will be smaller, but with slower search performance.


## Managing and updating mappings [mapping-manage-update]

Explicit mappings should be defined at index creation for fields you know in advance. You can still add *new fields* to mappings at any time, as your data evolves.

Use the [Update mapping API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html) to update an existing mapping.

In most cases, you can’t change mappings for fields that are already mapped. These changes require [reindexing](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).

However, you can *update* mappings under certain conditions:

* You can add new fields to an existing mapping at any time, explicitly or dynamically.
* You can add new [multi-fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html) for existing fields.

    * Documents indexed before the mapping update will not have values for the new multi-fields until they are updated or reindexed. Documents indexed after the mapping change will automatically have values for the new multi-fields.

* Some [mapping parameters](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-params.html) can be updated for existing fields of certain [data types](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html).


## Prevent mapping explosions [mapping-limit-settings]

Defining too many fields in an index can lead to a mapping explosion, which can cause out of memory errors and difficult situations to recover from.

Consider a situation where every new document inserted introduces new fields, such as with [dynamic mapping](../../../manage-data/data-store/mapping/dynamic-mapping.md). Each new field is added to the index mapping, which can become a problem as the mapping grows.

Use the [mapping limit settings](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-settings-limit.html) to limit the number of field mappings (created manually or dynamically) and prevent documents from causing a mapping explosion.

