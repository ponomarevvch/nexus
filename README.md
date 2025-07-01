# nexus

# 1. Удалим старый swap
sudo swapoff -a

# 2. Создаём новый swap-файл размером 10GB
sudo fallocate -l 10G /swapfile

# 3. Делаем swap-файл безопасным
sudo chmod 600 /swapfile

# 4. Инициализируем его как swap
sudo mkswap /swapfile

# 5. Включаем swap
sudo swapon /swapfile

# 6. Чтобы сохранялся после перезагрузки:
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# 7. Проверим:
free -h

# 9. Скачай образ Ubuntu 24.04
sudo usermod -aG docker $USER
newgrp docker

# 10. Скачай образ Ubuntu 24.04
docker pull ubuntu:24.04

# 11. Запусти контейнер в интерактивной tmux-сессии
tmux new -s ubuntu24
# 11.1 Если упала нода убиваем сессии докера в tmux
docker stop ubuntu24
docker rm ubuntu24
# 11.2 Ставим заново
docker run -it --name ubuntu24 --hostname ubuntu24 ubuntu:24.04 bash

# 12. Настрой внутри контейнера (один раз)
apt update && apt install -y curl git wget build-essential nano lsb-release
ldd --version
#Ожидаешь: 2.39

# 13. Установить ноду в контейнер
apt update && apt install curl -y
curl https://cli.nexus.xyz/ | sh
source ~/.bashrc

# 14. Запуск Nexus
~/.nexus/bin/nexus-network start \
  --node-id <номер> \
  --headless \
  --max-threads $(echo $(( $(nproc) - 1 )))

# 14. Выйти из TMUX
Ctrl + B, потом D

# 15. Вернуться в TMUX
tmux attach -t ubuntu24

# 15. Убить TMUX
tmux kill-session -t ubuntu24





