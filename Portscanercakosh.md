import socket
import pyfiglet
from termcolor import colored
import time
import threading

# PyFiglet ile ASCII sanatı oluştur
figlet_text = pyfiglet.figlet_format("Port Scanner Security Wiles cyber Team", font="standard")

# ASCII sanatını renklendir ve yazdır
print(colored(figlet_text, 'blue'))

# Kullanıcıdan bir IP adresi alın
user_input = input(colored('Lütfen bir kullanıcı IP adresi giriniz: ', 'red'))
print(colored(f'Kullanıcı IP adresi: {user_input}', 'yellow'))

# Kısa bir bekleme süresi
time.sleep(1)

def scan_port(ip, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((ip, port))
        if result == 0:
            print(f"Port {port} is open")
        else:
            print(f"Port {port} is closed")
    except Exception as e:
        print(f"Error scanning port {port}: {e}")
    finally:
        sock.close()

def scan_ports(ip, port_range):
    threads = []
    for port in port_range:
        thread = threading.Thread(target=scan_port, args=(ip, port))
        thread.start()
        threads.append(thread)
        # Bekleme süresini ayarla
        time.sleep(0.03)  # Her tarama arasında yaklaşık 0.03 saniye bekle

    # Tüm thread'lerin tamamlanmasını bekle
    for thread in threads:
        thread.join()

# Kullanıcının verdiği IP adresini kullanarak port taraması yap
target_ip = user_input
scan_ports(target_ip, range(1, 1025))
