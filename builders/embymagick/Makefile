all: build

current_dir := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
VOLUMES = \
		  --volume=$(current_dir):/pkg

ENV_VARS = \
		   --env="USER_UID=$(shell id -u)" \
		   --env="USER_GID=$(shell id -g)" \
		   --env="BUILD_USER=${USER}"

DIST = "ubuntu/wily"

help:
	@echo ""
	@echo "-- Help Menu"
	@echo ""
	@echo "   1. make embymagick       - build imagemagick debian package for emby"
	@echo "   4. make bash             - bash login"
	@echo "   5. make build            - build imagemagick-builder image"
	@echo ""

clean:
	@docker rm -f `docker ps -a | grep "${USER}/imagemagick-builder" | awk '{print $$1}'` > /dev/null 2>&1 || exit 0
	@docker rmi `docker images  | grep "${USER}/imagemagick-builder" | awk '{print $$3}'` > /dev/null 2>&1 || exit 0

build:
	@docker build --rm=true --tag=${USER}/imagemagick-builder -f ./${DIST}/Dockerfile .

embymagick bash: build
	@echo "Starting container to build $1 package."
	@docker run -it --rm \
		${ENV_VARS} \
		${VOLUMES} \
		${USER}/imagemagick-builder:latest $@
