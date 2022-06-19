# speedtest_exporter

Prometheus Exporter in Python to colect metrics from speedtest about your internet provider

## Badges

[![python][python-badge]][python-version] ![pylint-score]

## Setup

### Resolving Dependencies

```sh
pip3 install -r requirements.txt
```

| Option         | Mandatory?        | Description          |
|----------------|-------------------|----------------------|
| **-p, --port** | No. Default: 8042 | App Port             |
| **-h, --help** | No                | Show help like below |

### Systemd

Change the path to this script in the file file to `speedtest_exporter.service`, change informations from command-line options and follow the steps to configure the script to run like a [systemd] service.

## Command-line Options

```sh
# speedtest_exporter
usage: speedtest_exporter [-h] [-p [PORT]]

optional arguments:
  -h, --help            show this help message and exit
  -p [PORT], --port [PORT]
                        Port, default: 8042
```

## Prometheus Configuration

Add the job below to your Prometheus Configuration File to scrape the app metrics.

```yml
- job_name: 'speed_test'
    scheme: http # change to http if you don't have https
    metrics_path: '/metrics'
    scrape_timeout: 1m
    scrape_interval: 1m
    static_configs:
      - targets:
        - mytarget
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:8042 # Change here with your real exporter address:port
```

## Grafana Dashboard

Import the file "speedtest_dashboard.json" to your Grafana.

## Docker

Soon.

## Maintainer

 [Reinaldo Lima]

## License

See LICENSE file

[//]: #

[python-badge]: https://img.shields.io/badge/python-3.7.3-blue
[python-version]: https://www.python.org/downloads/release/python-373/
[pylint-score]: https://mperlet.github.io/pybadge/badges/9.18.svg
[Reinaldo Lima]: https://github.com/reimlima
[systemd]: https://wiki.debian.org/systemd/Services
[YAML]: https://en.wikipedia.org/wiki/YAML