Script de Pare-feu Préventif pour Détection et Blocage des Attaques DDoS
Ce script Shell permet de détecter et de bloquer temporairement les attaques DDoS (Distributed Denial of Service) sur un serveur web utilisant Nginx. Il surveille les fichiers de logs Nginx en temps réel, identifie les requêtes HTTP GET suspectes et bloque les adresses IP attaquantes en utilisant iptables.

Fonctionnalités
-Surveillance en temps réel des logs Nginx.
-Détection des requêtes HTTP GET suspectes.
-Blocage temporaire des adresses IP attaquantes pendant 30 secondes.
-Déblocage automatique des adresses IP après le délai.

Prérequis
-Serveur Nginx configuré et en fonctionnement.
-Fichier de logs Nginx accessible (généralement situé à /var/log/nginx/access.log).
-Permissions sudo pour exécuter les commandes iptables.


Installation :


Clonez ce dépôt sur votre serveur puis rendez le script exécutable :

chmod +x ddos_firewall.sh


Utilisation

-Exécutez le script avec les permissions nécessaires :

sudo ./ddos_firewall.sh

Le script affichera les messages de démarrage et surveillera les logs Nginx en temps réel.
