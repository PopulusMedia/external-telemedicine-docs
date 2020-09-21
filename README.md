# External telemedicine docs

> Documentation for Populus's telemedicine partner API

Populus maintains a REST API with endpoints available for our telemedicine partners for reporting purposes. With your API key, you may begin to make requests to our API.

- [What you will need](#what-you-will-need)
- [Usage](#usage)
  - [Stages](#stages)
    - [Base URLs](#base-urls)
  - [Endpoints](#endpoints)
    - [event](#event)
      - [Consult concluded](#consult-concluded)
      - [Consult canceled](#consult-canceled)
      - [RX:WRITTEN](#rx:written)
      - [RX:FULFILLED](#rx:fulfilled)

## What you will need

In order to communicate with our telemedicine API, you will need an API key.

Your API key is specific to your telemedicine platform. It should be kept secret and, as such, should not be exposed in any client-facing applications.

## Usage

Every request to our telemedicine API requires a standard set of parameters. Depending on the endpoint, additional parameters may be required (discussed in the [Endpoints](#endpoints) section).

For each request, provide the following parameters in the body of the request:

```javascript
{
  master_id: string; // Master ID of user, which you received from us via our API request(s) to you
  engagement_id: string; // Engagement ID of user, which you received from us via our API request(s) to you
}
```

## Stages

Our telemedicine API exists in two stages: `production` and `development`.

### Base URLs

For staging, the base URL is:

`https://telemedicine.populus-media.net/dev/external`

For production, the base URL is:

`https://telemedicine.populus-media.net/prod/external`

## Endpoints

Endpoints are available for consumption by our telemedicine partners.

All endpoints require your API key to be passed as a header:

`apikey: YOUR_API_KEY`

### event

Each request to the `BASE_URL/event` endpoint requires the following in the body of the request:

```javascript
{
  master_id: string; // Master ID of user, which you received from us via our API request(s) to you
  engagement_id: string; // Engagement ID of user, which you received from us via our API request(s) to you
  event_name: string; // Predefined event name, available below
}
```

For `event_name`, the following events are allowed:

```
  TELEMEDICINE:CONSULT_CONCLUDED
  TELEMEDICINE:CONSULT_CANCELED
  RX:WRITTEN
  RX:FULFILLED
```

### Consult concluded

The `TELEMEDICINE:CONSULT_CONCLUDED` event should be called whenever a consult has been completed.

- **URL**

  BASE_URL/external/event

- **Method**

  `POST`

- **Headers**

  `apikey: YOUR_API_KEY`

- **URL Params**

  **Required:**

  `master_id: String`

  `engagement_id: String`

  `event_name: String` Pass the event name: TELEMEDICINE:CONSULT_CONCLUDED

- **Sample Call:**

  ```
  curl --location --request POST 'https://telemedicine.populus-media.net/dev/external/event' \
    --header 'apikey: YOUR_API_KEY' \
    --data-raw '{
        "master_id": "abcd-1234",
        "engagement_id": "efgh-5678",
        "event_name": "TELEMEDICINE:CONSULT_CONCLUDED"
    }'
  ```

### Consult canceled

The `TELEMEDICINE:CONSULT_CANCELED` event should be called whenever a consult has been canceled.

- **URL**

  BASE_URL/external/event

- **Method**

  `POST`

- **Headers**

  `apikey: YOUR_API_KEY`

- **URL Params**

  **Required:**

  `master_id: String`

  `engagement_id: String`

  `event_name: String` Pass the event name: TELEMEDICINE:CONSULT_CANCELED

- **Sample Call:**

  ```
  curl --location --request POST 'https://telemedicine.populus-media.net/dev/external/event' \
    --header 'apikey: YOUR_API_KEY' \
    --data-raw '{
        "master_id": "abcd-1234",
        "engagement_id": "efgh-5678",
        "event_name": "TELEMEDICINE:CONSULT_CANCELED"
    }'
  ```

### RX:WRITTEN

The `RX:WRITTEN` event should be called whenever a prescription is written by a health care provider.

- **URL**

  BASE_URL/external/event

- **Method**

  `POST`

- **Headers**

  `apikey: YOUR_API_KEY`

- **URL Params**

  **Required:**

  `master_id: String`

  `engagement_id: String`

  `event_name: String` Pass the event name: TELEMEDICINE:CONSULT_CANCELED

  `rx_name: String`

  **Optional:**

  `pharmacy_name: String`

  `dosage: String`

  `num_refills: Integer`

- **Sample Call:**

  ```
  curl --location --request POST 'https://telemedicine.populus-media.net/dev/external/event' \
    --header 'apikey: YOUR_API_KEY' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "master_id": "abcd-1234",
        "engagement_id": "efgh-5678",
        "event_name": "RX:WRITTEN",
        "rx_name": "Zetrello",
        "pharmacy_name": "KnippeRx",
        "dosage": "One per day",
        "num_refills": 2
    }'
  ```

### RX:FULFILLED

The `RX:FULFILLED` event should be called whenever a prescription is fulfilled by a pharmacy.

- **URL**

  BASE_URL/external/event

- **Method**

  `POST`

- **Headers**

  `apikey: YOUR_API_KEY`

- **URL Params**

  **Required:**

  `master_id: String`

  `engagement_id: String`

  `event_name: String` Pass the event name: TELEMEDICINE:CONSULT_CANCELED

  `rx_name: String`

  **Optional:**

  `pharmacy_name: String`

  `dosage: String`

  `num_refills: Integer`

- **Sample Call:**

  ```
  curl --location --request POST 'https://telemedicine.populus-media.net/dev/external/event' \
    --header 'apikey: YOUR_API_KEY' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "master_id": "abcd-1234",
        "engagement_id": "efgh-5678",
        "event_name": "RX:FULFILLED",
        "rx_name": "Zetrello",
        "pharmacy_name": "KnippeRx",
        "dosage": "One per day",
        "num_refills": 2
    }'
  ```
