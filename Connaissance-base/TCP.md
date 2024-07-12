Pour illustrer un exemple pratique de l'utilisation du protocole TCP/IP sous Linux, nous allons créer un simple serveur et client TCP en utilisant les outils de ligne de commande disponibles sous Linux. Cet exemple démontrera comment établir une connexion TCP entre un client et un serveur, comment envoyer des données et comment les recevoir.

### Exemple de serveur TCP avec `netcat`

`netcat` (ou `nc`) est un utilitaire réseau très puissant qui peut lire et écrire des données sur des connexions réseau en utilisant les protocoles TCP ou UDP.

1. **Installer `netcat`** :
   Si `netcat` n'est pas déjà installé, vous pouvez l'installer en utilisant votre gestionnaire de paquets.

   Pour Debian/Ubuntu :
   ```bash
   sudo apt-get install netcat
   ```

   Pour CentOS/RHEL :
   ```bash
   sudo yum install nc
   ```

2. **Démarrer un serveur TCP** :
   Démarrons un serveur TCP sur le port 12345.
   ```bash
   nc -l -p 12345
   ```

   Cette commande configure `netcat` pour écouter (`-l`) sur le port (`-p`) 12345 pour les connexions TCP entrantes.

### Exemple de client TCP avec `netcat`

1. **Se connecter au serveur TCP** :
   Sur une autre machine ou dans un autre terminal, utilisez `netcat` pour se connecter au serveur TCP.
   ```bash
   nc [adresse_ip_du_serveur] 12345
   ```

   Remplacez `[adresse_ip_du_serveur]` par l'adresse IP de la machine où le serveur `netcat` est en cours d'exécution. Si vous exécutez à la fois le serveur et le client sur la même machine, utilisez `localhost` ou `127.0.0.1`.

2. **Envoyer et recevoir des messages** :
   Une fois connecté, tout ce que vous tapez dans le terminal du client sera envoyé au serveur, et tout ce que vous tapez dans le terminal du serveur sera envoyé au client.

   Exemple :
   - Dans le terminal client, tapez :
     ```bash
     Hello, Server!
     ```

   - Dans le terminal serveur, vous verrez :
     ```bash
     Hello, Server!
     ```

   - Dans le terminal serveur, tapez :
     ```bash
     Hello, Client!
     ```

   - Dans le terminal client, vous verrez :
     ```bash
     Hello, Client!
     ```

### Exemple de serveur et client TCP avec `Python`

Pour aller un peu plus loin, vous pouvez également utiliser Python pour créer un serveur et un client TCP simples.

1. **Serveur TCP en Python** :

   Créez un fichier `server.py` avec le contenu suivant :
   ```python
   import socket

   host = '0.0.0.0'
   port = 12345

   with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
       s.bind((host, port))
       s.listen()
       print(f"Serveur écoutant sur {host}:{port}")
       conn, addr = s.accept()
       with conn:
           print('Connecté par', addr)
           while True:
               data = conn.recv(1024)
               if not data:
                   break
               print('Reçu du client:', data.decode())
               conn.sendall(data)
   ```

2. **Client TCP en Python** :

   Créez un fichier `client.py` avec le contenu suivant :
   ```python
   import socket

   host = '127.0.0.1'
   port = 12345

   with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
       s.connect((host, port))
       s.sendall(b'Hello, Server!')
       data = s.recv(1024)

   print('Reçu du serveur:', data.decode())
   ```

3. **Exécuter le serveur et le client** :
   - Dans un terminal, démarrez le serveur :
     ```bash
     python3 server.py
     ```

   - Dans un autre terminal, démarrez le client :
     ```bash
     python3 client.py
     ```

   Le client enverra le message "Hello, Server!" au serveur, et le serveur renverra ce message au client.

Ces exemples montrent comment utiliser le protocole TCP/IP sous Linux à l'aide de `netcat` et Python. Ils illustrent les bases de la communication réseau, y compris la configuration d'un serveur TCP, la connexion à partir d'un client, et l'échange de données entre eux.
