# presentation-reveal-md-ansible-dev-stack

## Description



## Requirements

* git
* reveal-md

## Run presentation and make live edits.

```shell
reveal-md -w slides.md --css css/overide.css
```

## Regenerate PDF version of presenation.

```shell
reveal-md slides.md --css css/overide.css --print resources/ansible-dev-stack.pdf
```

## Regenerate static HTML version of presentation.

```shell
reveal-md slides.md --css css/overide.css --static html
```

