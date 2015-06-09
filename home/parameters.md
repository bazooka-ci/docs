# Job parameters

Parameters can be injected to a job and are exposed as environment variable.

`.bazooka.yml` can define mandatory parameters by adding an entry on the env section without value:

```
env:
  - A
  - B=5
```

`A` has to be defined at the start of the job. A job fails if mandatory parameters are missing.

## Inject parameter for a specific job

Use `bzk job start` command with --env option

```
Usage: bzk job start PROJECT_ID [SCM_REF] [--env...]

Start a new bazooka job on a project

Arguments:
  PROJECT_ID=""      the project id
  SCM_REF="master"   the scm ref to build

Options:
  -e, --env=[]string(nil)   define an environment variable for the job
```

E.g.:
```bzk job start my-project my-branch -e A=3 -e B=42```

Permutation can be created by settings multiple times the same variable:
```bzk job start my-project my-branch -e A=3 -e A=5```
