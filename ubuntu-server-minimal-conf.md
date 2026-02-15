Ubuntu Server Einrichtung: ws-forms.comDiese Anleitung beschreibt die Einrichtung eines Apache Virtual Hosts auf einem Ubuntu System.1. Verzeichnisstruktur & BerechtigungenZuerst legen wir das Projektverzeichnis an. Der aktuelle User bleibt Besitzer, die Gruppe wird www-data.Bash# Verzeichnis erstellen
sudo mkdir -p /var/www/ws-forms

# Gruppe auf www-data setzen (Besitzer bleibt dein User)
sudo chown -R :www-data /var/www/ws-forms

# RECHTE-OPTIMIERUNG:
# Alle Ordner auf 775 setzen (drwxrwxr-x) -> Gruppe darf in Ordner wechseln
sudo find /var/www/ws-forms -type d -exec chmod 775 {} \;

# Alle Dateien auf 664 setzen (-rw-rw-r--) -> Gruppe darf Dateien bearbeiten
sudo find /var/www/ws-forms -type f -exec chmod 664 {} \;

# Setgid-Bit setzen (neue Dateien erben automatisch die Gruppe www-data)
sudo chmod g+s /var/www/ws-forms
2. Apache Virtual Host KonfigurationErstelle die Konfigurationsdatei:sudo nano /etc/apache2/sites-available/ws-forms.confInhalt der Datei:Apache<VirtualHost *:80>
   ServerAdmin test@example.de
   DocumentRoot /var/www/ws-forms/
   ServerName ws-forms.com

   ErrorLog ${APACHE_LOG_DIR}/error.log
   CustomLog ${APACHE_LOG_DIR}/access.log combined

   <Directory /var/www/ws-forms/>
   AllowOverride All
   Require all granted
   </Directory>
   </VirtualHost>
3. Seite aktivieren & Apache neu ladenBash# Seite aktivieren
   sudo a2ensite ws-forms.conf

# Syntax der Konfiguration prüfen
sudo apache2ctl configtest

# Apache neu laden
sudo systemctl reload apache2
4. Lokale DNS-Auflösung (Hosts-Datei)sudo nano /etc/hostsFolgende Zeile hinzufügen:Plaintext127.0.0.1   ws-forms.com
5. Nützliche Befehle zur VerwaltungAktionBefehlSeite deaktivierensudo a2dissite ws-forms.confApache Status prüfensudo systemctl status apache2Error Log einsehensudo tail -f /var/log/apache2/error.log
