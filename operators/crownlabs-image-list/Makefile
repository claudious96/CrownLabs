all: container

TAG?=v0.1-crown
REGISTRY?=crownlabs
DOCKER?=docker

IMGNAME = update-crownlabs-image-list
IMAGE = $(REGISTRY)/$(IMGNAME)

container:
	$(DOCKER) build -t $(IMAGE):$(TAG) .

push:
	$(DOCKER) push $(IMAGE):$(TAG)

clean:
	$(DOCKER) rmi -f $(IMAGE):$(TAG) || true

release: container push
	echo "done"
