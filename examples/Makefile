#
# Example Makefile for fake-mocha using Docker
# Pass [--command "make test"] to fake-mocha to use.
#
# Copyright (c) 2017 Tom Shawver
#

DIR = $(shell pwd)
# Populate the following with an environment variable to be able to support
# the Linux path and/or the full range of JetBrains IDEs on your team -- they
# just need to configure the Mocha run configuration to set that environment
# variable to the appropriate path before the tests run!
JETBRAINS_DIR = "$${HOME}/Library/Application Support/IntelliJIdea14"
JB_MOUNT = "/jetbrains"
JB_REPORTER_PATH = "NodeJS/js/mocha-intellij/lib/mochaIntellijReporter.js"
DEFAULT_REPORTER = spec

all: install test

clean:
	rm -rf node_modules

install:
	docker run --rm -v $(DIR):$(DIR) -w $(DIR) node npm install

localtest:
# Run tests using the IntelliJ reporter if it exists
	@reporter=$(DEFAULT_REPORTER); \
	if [ -d "$(JB_MOUNT)" ]; then \
		reporter="$(JB_MOUNT)/$(JB_REPORTER_PATH)"; \
	fi; \
	node_modules/mocha/bin/_mocha --recursive --reporter $$reporter test

test:
# Execute, linking in jetbrains directory if applicable. By mounting the
# project at the same path as the local machine, file paths in the test output
# (such as when a test fails, or in stack traces) become clickable!
	@dockerArgs="run --rm -v $(DIR):$(DIR) -w $(DIR)"; \
	if [ -d "$(JETBRAINS_DIR)" ]; then \
		docker $$dockerArgs -v "$(JETBRAINS_DIR):$(JB_MOUNT)" node make localtest; \
	else \
		docker $$dockerArgs node make localtest; \
	fi

.PHONY: all clean install localtest test
