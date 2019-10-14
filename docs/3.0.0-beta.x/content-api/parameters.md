# Parameters

See the [parameters' concepts](../concepts/concepts.md#parameters) for details.

::: warning
By default, the filters can only be used from `find` endpoints generated by the Content Type Builder and the [CLI](../cli/CLI.md). If you need to implement a filters system somewhere else, read the [programmatic usage](../guides/parameters.md) section.
:::

## Available operators

The available operators are separated in four different categories:

- [Filters](#filters)
- [Sort](#sort)
- [Limit](#limit)
- [Start](#start)

## Filters

Filters are used as a suffix of a field name:

- Not suffix or `eq`: Equals
- `ne`: Not equals
- `lt`: Lower than
- `gt`: Greater than
- `lte`: Lower than or equal to
- `gte`: Greater than or equal to
- `in`: Included in an array of values
- `nin`: Isn't included in an array of values
- `contains`: Contains
- `ncontains`: Doesn't contain
- `containss`: Contains case sensitive
- `ncontainss`: Doesn't contain case sensitive
- `null`: Is null/Is not null

### Examples

#### Find users having `John` as first name.

`GET /users?firstName=John`
or
`GET /users?firstName_eq=John`

#### Find restaurants having a price equal or greater than `3`.

`GET /restaurants?price_gte=3`

#### Find multiple restaurant with id 3, 6, 8

`GET /restaurants?id_in=3&id_in=6&id_in=8`

#### Or clauses

If you use the same operator (except for in and nin) the values will be used to build an `OR` query

`GET /restaurants?name_contains=pizza&name_contains=giovanni`

## Deep filtering

Find restaurants owned by a chef who belongs to a restaurant with star equal to 5
`GET /restaurants?chef.restraurant.star=5`

::: warning
Querying your API with deep filters may cause performance issues.
If one of your deep filtering queries is too slow, we recommend building a custom route with an optimized version of your query.
:::

::: note
This feature doesn't allow you to filter nested models, e.g `Find users and only return their posts older than yesterday`.

To achieve this, there are two options:

- Either build a custom route or modify your services
- Use [GraphQL](../guides/graphql.md#query-api)
  :::

::: warning
This feature isn't available for the `upload` plugin.
:::

## Sort

Sort according to a specific field.

### Example

#### Sort users by email.

- ASC: `GET /users?_sort=email:ASC`
- DESC: `GET /users?_sort=email:DESC`

#### Sorting on multiple fileds

- `GET /users?_sort=email:asc,dateField:desc`
- `GET /users?_sort=email:DESC,username:ASC`

## Limit

Limit the size of the returned results.

### Example

#### Limit the result length to 30.

`GET /users?_limit=30`

You can require the full data set by passing a limit equal to `-1`

## Start

Skip a specific number of entries (especially useful for pagination).

### Example

#### Get the second page of results.

`GET /users?_start=10&_limit=10`