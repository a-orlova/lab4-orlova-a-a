# ЛАБ 4
Начинаю лабораторную работу с установки докера. Для этого пишу в терминале следующите команды:
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```
Докер успешко установлен, перехожу к самому заданию лабораторной работы. В первую очередь я создаю директорию docker_aafire с помощью mkdir, где будет храниться мой докер-файл Dockerfile, создаю его внутри директории с помощью touch. 
Затем используя текстовый редактор nano и соответсвущей команды ввожу туда следующий текст:
```bash
FROM ubuntu:latest
RUN apt-get update && apt-get install -y libaa-bin
RUN apt-get update && apt-get install -y iputils-ping
```
![image](https://github.com/user-attachments/assets/2ee2b83b-8c48-4dbe-96e1-296b849a4361)

Dockerfile создает образ, основанный на Ubuntu. Он обновляет пакетные репозитории и устанавливает два пакета:
1) libaa-bin для графической анимации в текстовом режиме.
2) iputils-ping для проверки сетевых соединений командой ping.
Этот образ будет использоваться для запуска контейнеров с aafire и проверки сетевых соединений.

После изменения Dockerfile'a я строю два образа с помощью:
```bash
docker build -t aafire1 .
docker build -t aafire2 .
```
![image](https://github.com/user-attachments/assets/4703d230-8ef2-44e7-84ef-832244d5bdf1)

Все прошло успешно, поэтому теперь перехожу к самому запуску контейнеров. Для запуска контейнера-1 пишу команду:
```bash
docker run -it aafire1 
```
И аналогичную для запуска контейнера-2:
```bash
docker run -it aafire2
```
![image](https://github.com/user-attachments/assets/98576d9f-d97f-4eaa-b8c3-74633794a67a)

В итоге в отедльных окнах появляются анимации огня - интерактивная форма контейнеров, они работают в фоновом режиме.
![image](https://github.com/user-attachments/assets/bd21c1d5-aeb7-462f-8a17-7fe0510fe319)
![image](https://github.com/user-attachments/assets/a3980ef6-c832-4de2-948c-11fbc5bd0d9b)

Теперь я создаю сеть с помощью команды:
```bash
docker network create myNetwork
```
А с помощью *docker ps* узнаю имена контейнеров. Затем подключаю оба контейнера к своей сети с помощью:
```bash
docker network connect myNetwork *имя контейнера*
```
После этого использую *docker inspect* и узнаю айпи-адреса обоих контейнеров.
![image](https://github.com/user-attachments/assets/ab85e1a9-fce8-4cd1-ae2e-29e3b16b9c26)
![image](https://github.com/user-attachments/assets/a465e671-90a3-4313-b724-48df0ead2c16)

Затем использую следующую команду для каждого из контейнеров, чтобы войти в него, и уже оттуда проверить связь с другим контейнером через *ping*:

