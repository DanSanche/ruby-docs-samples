# Ruby Google Cloud Tasks sample for Google App Engine

This sample application shows how to use [Google Cloud Tasks](https://cloud.google.com/cloud-tasks/).

`create_http_task.rb` is a simple command-line program to create tasks with an
HTTP target.

## Setup

Before you can run or deploy the sample, you need to do the following:

1.  Refer to the [appengine/README.md](readme) file for instructions on
    running and deploying.
1.  Enable the Cloud Tasks API in the [Google Cloud Console](https://console.cloud.google.com/apis/api/tasks.googleapis.com).
1.  Set up [Google Application Credentials](https://cloud.google.com/docs/authentication/getting-started).
1.  Install dependencies:
    ```
    bundle install
    ```

## Creating a queue

To create a queue using the Cloud SDK, use the following gcloud command:

    gcloud beta tasks queues create-app-engine-queue my-appengine-queue

Note: A newly created queue will route to the default App Engine service and
version unless configured to do otherwise.

## Deploying the app to App Engine flexible environment

Deploy the App Engine app with gcloud:

    gcloud app deploy app.yaml

Verify the index page is serving:

    gcloud app browse

## Run the Sample Using the Command Line

Set environment variables:

First, your project ID:

```
export GOOGLE_CLOUD_PROJECT=my-project-id
```

Then the queue ID, as specified at queue creation time. Queue IDs already
created can be listed with `gcloud beta tasks queues list`.

```
export QUEUE_ID=my-appengine-queue
```

And finally the location ID, which can be discovered with
`gcloud beta tasks queues describe $QUEUE_ID`, with the location embedded in
the "name" value (for instance, if the name is
"projects/my-project/locations/us-central1/queues/my-appengine-queue", then the
location is "us-central1").

```
export LOCATION_ID=us-central1
```

### Using HTTP Push Queues
Set an environment variable for the endpoint to your task handler. This is an
example url to send requests to the App Engine task handler:

```
export URL=https://${GOOGLE_CLOUD_PROJECT}.appspot.com/log_payload
```
Running the sample will create a task and send the task to the specific URL
endpoint, with a payload specified:

```
ruby create_http_task.rb $LOCATION_ID $QUEUE_ID $URL
```

[appengine-flex]: https://cloud.google.com/appengine/docs/flexible/ruby
