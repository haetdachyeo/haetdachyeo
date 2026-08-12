.PHONY: up down rebuild shell

up:
	docker compose up -d --build
	docker compose exec git bash

down:
	docker compose down

rebuild:
	docker compose down
	docker compose up -d --build

shell:
	docker compose exec git bash