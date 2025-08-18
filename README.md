#tegramm
uzgramm
import socket
import threading
import uuid
import sys

clients = {}  # {id: (socket, addr)}

# ================= СЕРВЕР =================
def handle_client(client_socket, client_id):
    while True:
        try:
            msg = client_socket.recv(1024).decode("utf-8")
            if not msg:
                break

            # формат: SEND:ID:текст
            if msg.startswith("SEND:"):
                parts = msg.split(":", 2)
                if len(parts) == 3:
                    target_id, text = parts[1], parts[2]
                    if target_id in clients:
                        target_socket, _ = clients[target_id]
                        target_socket.send(f"От {client_id}: {text}".encode("utf-8"))
                    else:
                        client_socket.send(f"❌ Пользователь {target_id} не найден".encode("utf-8"))
            else:
                client_socket.send("⚠ Неверная команда".encode("utf-8"))

        except:
            break

    if client_id in clients:
        del clients[client_id]
    client_socket.close()
    print(f"[Отключен] {client_id}")

def run_server():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind(("0.0.0.0", 5555))
    server.listen()
    print("[Сервер запущен на порту 5555]")

    while True:
        client_socket, addr = server.accept()
        client_id = str(uuid.uuid4())[:8]  # короткий ID
        clients[client_id] = (client_socket, addr)
        print(f"[Подключен] {client_id} {addr}")

        client_socket.send(f"Ваш ID: {client_id}".encode("utf-8"))

        thread = threading.Thread(target=handle_client, args=(client_socket, client_id))
        thread.start()

# ================= КЛИЕНТ =================
def listen(sock):
    while True:
        try:
            msg = sock.recv(1024).decode("utf-8")
            if msg:
                print("\n" + msg)
        except:
            break

def run_client():
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(("127.0.0.1", 5555))  # можно заменить IP, если сервер на другом ПК

    print(sock.recv(1024).decode("utf-8"))  # вывод ID

    threading.Thread(target=listen, args=(sock,), daemon=True).start()

    while True:
        to_id = input("Кому (ID): ")
        text = input("Сообщение: ")
        msg = f"SEND:{to_id}:{text}"
        sock.send(msg.encode("utf-8"))

# ================= ЗАПУСК =================
if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Использование:")
        print("  python messenger.py server  - запустить сервер")
        print("  python messenger.py client  - запустить клиента")
        sys.exit(1)

    if sys.argv[1] == "server":
        run_server()
    elif sys.argv[1] == "client":
        run_client()
    else:
        print("Неизвестная команда")
