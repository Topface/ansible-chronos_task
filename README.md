# topface.marathon_chronos_task

Deploy [Chronos](https://airbnb.github.io/chronos/) tasks on chronos
that is running on marathon. Marathon ensures that Chronos is always up
and Chronos ensures that your cron jobs are working.

Use [topface.marathon_app](https://github.com/Topface/ansible-marathon_app)
to deploy Chronos on marathon.

## Usage

This role deploys tasks on Chronos via REST API.

### Role configuration

* `marathon_url` url of Marathon, example: `http://marathon.dev:8080`
* `chronos_app_name` used app name for Chronos, example: `/chronos`
* `chronos_tasks` list of chronos tasks to add

### Example

```yaml
- hosts: marathon-api-server
  gather_facts: no
  roles:
    - role: topface.marathon_chronos_task
      marathon_url: http://marathon.dev:8080
      chronos_tasks:
        - type: iso8601
            app:
              name: optimize_something
              owner: ibobrik@gmail.com
              command: sleep 600 # do hard work
              epsilon: PT30M
              retries: 2
              async: false
              schedule: R/2014-10-30T01:45:00.000Z/PT24H
          - type: dependency
            app:
              name: snapshot_something # disabled
              owner: ibobrik@gmail.com
              command: sleep 30 # do easy work
              epsilon: PT30M
              retries: 2
              async: false
              parents:
                - optimize_something
```

Here we deploy one top-level task `optimize_something` and one dependent
task `snapshot_something`. See Chronos docs to learn more.

## License

MIT
