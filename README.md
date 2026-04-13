Déploiement avec Vagrant (Backend + DB + Frontend)

CRÉATION DES MACHINES VIRTUELLES

Créer le Vagrantfile avec la commande : vagrant init
(voir fichier Vagrantfile)

Démarrer les machines virtuelles : vagrant up

Vérifier que les machines sont démarrées : vagrant status

<img width="728" height="206" alt="Capture d&#39;écran 2026-04-13 085837" src="https://github.com/user-attachments/assets/8d930bf1-317c-478d-8f5d-9822f2bb7d2d" />

CONFIGURATION DE server-dba (BASE DE DONNÉES MYSQL)

Connexion à la VM : vagrant ssh server-dba

Vérifier que MySQL fonctionne : sudo systemctl status mysql

<img width="682" height="251" alt="Capture d&#39;écran 2026-04-13 090316" src="https://github.com/user-attachments/assets/8e093815-7136-464e-a7e7-c141fe988e12" />

Vérifier que MySQL fonctionne : sudo systemctl status mysql

Se connecter à MySQL : mysql

Afficher les bases : SHOW DATABASES;

Vérifier l'utilisateur : SELECT user, host FROM mysql.user;

Tester la connexion avec l'utilisateur créé : mysql -u demo_user -pdemo_pass -h 127.0.0.1 demo_db

Sortir de MySQL : exit;

<img width="898" height="204" alt="Capture d&#39;écran 2026-04-13 090919" src="https://github.com/user-attachments/assets/e6727fe3-9f98-4017-9aa8-4a28854f40d4" />


<img width="762" height="484" alt="Capture d&#39;écran 2026-04-13 091436" src="https://github.com/user-attachments/assets/499dc6f8-d5a2-4323-a4bc-cb3877f655dc" />

CONFIGURATION DE server-back (BACKEND SPRING BOOT)

Connexion à la VM : vagrant ssh server-back

Vérifier Java et Maven : java -version   mvn -version

<img width="764" height="189" alt="Capture d&#39;écran 2026-04-13 092040" src="https://github.com/user-attachments/assets/d3b1da1f-3016-46f6-b46c-448bbb3097a6" />

Se déplacer dans le projet : cd /home/vagrant/backend 

TEST DE CONNEXION MYSQL

Installer client MySQL : sudo apt-get install -y mysql-client 

Tester connexion à la base : mysql -u demo_user -pdemo_pass -h 192.168.56.11 demo_db 

TEST BACKEND

Lancer les tests : mvn test 

<img width="950" height="225" alt="Capture d&#39;écran 2026-04-13 092722" src="https://github.com/user-attachments/assets/7312db94-78c0-4d4f-b365-f3fd9c176d7c" />


BUILD DU BACKEND: mvn clean package

<img width="956" height="328" alt="Capture d&#39;écran 2026-04-13 093423" src="https://github.com/user-attachments/assets/a17527c0-9e49-4ea9-ab56-f0861331c46f" />

DÉMARRAGE DU BACKEND: java -jar target/demo-0.0.1-SNAPSHOT.jar

Tester depuis la machine hôte :  http://localhost:8080/api/products 

<img width="611" height="513" alt="Capture d&#39;écran 2026-04-13 094628" src="https://github.com/user-attachments/assets/d1e71ca3-edc0-4a49-95be-0aa97a199756" />

CONFIGURATION DE server-front (FRONTEND + NGINX)

Connexion : vagrant ssh server-front 

Vérifier Node et npm : node -v    npm -v

<img width="384" height="86" alt="Capture d&#39;écran 2026-04-13 095745" src="https://github.com/user-attachments/assets/997bb693-96d8-41e6-a929-7f88cdc4fc05" />

INSTALLATION DU FRONTEND: cd /home/vagrant/frontend 

executer la commande: npm install

<img width="701" height="153" alt="Capture d&#39;écran 2026-04-13 104032" src="https://github.com/user-attachments/assets/e904ce27-d87f-44c4-b9be-0a7762c926dd" />


BUILD DU FRONTEND: npm run build 

<img width="507" height="278" alt="Capture d&#39;écran 2026-04-13 104415" src="https://github.com/user-attachments/assets/01307099-7009-4e48-b637-612d02b5e5f3" />

CONFIGURATION NGINX

Créer configuration : sudo nano /etc/nginx/sites-available/frontend 

voici le contenu du fichier

<img width="530" height="379" alt="Capture d&#39;écran 2026-04-13 101326" src="https://github.com/user-attachments/assets/75fac728-68dc-4bfa-a4ac-3274298a2af1" />

Activer Nginx :
sudo rm -f /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/frontend /etc/nginx/sites-enabled/

COPIE DU BUILD: sudo mkdir -p /var/www/frontend
                sudo cp -r dist/* /var/www/frontend/

REDÉMARRER NGINX: sudo nginx -t
                  sudo systemctl restart nginx
                  sudo systemctl status nginx

<img width="935" height="243" alt="Capture d&#39;écran 2026-04-13 104517" src="https://github.com/user-attachments/assets/6b0fdaed-2bdb-4872-8261-eb7d2dd8fb51" />


<img width="944" height="987" alt="Capture d&#39;écran 2026-04-13 104625" src="https://github.com/user-attachments/assets/51a4b9f1-3965-471c-b203-07ad6808654f" />

                  


