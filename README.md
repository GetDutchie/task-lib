# task-lib

Task Lib is a collection of re-usable tasks that work with [taskfile.dev](https://taskfile.dev/).

## Usage

There is currently no native way to include remote task files into your project, but you have a couple of options.

### 1. Fetch updates

You can use [go-getter]() to pull updates as needed. You can even implement this as a task in your `Taskfile.yml`.

```yaml
update:
  desc: Update task dependencies
  cmds:
    - for: [tilt, python, rancher, yamlfmt]
      cmd: |
        go-getter github.com/GetDutchie/task-lib/tools/{{.ITEM}} .task/lib/tools/{{.ITEM}}
```

This task will loop throught the components listed and download them to `.task/lib/tools/<component>`.

#### OR Git Submodules

Don't like fetching updates? Take a look at [Git Tools Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) for information on submodules for git. Then include this repo (or a fork) as a submodule at `.task` or `.task-lib`.

### 2. Include the tasks

Now that the library taskfiles have been fetched they can be used. Taskfiles can [include](https://taskfile.dev/usage/#including-other-taskfiles) other taskfiles.

```yaml
includes:
  tilt: ".task/lib/tools/tilt/task.yaml"
  python: ".task/lib/tools/python/task.yaml"
  rancher: ".task/lib/tools/rancher/task.yaml"
  yamlfmt: ".task/lib/tools/yamlfmt/task.yaml"
```

You can then call the includes taskfiles and their tasks with the `<namespace>:<task>` format. For example:

```yaml
setup:
  cmds:
    - task: yamlfmt:install
```

The setup task will now call the `yamlfmt` libraries `install` task.

## Windows

On Windows by default tools get installed in `${HOME}/tools`. Make sure to add this to your path.