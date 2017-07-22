# partcycle-api-fixtures

## Summary

PartCycle API Fixtures provides example request/response pairs against which both the frontend and the backend can validate their own behavior. The idea is that by testing both the frontend and the backend against the same data, we minimize the probability of miscommunication between the two apps in production.

After all, our frontend tests and our backend tests mean little if the two apps are testing against different requirements. The goal of these fixtures is to keep the requirements in sync.

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
- `id`

## Usage

Using these fixtures will require both inserting these fixtures into an existing codebase, and then using a library to integrate them with an existing framework. The recommended way to add these fixtures to a repository is to use [git submodules](https://git-scm.com/docs/git-submodule).
