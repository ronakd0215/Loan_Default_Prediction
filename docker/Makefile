
help:
	cat Makefile
run:
	docker-compose up
exec:
	docker-compose exec loan_default_project bash
build: stop .FORCE
	docker-compose build
rebuild: stop .FORCE
	docker-compose build --no-cache
stop:
	docker stop loan_default_project || true; docker rm loan_default_project || true;
.FORCE:
