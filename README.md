# partcycle-api-fixtures
Fixtures to be used in both client and server testing, to keep them in sync.

![graphic](https://image.ibb.co/dPKWkQ/api_validator_1.png "graphic")

## Summary

PartCycle API Fixtures provides example request/response pairs against which both the frontend and the backend can validate their own behavior. The idea is that by testing both the frontend and the backend against the same data, we minimize the probability of miscommunication between the two apps in production.

## Usage

Using these fixtures will require both inserting these fixtures into an existing codebase, and then using a library to integrate them with an existing framework.

- The fixtures can be added to a git repository using [submodules](https://git-scm.com/docs/git-submodule).
- Integration libraries:
  - [Rails](https://github.com/PartCycleTech/rails-api-validator)
  - [Ember](https://github.com/PartCycleTech/ember-api-validator)

## Specifics

The repository is a collection of JSON files. The contents of each file is a JSON object which serves as a single fixture.

The structure of each fixture is as follows:

- request
  - body
- response
  - status
  - body

`status` should represent an HTTP status code. An example status value would be `"404"`.
`body` can consist of any valid JSON.

An example fixture:

```
{
  "request": {
    "body": {
      "data": {
        "attributes": {
          "to-zip": "35630",
          "delivery-date": null
        },
        "relationships": {
          "inventory-item": {
            "data": {
              "id": "@id",
              "type": "inventory-items"
            }
          }
        },
        "type": "delivery-estimates"
      }
    }
  },
  "response": {
    "status": "202",
    "body": {
      "data": {
        "id": "@id",
        "attributes": {
          "delivery-date": "2017-06-23",
          "to-zip": "35630"
        },
        "type": "delivery-estimates"
      }
    }
  }
}
```

Ordering does not matter. You might notice the value `@id` in both the request and the response. This brings us to the next concept - flex params.

### Flex Params

There are some values which are best not hard-coded into the fixtures. Resource id's are an example. Hard-coding any specific id value might make writing the tests unnecessarily challenging. We don't care if the specific ids match, just that *some* valid id is used. This is where flex params come in.

To specify a flex param, just use the `@` symbol with the param name as the value in the fixture.

Currently supported flex params:
- `id` | any non-empty string

### File path conventions

It is recommended that the directories correspond to url parameters, and that the file name end with the type of request (e.g. `get`,`post` etc.). If there are multiple fixtures with the same url and request type, they can be differentiated with the first part of the file name. The file names should always have the `.json` extension.

Some examples:
- POST `explosion/floral/drifter` => `explosion/floral/drifter/post.json`
- GET `explosion/floral/drifter` => `explosion/floral/drifter/maximum-get.json`
- GET `explosion/floral/drifter` => `explosion/floral/drifter/pilot-get.json`
