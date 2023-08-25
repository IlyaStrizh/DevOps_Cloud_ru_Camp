FROM debian

WORKDIR /build

COPY ./app.py .

RUN apt-get update \
  && apt-get install -y python3 \
  && rm -rf /var/lib/apt/lists \
  # Добавляем пользователя
  && adduser server_user \
  && chown -R server_user:server_user /build/ \
  && chown -R server_user:server_user /var/run/ \
  && chmod +rx app.py \
  # Устанавливаем биты setgid (g-s позволяет пользователям,
  # принадлежащим к той же группе, что и владелец файла, временно
  # получить привилегии группы-владельца файла.)
  # Устанавливаем биты setuid (u-s позволяет пользователю,
  # который запускает программу, получить временные привилегии
  # пользователя-владельца файла)
  && chmod 755 /usr/bin/passwd \
  && chmod 755 /usr/bin/chage \
  && chmod 755 /usr/bin/gpasswd \
  && chmod 755 /usr/sbin/unix_chkpwd \
  && chmod 755 /usr/bin/wall \
  && chmod 755 /usr/bin/expiry \
  && chmod 755 /usr/bin/chfn \
  && chmod 755 /usr/bin/chsh \
  && chmod 755 /usr/bin/newgrp \
  && chmod 755 /usr/bin/su \
  && chmod 755 /usr/bin/mount \
  && chmod 755 /usr/bin/umount

HEALTHCHECK CMD curl --fail http://localhost:8000/hostname || exit 1

USER server_user

CMD ["python3", "app.py", "--host", "0.0.0.0", "--port", "8000"]