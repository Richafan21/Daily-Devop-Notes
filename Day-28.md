# Putting Everything into Practice
Context: My app uses flask and spotify-api alongside basic front-end to roll songs.

Today I containerised my app I built on my pc. Here's how I did it.


## Commands ran on PC:
```bash
docker build -t kroulette-app .

docker tag kroulette-app richardfan21/kroulette-app:latest

docker login

docker push richardfan21/kroulette-app:latest
```

## Commands ran on Laptop:
```bash
docker pull richardfan21/kroulette-app:latest

docker run -d -p 5001:5000 richardfan21/kroulette-app:latest
```
