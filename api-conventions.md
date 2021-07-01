# API conventions

## API definition

`POST /scope/object.action`

### Requests

1. Every endpoint HTTP method should be POST

    For browser cache you can use GET as an exception:

    `GET /scope/object.action?arg=value&foo.bar=deep`

1. **scope** is optional. It helps to group few methods by sense.

1. **object.action** - use only this order and only in camelCase

    - if there are few objects - it makes sense to use format `first/second.action`

1. All params should be passed as JSON object in body of request. There no need to use additional wrapper for fields (data/params)

    - Additional wrapper may be needed only for complex filters `{where: { card: 1 }, fields: ['name', 'author']}`

### Responses

1. Response has only 3 possible statuses:

    - **200** - for any success request (in business-logic context)
    - **400** - for any failed or mistaken request
    - **500** - for other crashes

1. **[200]** Success response should return JSON object. Even if there isn't any data then should be returned empty object

    Every response entity should have own property:
    - `{ user }` - "one user"
    - `{ users, cards }` - "users and theirs cards"

1. **[400]** Failed response should return error as field **code** with enum value (in snake_case)

    - Error **code** should describe situtaion explicitly (what and where problem was happened).

    - Server can send field **message** with more details about error;

    - Field **error** always should has *true* value

    ```js
    {
        "code": "already_saved", // "card_not_found", "no_access"
        "message": "Card has been already saved in user favorite collection",
        "error": true,
    }
    ```

1. **[500]** Infrastructure error alwasys should be in 5XX errors.
    - It may not have a body at all, or it may not have json
    - Cannot be parsed
    - Some requests that return 500 can be safely repeated once.
        - The server should not keep an invalid state on a 500 response (transactional)

## API usage

- Use API codegen by scheme for client and server sides.

- It's bad practice to copy API manually in code.

- For frontend - you should use generated effects for data fetching

## Further reading

- [API endpoints naming](https://gist.github.com/sergeysova/9f6dc24e21efe5f5bb4f9878216c2ce9)
- [OpenApi tools](https://github.com/openapi)
- [API scheme example](https://cardbox.github.io/backend/api-internal/index.html#/)
