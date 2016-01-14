# Rainforest Integrations

This is a small web service for outbound integrations from
Rainforest.

## API
`POST /events`
Rainforest will post events to this endpoint (see below for details
and the format).

`GET /events`
Return a list of schema for all supported events.

`GET /integrations/:key`
Return the schema of settings for a particular integration (see below
for the format).

`GET /integrations`
Return a list of schemas for all supported integrations.

## General flow
Rainforest will check the API endpoint for the required settings keys
for each integration. Every event will be posted to the `events`
endpoint (Rainforest will determine which events to post based on user
preferences). The payload for the `events` post should contain all the
information necessary to complete integration event, including all
authentication and user settings.

### `POST /events`
#### Request payload
Each post to `events` should include a JSON object in the request body
with the following keys:

- `event_type`: The event name key (see below for more details).
- `integrations`: A list of integrations to post the event to. Each
  integration object should have 2 keys: `key`, which is the key name
  of the integration (e.g. `"slack"`), and `settings`, which is an
  object with integration-specific settings (including all
  authentication settings).
- `payload`: The appropriate event data (event-specific, see below for
  the payload types for each event).

#### Response
`POST /events` will return a 201 response on success with a
`{"status": "ok"}` object. On failure due to a bad request, it will
return a 400 response and an object with two keys:

- `error`: A description of the error that that occurred.
- `type`: The type of failure (currently one of `invalid_request`,
  `unsupported_integration`, or `misconfigured_integration`).

### `GET /integrations`
#### Response
`GET /integrations` will return a list of schemas for all supported
integrations, each of which has the following keys:

- `key`: The unique key of the integration (e.g. `"slack"`).
- `title`: A human-readable title for the integration name
  (e.g. `"Slack"`).
- `settings`: A list of possible settings for the integration. Each
  setting has three keys: `key`, the key name for the setting;
  `title`, a human-readable title for the setting; and `required`, a
  boolean indicating whether the setting is required.

The response object for an individual integration follows the same
format.

## Event payload
Events come with a payload that is specific to the event. The
following events are currently supported, with the corresponding keys
in the payload:


#### `run_completion`
  
- **run:** id, environment, result, description, time_taken, total_tests, total_passed_tests, total_failed_tests, total_no_result_tests
  

#### `run_error`
  
- **run:** id, description, error_reason
  

#### `webhook_timeout`
  
- **run:** id, description, environment
  

#### `run_test_failure`
  
- **run:** id, description, environment
  
- **failed_test:** id, title, frontend_url
  
- **browser:** description
  



## Adding integrations
There are two steps to adding a new integration:

1. Add an integration class to the `lib/integrations` directory. This
   should inherit from the `Integrations::Base` class and should
   overwrite the `.key`, `#send_event` methods. If the integration post is
   unsuccessful, you should raise `Integrations::Error.new(type, message)`
   where `type` is either `user_configuration_error` or `misconfigured_integration`
   depending on the cause and `message` is a custom message to describe the error.
2. Edit `data/integrations.yml` to add your integration (including the
   appropriate values for `title` and `settings`).

## Local setup

1. Clone this repository
2. `bundle install`
3. You need to set the environment variable `INTEGRATION_SENTRY_DSN` (no default included due to the public nature of this project)
4. `rails s`

### For internal Rainforest developers

The main Rainforest app will try to call (by default, this is configurable) `http://integrations.dev` in development.

1. Install Pow if you don't have it already: `curl get.pow.cx | sh`
2. `ln -s ~/rainforest/rainforest-integration ~/.pow/integrations`


## Contributing
1. Fork it ( https://github.com/[my-github-username]/rainforest-integrations/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Generate this README by running `rake doc`
4. Commit your changes (git commit -am 'Add some feature')
5. Push to the branch (git push origin my-new-feature)
6. Create a new Pull Request
