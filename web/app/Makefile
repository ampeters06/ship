.PHONY: clean_build_artifacts clean deps serve_ship build_ship build_ship_dev test test_CI

SHELL := /bin/bash
$SRC = $(shell find . -path "./node_modules" -prune -o -path "./build" -prune -o \( -name "*.js" -o -name "*.jsx" -o -name "*.scss" -o -name "*.json" -o -name "*.svg" -o -name "*.png" \) -print)

.state/package: package.json yarn.lock
	if [[ -n "${CIRCLECI}" ]]; then CYPRESS_INSTALL_BINARY=0 yarn install; else yarn install; fi
	@mkdir -p .state
	@touch .state/package

.state/package-init: ../init/package.json ../init/yarn.lock
	cd ../init && \
		yarn --pure-lockfile && \
		yarn build && \
		yarn link
	@mkdir -p .state
	@touch .state/package-init

.state/package-init-dev: ../init/package.json ../init/yarn.lock
	cd ../init && \
		yarn --pure-lockfile && \
		yarn build-dev && \
		yarn link
	@mkdir -p .state
	@touch .state/package-init-dev

clean_build_artifacts:
	rm -rf build
	rm -rf dist
	rm -rf ../init/dist

clean: clean_build_artifacts
	rm -rf node_modules
	rm -rf ../init/node_modules
	rm -rf .state

deps: .state/package .state/package-init

deps-dev: .state/package .state/package-init-dev

serve_ship:
	yarn start

.state/build_ship: deps $(shell find src -type f)
	yarn build
	@mkdir -p .state
	@touch .state/build_ship

.state/build_ship_dev: deps-dev
	yarn build
	@mkdir -p .state
	@touch .state/build_ship_dev

build_ship: .state/build_ship

build_ship_dev: .state/build_ship_dev

test:
	yarn test

test_CI:
	cd ../init && \
	yarn --pure-lockfile && \
	CI=1 yarn test
