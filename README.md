#tegramm
uzgramm
# messenger.py
# Простой мессенджер "как Телеграм по ID": один файл, сервер+клиент.
# Работает в консоли. Без БД/шифрования. Для обучения/демо.

import socket
import threading
import uuid
import sys

# ---------- Глобальные структуры (на сервере) ----------
clients = {}        # id -> {"sock": socket, "addr": (ip,port), "username": str|None}
user_creds = {}     # username -> password
user_by_name = {}   # username -> id (последняя сессия)
lock = threading.Lock()


# ---------- Общие утилиты ----------
def send_line(sock, text: str):
    try:
        sock.sendall((text + "\n").encode("utf-8"))
    except Exception:
        pass

def recv_lines(sock):
    """Итератор по строкам от сокета (utf-8, \n)."""
    f = sock.makefile("r", encoding="utf-8", errors="ignore", newline="\n")
    while True:
        line = f.readline()
        if not line:
            break
        yield line.rstrip("\n")


# ---------- СЕРВЕР ----------
def handle_client(client_sock: socket.socket, client_id: str, addr):
    with lock:
        clients[client_id] = {"sock": client_sock, "addr": addr, "username": None}

    send_line(client_sock, f"ASSIGNED|{client_id}")
    send_line(client_sock, "INFO|Команды: "
                           "/register <name> <pass>, /login <name> <pass>, "
                           "/send <id> <текст>, /whoami, /quit")

    try:
        for line in recv_lines(client_sock):
            if not line:
                break

            # Протокол: <CMD>|...
            if "|" in line:
                cmd, rest = line.split("|", 1)
            else:
                cmd, rest = line, ""

            if cmd == "REGISTER":
                # REGISTER|name|pass
                parts = rest.split("|", 1)
                if len(parts) != 2:
                    send_line(client_sock, "ERROR|Используй: /register <name> <pass>")
                    continue
                name, pwd = parts[0].strip(), parts[1].strip()
                with lock:
                    if name in user_creds:
                        send_line(client_sock, "ERROR|Такой username уже существует")
                    else:
                        user_creds[name] = pwd
                        clients[client_id]["username"] = name
                        user_by_name[name] = client_id
                        send_line(client_sock, f"INFO|Регистрация ок. Вы вошли как {name}")

            elif cmd == "LOGIN":
                # LOGIN|name|pass
                parts = rest.split("|", 1)
                if len(parts) != 2:
                    send_line(client_sock, "ERROR|Используй: /login <name> <pass>")
                    continue
                name, pwd = parts[0].strip(), parts[1].strip()
                with lock:
                    if user_creds.get(name) != pwd:
                        send_line(client_sock, "ERROR|Неверный логин или пароль")
                    else:
                        clients[client_id]["username"] = name
                        user_by_name[name] = client_id
                        send_line(client_sock, f"INFO|Вход выполнен. Вы: {name}")

            elif cmd == "SEND":
                # SEND|target_id|text
                parts = rest.split("|", 1)
                if len(parts) != 2:
                    send_line(client_sock, "ERROR|Используй: /send <id> <текст>")
                    continue
                target_id, text = parts[0].strip(), parts[1]
                with lock:
                    if target_id in clients:
                        sender_name = clients[client_id]["username"]
                        prefix = sender_name if sender_name else client_id
                        send_line(clients[target_id]["sock"], f"MSG|{prefix}|{text}")
                        send_line(client_sock, "INFO|Отправлено")
                    else:
                        send_line(client_sock, f"ERROR|Пользователь {target_id} не найден")

            elif cmd == "WHOAMI":
                with lock:
                    name = clients[client_id]["username"]
                if name:
                    send_line(client_sock, f"INFO|Вы: {name} (ID {client_id})")
                else:
                    send_line(client_sock, f"INFO|Вы: (без имени) ID {client_id}")

            elif cmd == "QUIT":
                send_line(client_sock, "INFO|Пока!")
                break

            else:
                send_line(client_sock, "ERROR|Неизвестная команда")

    except Exception:
        pass
    finally:
        with lock:
            info = clients.pop(client_id, None)
            if info and info["username"]:
                # если этот id был последним для username — очищать не обязательно
                pass
        try:
            client_sock.close()
        except Exception:
            pass
        print(f"[Отключен] {client_id} {addr}")


def run_server(host="0.0.0.0", port=5555):
    srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    srv.bind((host, port))
    srv.listen(100)
    print(f"[СЕРВЕР] слушает {host}:{port}")

    while True:
        sock, addr = srv.accept()
        client_id = str(uuid.uuid4())[:8]
        print(f"[Подключен] {client_id} {addr}")
        threading.Thread(target=handle_client, args=(sock, client_id, addr), daemon=True).start()


# ---------- КЛИЕНТ ----------
def client_listener(sock: socket.socket):
    try:
        for line in recv_lines(sock):
            if not line:
                print("\n[СЕРВЕР] соединение закрыто")
                break
            if "|" in line:
                tag, rest = line.split("|", 1)
            else:
                tag, rest = line, ""
            if tag == "ASSIGNED":
                print(f"\nВаш ID: {rest}")
            elif tag == "MSG":
                sender, text = rest.split("|", 1) if "|" in rest else (rest, "")
                print(f"\nСообщение от {sender}: {text}")
            elif tag in ("INFO", "ERROR"):
                print(f"\n{tag}: {rest}")
            else:
                print("\n" + line)
    except Exception:
        print("\n[КЛИЕНТ] соединение потеряно")


def run_client(host="127.0.0.1", port=5555):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((host, port))
    threading.Thread(target=client_listener, args=(sock,), daemon=True).start()

    print("Подключено. Введите команды.")
    print("Команды:\n"
          "  /register <name> <pass>\n"
          "  /login <name> <pass>\n"
          "  /send <id> <текст>\n"
          "  /whoami\n"
          "  /quit\n")

    try:
        while True:
            line = input("> ").strip()
            if not line:
                continue
            if line.startswith("/register "):
                _, rest = line.split(" ", 1)
                try:
                    name, pwd = rest.split(" ", 1)
                except ValueError:
                    print("Используй: /register <name> <pass>")
                    continue
                send_line(sock, f"REGISTER|{name}|{pwd}")

            elif line.startswith("/login "):
                _, rest = line.split(" ", 1)
                try:
                    name, pwd = rest.split(" ", 1)
                except ValueError:
                    print("Используй: /login <name> <pass>")
                    continue
                send_line(sock, f"LOGIN|{name}|{pwd}")

            elif line.startswith("/send "):
                # /send <id> <текст>
                try:
                    _, rest = line.split(" ", 1)
                    target_id, text = rest.split(" ", 1)
                except ValueError:
                    print("Используй: /send <id> <текст>")
                    continue
                send_line(sock, f"SEND|{target_id}|{text}")

            elif line == "/whoami":
                send_line(sock, "WHOAMI")

            elif line == "/quit":
                send_line(sock, "QUIT")
                break

            else:
                print("Неизвестная команда. /register, /login, /send, /whoami, /quit")

    except KeyboardInterrupt:
        pass
    finally:
        try:
            sock.close()
        except Exception:
            pass
        print("Отключено.")


# ---------- Точка входа (без аргументов) ----------
if __name__ == "__main__":
    print("Режим: server / client")
    mode = input("Введите режим (server/client): ").strip().lower()
    if mode == "server":
        host = input("Хост (по умолчанию 0.0.0.0): ").strip() or "0.0.0.0"
        try:
            port = int(input("Порт (по умолчанию 5555): ").strip() or "5555")
        except ValueError:
            port = 5555
        run_server(host, port)
    elif mode == "client":
        host = input("IP сервера (по умолчанию 127.0.0.1): ").strip() or "127.0.0.1"
        try:
            port = int(input("Порт (по умолчанию 5555): ").strip() or "5555")
        except ValueError:
            port = 5555
        run_client(host, port)
    else:
        print("Нужно ввести 'server' или 'client'.")

