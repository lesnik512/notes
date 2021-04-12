# Using pip-tools for multiple environments

For couple of years I'm using pip-tools for managing requirements but recently I faced with a problem splitting requirements for dev and production environments.
The problem was that if we just split requirements and compile them separately we will end up with different versions.

So next step is to use an approach from [this article](https://jamescooke.info/a-successful-pip-tools-workflow-for-managing-python-package-requirements.html). The idea is to compile `requirements.prod.in` file first and than compile development part with `requirements.prod.txt` as constraints. It can be done like this:

```bash
-c requirements.prod.txt

# other requirements
```

This solution is also has drawbacks which I faced on the first try. I'v got unsolvable dependendencies between requirements.prod.txt and my development requirements. This conflict can be resolved manually, but it's quite a tedious and not an elegant solution.

Next step is using intermediate file with all constraints, compile it and than build requirements. I found this solution in [this comment](https://github.com/jazzband/pip-tools/issues/1092#issuecomment-632584777) to github issue. The author BTW is the same person, who wrote the article in the link above. Thank you, [James Cooke](https://jamescooke.info)!

Here my modified solution for `Makefile`:

```bash
## pip-compile: compile all requirements
pip-compile: prepare-constraints
	pip-compile constraints.in
	pip-compile requirements.prod.in
	pip-compile requirements.dev.in

## pip-upgrade: upgrade all requirements
pip-upgrade: prepare-constraints
	pip-compile constraints.in --upgrade
	rm -f requirements.prod.txt requirements.dev.txt
	pip-compile requirements.prod.in
	pip-compile requirements.dev.in

prepare-constraints: check-pip-compile
	rm -f constraints.in
	touch constraints.txt
	cat requirements.*.in > constraints.in

## pip-sync:    sync requirements in local environment
pip-sync: check-pip-compile
	pip-sync requirements.prod.txt requirements.dev.txt

check-pip-compile:
	@which pip-compile > /dev/null
```

And you need to add a following string to each .in file:

```bash
-c constraints.txt
```