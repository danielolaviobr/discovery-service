## Specification

Implement a RESTful horizontally scalable discovery service using our backend framework [Node Framework](https://github.com/ubio/node-framework).

The idea is simple: a number of different client applications will periodically send heartbeats to this service, and the service keeps track of them, periodically removing those that didn't send any heartbeats in some configured time frame.

### Endpoints

The service should expose the following endpoints:

- `POST /:group/:id`

    - registers an application instance in specified `group`
    - `id` is a unique identifier of application instance, generated by client
    - if instance is already registered, the `updatedAt` timestamp must be updated
    - the request body can specify meta information that will be attached to the instance
    - returns JSON with following structure:

        ```js
        {
            "id": "e335175a-eace-4a74-b99c-c6466b6afadd",   // echoed from path parameter
            "group": "particle-detector",                   // echoed from path parameter
            "createdAt": 1571418096158,                     // first registered heartbeat
            "updatedAt": 1571418124127,                     // last registered heartbeat
            "meta": {                                       // meta echoed from request body
                "foo": 1
            }
        }
        ```

- `DELETE /:group/:id`

    - unregisters an application instance

- `GET /`

    - returns a JSON array containing a summary of all currently registered groups as follows:

        ```js
        [
            {
                "group": "particle-detector",
                "instances": "4",               // the number of registered instances in this group
                "createdAt": 1571418124127,     // first heartbeat registered in this group
                "lastUpdatedAt": 1571418124127, // last heartbeat registered in this group
            },
            // ...
        ]
        ```

    - groups containing 0 instances should not be returned

- `GET /:group`

    - returns a JSON array describing instances of the `group`:

        ```js
        [
            {
                "id": "e335175a-eace-4a74-b99c-c6466b6afadd",
                "group": "particle-detector",
                "createdAt": 1571418096158,
                "updatedAt": 1571418124127,
                "meta": {
                    "foo": 1
                }
            },
            // ...
        ]
        ```

The service should periodically remove expired instances. The "age" of the most recent heartbeat of an instance to be considered expired should be configurable with an environment variable and have a sensible default value.

## Our expectations

- A service should be implemented in Node.js, with README containing steps to get it up and running.
- TypeScript with `strict: true` is strongly encouraged, but not 100% required.
- Unless something is explicitly stated in Spec, please choose how to approach the specifics.
    - If some use cases are missing in Spec but important, please do cover them in your solution as well. Don't forget to document important implementation decisions.
    - Feel free to challenge the Spec if something does not add up, or can be done more elegantly and/or efficiently. We appreciate the ability to work with the requirements.
- Feel free to use the latest Node version (we don't like legacy either!)


## Submitting solution

- Create a repository on GitHub and/or BitBucket, push your code there and send us a link.
- If you choose to use a private repository, download your code as ZIP and attach it to the email.
- Optional: Deploying the solution to some public service like Glitch
- Optional: Showing a green CI page would also be beneficial
