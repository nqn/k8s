# Copyright 2017 The Kubernetes Authors.
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

IMG = gcr.io/tf-on-k8s-releasing/releaser
TAG = $(shell date +v%Y%m%d)-$(shell git describe --tags --always --dirty)

all: build

build:
	@echo {\"image\": \"$(IMG):$(TAG)\"} > version.json
	mkdir -p py
	cp ../py/*.py ./py/
	docker build -t $(IMG):$(TAG) -f Dockerfile.release .
	docker tag $(IMG):$(TAG) $(IMG):latest
	@echo Built $(IMG):$(TAG) and tagged with latest
	rm -f version.json
	rm -rf ./py

push: build
	gcloud docker -- push $(IMG):$(TAG)
	gcloud docker -- push $(IMG):latest
	@echo Pushed $(IMG) with :latest and :$(TAG) tags
