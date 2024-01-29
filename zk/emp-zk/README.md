#### EMP-toolkit

1. With docker installed, create the base image by running:
```
docker build -t emp .
```

2. To run tests, rebuild image and start services:
```
docker build -f Dockerfile2 . -t emp:0.1
docker-compose up
```

3. To change file, change `.cpp` file from `src` in `CMakeLists.txt`
