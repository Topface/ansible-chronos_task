# topface.chronos_task

Deploy [Chronos](https://airbnb.github.io/chronos/) tasks from ansible.

Use [topface.marathon_app](https://github.com/Topface/ansible-marathon_app)
to deploy Chronos on Marathon
and [marathoner](https://github.com/bobrik/marathoner) to make Chronos
available on well-known host and port. Feel free to use any other path
to set `chronos_url`.

## Usage

This role deploys tasks on Chronos via REST API.

### Role configuration

* `chronos_url` url of Chronos, example: `http://chronos.dev:8000`
* `chronos_tasks` list of chronos tasks to add

### Example

```yaml
- hosts: chronos-api-server
  gather_facts: no
  roles:
    - role: topface.chronos_task
      chronos_url: http://chronos.dev:8000
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

[MIT](LICENSE)
