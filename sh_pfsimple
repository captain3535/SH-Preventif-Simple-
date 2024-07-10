#!/bin/sh

# Script de pare-feu préventif pour détecter et bloquer temporairement les attaques DDoS sur le trafic HTTP

# Fonction pour afficher un message de détection
print_detection() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - [ATTENTION] $1"
}

# Fonction pour bloquer temporairement toutes les adresses IP attaquantes avec iptables
block_all_ips() {
    # Récupérer toutes les adresses IP attaquantes
    attacking_ips=$(grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" /var/log/nginx/access.log | sort | uniq)
    # Bloquer chaque adresse IP
    for ip in $attacking_ips; do
        sudo iptables -A INPUT -s $ip -j DROP
    done
    # Attendre 30 secondes
    sleep 30
    # Débloquer toutes les adresses IP bloquées
    for ip in $attacking_ips; do
        sudo iptables -D INPUT -s $ip -j DROP
    done
}

# Début du programme
echo "--------------------------------------------"
echo "|      Pare-feu préventif de détection      |"
echo "|        DDoS pour le trafic HTTP           |"
echo "--------------------------------------------"
echo "$(date '+%Y-%m-%d %H:%M:%S') - Démarrage du pare-feu..."

# Boucle principale surveillant le fichier de logs en temps réel
tail -n0 -F /var/log/nginx/access.log | while read -r line; do
    # Vérifier si la ligne de log correspond à une requête HTTP GET
    if echo "$line" | grep -q "\"GET /"; then
        # Vérifier la fréquence des requêtes
        total_requests=$(grep -c "\"GET /" /var/log/nginx/access.log)
        if [ "$total_requests" -gt 10 ]; then
            print_detection "Attaque DDoS détectée - Bloquage temporaire de toutes les adresses IP attaquantes pendant 30 secondes..."
            block_all_ips
            echo "$(date '+%Y-%m-%d %H:%M:%S') - Toutes les adresses IP attaquantes débloquées."
        fi
    fi
done
