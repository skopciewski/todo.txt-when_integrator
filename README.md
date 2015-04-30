# WhenIntegrator addon for the ToDo.txt

This extension provides the command **win** for the [todo.txt][todotxt] scritp.
It is used to create the todo entries based on the events from the [when][when]
calendar.

**Ruby required**

## Assumptions

* You can mark a specific event in the calendar
  * the today event should be handled in x days (ex: "System update today
    comes out - perform an update, do it in 3 days")
  * The today event should be handled due specific date (ex: "System
    update today comes out - perform an update, do it due 2000-01-01")
  * The incoming event shpuld be handled earlier (ex: "Pay the bill since
    2000-01-01 - create the task about that 2 days earlier")
* When the script runs, get the active events (cond: `event_date` &lt;
  `current_date` &lt; `deadline_date`)
* When the script runs, import the active events into the todo list (if not exists)
* When the script runs, increase the status of the ending events

## Marking convention

* `<t:+x>`
  * `event_date` = the date from the calendar
  * `deadline_date` = `event_date` + `x` days
* `<t:2001-01-01>`
  * `event_date` = the date from the calendar
  * `deadline_date` = `2001-01-01`
  * or, conversely, if the input date is less than the event date
* `<t:>`
  * `event_date` = `deadline_date` = the date from the calendar
* `<t:-x>`
  * `deadline_date` = the date from the calendar
  * `event_date` = `deadline_date` - `x` days

## Installation

Add-ons can be installed into the $HOME/.todo.actions.d directory, or any other
directory configured via $TODO_ACTIONS_DIR.
So, download **win** script, and copy it to that dir.
After installing the add-on, you must make it executable; for example:

```bash
chmod +x ~/.todo.actions.d/win
```

## Usage

Edit when calendar:

```bash
when e
```

Place the mark in the specific event, for example:

```
...
d=1 , update the system <t:+5>
...
```

Execute manually or add it to the Cron:

```bash
todo.sh win "`when --noheader --language=EN --past=-30 m`"
```

where:
* `todo.sh` - your command to execute the todo.sh scritp (full path, alias, etc...)
* `win` - command provided by the addon
* "`when ...`" - command which lists the calendar events
  * `--noheader` - reduce the output
  * `--language=EN` - for parsing dates
  * `--past=-30` - we do not want miss events
  * `m` - we want know about events in the future

### Important

Because we use `--past=-30` and `m` (`--future=30`), so:
* `deadline_date` - `event_date` = approximately 60 days
* we should not set triggers longer than 30 days (max: &lt;t:+30&gt;)

... or if needed - increase the date range using `--past` and `--future` options

## Versioning

See [semver.org][semver]

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

[todotxt]: https://github.com/ginatrapani/todo.txt-cli
[when]: http://www.lightandmatter.com/when/when.html
[semver]: http://semver.org/
