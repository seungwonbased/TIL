# 1. 아래 명령어 실행 결과 및 실행 후 Ctrl + C 또는 Ctrl + PQ 시그널 전달 결과

> docker container run -p 8080 example/echo:latest

- 입출력이 안돼서 탈출 불가능

> docker container run -p 8080 -d  example/echo:latest

- 백그라운드에서 돌고 있어서 Attach하지 않으면 시그널 전달 불가능
- Attach해서 들어가도 입출력이 안돼서 탈출 불가능

> docker container run -p 8080 -it example/echo:latest

- 인터럽트가 발생해 컨테이너 중지됨

> docker container run -p 8080 example/echo:latest /bin/bash

- CMD가 /bin/bash로 덮어씌워졌고, 표준 입출력이 없기 때문에 bash가 바로 죽으며 컨테이너 중지됨

> docker container run -p 8080 -d example/echo:latest /bin/bash

- CMD가 /bin/bash로 덮어씌워졌고, 백그라운드로 run 되지만 표준 입출력이 없기 때문에 bash가 바로 죽으며 컨테이너 중지됨

> docker container run -p 8080 -it example/echo:latest /bin/bash

- CMD가 /bin/bash로 덮어씌워졌고, ctrl + pq로 나오면 bash가 죽지 않기 때문에 컨테이너가 백그라운드에서 구동되고 있음
- exit으로 나오면 bash가 죽으며 컨테이너 중지됨

