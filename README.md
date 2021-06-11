## invidious-arm-build
Attempt to build Invidious for arm (Raspberry PI)

# Current issues:

- NOT REALLY WORKING - crashes with:
```
invidious_1  | Failed to raise an exception: FAILURE
invidious_1  | [0x9ff97c] ???
invidious_1  | [0x18090] ???
invidious_1  | [0x354f4] ???
invidious_1  | [0x35208] ???
invidious_1  | 
invidious_1  | Tried to raise:: Unable to tell: Invalid argument (IO::Error)
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
invidious_1  |   from ???
```

- Assets not loading


# Building:
## Build lsquic for arm
sudo docker buildx build -t invidious:lsquicBuilder --platform linux/arm/v7 -f Dockerfile_lsquicBuilder .

## Cross-compile but don't link invidious
sudo docker buildx build -t invidious:compile --platform linux/arm/v7 -f Dockerfile_compile .

## Link invidious on arm
sudo docker buildx build -t invidious:link --platform linux/arm/v7 -f Dockerfile_link .

## Build final image
sudo docker buildx build -t invidious:arm --platform linux/arm/v7 -f Dockerfile_invidiousARM .

## Export image for import on Raspberry PI
sudo docker save invidious:arm | xz > invidious_arm.tar.xz