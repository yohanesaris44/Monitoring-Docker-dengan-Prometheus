# Monitoring-Docker-dengan-Prometheus

1. Configurasi Docker
Konfigurasi Daemon Docker sebagai target Prometheus,dengan menentukan alamat-metrik. Cara terbaik untuk menentukan ini adalah melalui daemon.json
Buatlah deamon.json di 

  Linux: /etc/docker/daemon.json
  
  Windows Server: C:\ProgramData\docker\config\daemon.json
Dengan isinya berikut


{
 
“metrics-addr” : “127.0.0.1:9323”,
 
“experimental” : true
 
}


2. Configurasi dan run Prometheus
Dalam contoh ini Prometheus berjalan sebagai layanan Docker pada kawasan Docker. Satu atau lebih mesin doker bergabung dengan sekumpulan docker, dengan menggunakan docker swarm, dan pada satu manajer dan buruh pelabuhanberkumpul di manajer dan node pekerja.
/tmp/prometheus.yml  ini adalah konfigurasi prometheus 

# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  external_labels:
      monitor: 'docker'
rule_files:
 

scrape_configs:

  - job_name: 'prometheus'

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9323']

3. Menjalankan service prometheus di dalam docker 
docker service craate \
--teplicas 10\
--name ping_Service \
Alpine ping docker.com


4. Target yang ingin dituju adalah Docker yang mana diakses do browser dengan mengetikkan localhost http://localhost:9323


