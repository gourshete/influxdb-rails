# Ruby On Rails Performance Dashboard

A dashboard providing Ruby on Rails performance insights based on
[Free Software](https://www.fsf.org/about/what-is-free-software), ready to
run inside your data-center.

![Screenshot of the dashboard](https://grafana.com/api/dashboards/10428/images/6557/image)

By default it measures (in various forms):

- Controller Action Runtime
- View/Partial Render Runtime
- Database Query Runtime

It provides an overview and you can also drill down into numbers on a per request basis. Of course you can use all the awesome features that Influx (Downsampling/Data Retention), Grafana (Alerts, Annotations) and influxdb-rails (custom tags) provide and extend this to your needs. Use your freedom and run, copy, distribute, study, change and improve this software!

## Requirements

To be able to measure performance you need the following things available:

- [InfluxDB 1.x](https://docs.influxdata.com/influxdb/v1.8/introduction/install/)
- [Grafana](https://grafana.com/docs/)
- A [Ruby On Rails](https://rubyonrails.org/) application with [influxdb-rails](https://github.com/influxdata/influxdb-rails) enabled

## Installation

Once you have influx/grafana instances running in your infrastructure just [import both
dashboards from grafana](https://grafana.com/docs/reference/export_import/#importing-a-dashboard).

- [Overview Dashboard](https://grafana.com/dashboards/10428)
- [Request Dashboard](https://grafana.com/dashboards/10429)

You can also paste the `.json` files from this repository.

In the unlikely case that you need to change the dashboard *UID*s during import you can configure the *UID* the `Overview` dashboard uses to link to the `Request` dashboard in the [variables](https://grafana.com/docs/reference/templating/#adding-a-variable). Just paste whatever *UID* you've set up for the `Request` dashboard.

## Demo

This repository includes a [docker-compose](https://docs.docker.com/compose/) demo setup that brings a simple rails app, influxdb and grafana.

### Starting the demo services

Clone this repository and run

```shell
docker-compose up
```

### Browse the sample app...

Go to http://0.0.0.0:4000 and do some things. Every request to the rails app will generate performance data in the demo.

### ...or Configure your own Rails app...

You can also use the dashboard with any other rails app you already have. Follow our [install instructions](https://github.com/influxdata/influxdb-rails/#installation), the default configuration works with the demo InfluxDB running on localhost:8086.

To be able to view individual requests you have to enable request ID tags in your application. Something like:

```ruby
class ApplicationController < ActionController::Base

  before_action :set_influx_data

  def set_influx_data
    InfluxDB::Rails.current.values = { request: request.request_id }
  end
end
```

### ...then see the dashboards in action

Just go to http://0.0.0.0:3000 and log in with admin/admin.

Enjoy! ❤️
