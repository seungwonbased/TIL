# Fotmatting

https://docs.docker.com/engine/reference/commandline/ps/#format

## Format the Output (--format)

|Placeholder|Description|
|---|---|
|`.ID`|Container ID|
|`.Image`|Image ID|
|`.Command`|Quoted command|
|`.CreatedAt`|Time when the container was created.|
|`.RunningFor`|Elapsed time since the container was started.|
|`.Ports`|Exposed ports.|
|`.State`|Container status (for example; "created", "running", "exited").|
|`.Status`|Container status with details about duration and health-status.|
|`.Size`|Container disk size.|
|`.Names`|Container names.|
|`.Labels`|All labels assigned to the container.|
|`.Label`|Value of a specific label for this container. For example `'{{.Label "com.docker.swarm.cpu"}}'`|
|`.Mounts`|Names of the volumes mounted in this container.|
|`.Networks`|Names of the networks attached to this container.|

```bash
$ docker ps --no-trunc --format "table {{.Image}} : {{.Command}}"
```

- table 명령어를 포함해 테이블 형식으로 출력 가능
- --no-trunc는 길어도 전부 다 출력