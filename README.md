# refine-pocketbase
![pb](https://github.com/necatiozmen/refine-pocketbase/assets/18739364/4c5e6c43-42f3-4d7f-88c2-74970144308b)

[PocketBase](https://pocketbase.io/) providers for [Refine](https://refine.dev/).

## Installation

``` sh
yarn add refine-pocketbase
# or
npm install refine-pocketbase
```

## Data Provider

### Basic Usage

``` tsx
import PocketBase from "pocketbase";
import { authProvider, dataProvider, liveProvider } from "refine-pocketbase";

const pb = new PocketBase(POCKETBASE_URL);

<Refine
  authProvider={authProvider(pb)}
  dataProvider={dataProvider(pb)}
  liveProvider={liveProvider(pb)}
  ...
>
  ...
</Refine>
```

### The Meta Properties `fields` and `expand`

The code below uses `useList` to fetch a list of users. The resulting list contains user records with the `id`, `name`, `avatar` and the name of the organisation a user is assigned to. The meta properties `fields` and `expand` are used to customize the server response to the requirements of the user interface. 

``` ts
const users = useList({
  resource: "users",
  meta: {
    fields: ["id", "name", "avatar", "expand.org.name"],
    expand: ["org"],
  }
});
```

Here `fields` is an array of strings limiting the fields to return in the server's response. `expand` is an array with names of the related records that will be included in the response. Pocketbase supports up to 6-level depth nested relations expansion. See https://pocketbase.io/docs/api-records for more examples.

A couple of other refine hooks and components like `useOne`, `useTable`, `<Show/>`, `<List/>`, etc. will support the meta props `fields` and `expand` if used with the refine-pocketbase data provider.

## Auth Provider

A number of configuration properties are supported by the auth provider, primarily for controlling redirection following specific auth events. Please take a look at the self-explanatory names in the `AuthOptions` typescript interface to find the available options.
 
``` ts
import { authProvider, AuthOptions } from "refine-pocketbase";

const authOptions: AuthOptions = {
  loginRedirectTo: "/dashboard",
};

<Refine
  authProvider={authProvider(pb, authOptions)}
  ...
>
  ...
</Refine>
```

### Auth Collection 

`users` is the default auth collection in Pocketbase. Several auth collections can be supported in a single Pocketbase instance. You can use a different collection with the `authProvider` by using the `collection` property:

``` ts
const authOptions: AuthOptions = {
  collection: "superusers",
};
```

## Features

- [x] auth provider
  - [x] register
  - [x] login with password
  - [x] login with provider
  - [x] forgot password
  - [x] update password
- [x] data provider
  - [x] filters
  - [x] sorters
  - [x] pagination
  - [x] expand
  - [x] filter
- [x] live provider
  - [x] subscribe
  - [x] unsubscribe
- [ ] audit log provider

## Tasks: PRs Welcome!

- [ ] `auditLogProvider` implementation 
- [ ] happy path test specs
  - [x] `authProvider`
  - [x] `dataProvider` (except for `deleteOne`)
  - [x] `liveProvider`
  - [ ] `auditLogProvider`
- [ ] test specs for `authProvider` error conditions
  - [x] `register`
  - [x] `forgotPassword`
  - [x] `updatePassword`
  - [ ] `login`
- [ ] test specs for `dataProvider` error conditions
  - [ ] `getList`
  - [ ] `create`
  - [ ] `update`
  - [ ] `getOne`
  - [ ] `deleteOne`
- [ ] test specs for `deleteOne`
- [ ] test specs with `expand`
  - [ ] `getList`
  - [ ] `getOne`
- [ ] test specs with `fields`
  - [ ] `getList`
  - [ ] `getOne`
- [ ] test specs for `auditLogProvider` errors
- [ ] Setup Github Actions
  - [ ] test environment
  - [ ] build & publish

## Contribute

- leave a star
- report a bug
- open a pull request
- help others
- [buy me a coffee ☕](https://www.buymeacoffee.com/kruschid)

<a href="https://www.buymeacoffee.com/kruschid" target="_blank"><img width="200px" src="https://cdn.buymeacoffee.com/buttons/v2/default-orange.png" alt="Buy Me A Coffee" ></a>
