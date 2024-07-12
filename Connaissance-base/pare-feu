L'inspection étatique (ou inspection d'état) est une fonctionnalité de pare-feu avancée qui permet de suivre l'état des connexions réseau et de prendre des décisions basées sur ces états. En utilisant `iptables` sous Linux, vous pouvez configurer un pare-feu avec inspection étatique pour améliorer la sécurité de votre réseau.

Voici un guide pour configurer l'inspection étatique avec `iptables` :

### Configuration de l'inspection étatique avec `iptables`

1. **Vérification des modules** :
   Assurez-vous que les modules du noyau nécessaires pour l'inspection étatique sont chargés.
   ```bash
   sudo modprobe ip_conntrack
   sudo modprobe iptable_filter
   sudo modprobe iptable_nat
   ```

2. **Politique par défaut** :
   Définissez les politiques par défaut pour bloquer tout le trafic entrant et autoriser tout le trafic sortant.
   ```bash
   sudo iptables -P INPUT DROP
   sudo iptables -P FORWARD DROP
   sudo iptables -P OUTPUT ACCEPT
   ```

3. **Autoriser les connexions établies et relatives** :
   Ajoutez une règle pour autoriser les paquets faisant partie de connexions déjà établies ou connexions relatives.
   ```bash
   sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
   ```

4. **Autoriser le trafic local (loopback)** :
   Autorisez le trafic sur l'interface loopback pour permettre aux applications locales de communiquer entre elles.
   ```bash
   sudo iptables -A INPUT -i lo -j ACCEPT
   ```

5. **Autoriser les nouvelles connexions pour certains services** :
   Ajoutez des règles pour autoriser les nouvelles connexions à certains services, par exemple, SSH et HTTP.
   ```bash
   # Autoriser SSH
   sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT

   # Autoriser HTTP
   sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT

   # Autoriser HTTPS
   sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT
   ```

6. **Enregistrer les règles** :
   Pour que les règles soient persistantes après un redémarrage, enregistrez-les dans un fichier de configuration.

   Sous Debian/Ubuntu :
   ```bash
   sudo sh -c "iptables-save > /etc/iptables/rules.v4"
   ```

   Sous CentOS/RHEL :
   ```bash
   sudo service iptables save
   ```

### Exemple de script pour l'inspection étatique

Voici un script shell pour configurer un pare-feu avec inspection étatique :

```bash
#!/bin/bash

# Politique par défaut
echo "Définition des politiques par défaut"
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Autoriser les connexions établies et relatives
echo "Autorisation des connexions établies et relatives"
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Autoriser le trafic local (loopback)
echo "Autorisation du trafic local (loopback)"
sudo iptables -A INPUT -i lo -j ACCEPT

# Autoriser les nouvelles connexions pour certains services
echo "Autorisation des nouvelles connexions pour SSH, HTTP, et HTTPS"
# SSH
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT
# HTTP
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT
# HTTPS
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT

# Enregistrer les règles
echo "Enregistrement des règles"
sudo sh -c "iptables-save > /etc/iptables/rules.v4"

echo "Configuration terminée."
```

### Tests

Pour vérifier que l'inspection étatique fonctionne correctement :

1. **Connexion SSH** :
   Essayez de vous connecter à votre machine via SSH depuis une autre machine.
   ```bash
   ssh [votre_utilisateur]@[votre_ip]
   ```

2. **Accès HTTP/HTTPS** :
   Utilisez un navigateur ou `curl` pour accéder à votre serveur web sur les ports 80 (HTTP) et 443 (HTTPS).
   ```bash
   curl http://[votre_ip]
   curl https://[votre_ip]
   ```

3. **Vérification des connexions établies** :
   Vous pouvez utiliser la commande suivante pour voir les connexions suivies par `conntrack`.
   ```bash
   sudo conntrack -L
   ```
