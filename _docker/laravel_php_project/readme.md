docker-compose run --rm composer create-project --prefer-dist laravel/laravel .
docker-compose up server -d --build
    build - force to recreate images (check for image changes)