# Copyright 2018 Google LLC All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

#
# Makefile for building a simple udp client and server
#

#  __     __         _       _     _
#  \ \   / /_ _ _ __(_) __ _| |__ | | ___ ___
#   \ \ / / _` | '__| |/ _` | '_ \| |/ _ \ __|
#    \ V / (_| | |  | | (_| | |_) | |  __\__ \
#     \_/ \__,_|_|  |_|\__,_|_.__/|_|\___|___/
#

REPOSITORY ?=
PROD_REPO ?= us-docker.pkg.dev/agones-images/examples

mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
project_path := $(dir $(mkfile_path))
root_path := $(realpath $(project_path)/../..)
version := 0.18
ifeq ($(REPOSITORY),)
	autoscaler_webhook_tag := autoscaler-webhook:$(version)
else
	autoscaler_webhook_tag := $(REPOSITORY)/autoscaler-webhook:$(version)
endif

#   _____                    _
#  |_   _|_ _ _ __ __ _  ___| |_ ___
#    | |/ _` | '__/ _` |/ _ \ __/ __|
#    | | (_| | | | (_| |  __/ |_\__ \
#    |_|\__,_|_|  \__, |\___|\__|___/
#                 |___/

# Build a docker image for the server, and tag it
build:
	cd $(root_path) && docker build -f $(project_path)/Dockerfile --tag=$(autoscaler_webhook_tag) .

# Push the docker image
push: build
	docker push $(autoscaler_webhook_tag)

# check if hosted on Google Artifact Registry
gar-check:
	gcloud container images describe $(PROD_REPO)/$(autoscaler_webhook_tag)

#output the tag for this image
echo-image-tag:
	@echo $(PROD_REPO)/$(autoscaler_webhook_tag)

# build and push the autoscaler-webhook image with specified tag
cloud-build:
	cd $(root_path) && gcloud builds submit --config=./examples/autoscaler-webhook/cloudbuild.yaml