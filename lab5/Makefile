STATES := app tests
REPO := nazaranisimov/lab5

.PHONY: $(STATES)

$(STATES):
	@docker build -f Dockerfile.$(@) -t $(REPO):$(@) .

run:
	@(docker network create --driver=bridge appnet || true) \
	&& docker run --rm --name redis --net=appnet -d redis \
	&& docker run --rm -it --name app --net=appnet -d -p 5000:5000 $(REPO):app

test-app:
	@docker run --rm -it --name test --net=appnet $(REPO):tests

docker-prune:
	@@docker rm $$(docker ps -a -q) --force || true \
	&& docker container prune --force \
	&& docker volume prune --force \
	&& docker network prune --force \
	&& docker image prune --force
docker-push:
	@docker push $(REPO):app
	@docker push $(REPO):tests
docker-image-prune:
	@docker image prune -a --force
