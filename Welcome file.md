---


---

<h1 id="modul-300---docker-de">Modul 300 - Docker DE</h1>
<h2 id="definition-docker">Definition Docker</h2>
<p>Docker ist eine Virtualiserung ohne Virtualisierung. Docker ist eine Container Technologie. Ein Container fasst eine einzelne Anwendung mitsamt aller Abhängigkeiten wie Bibliotheken, Hilfsprogrammen und statischer Daten in einer Image-Datei zusammen, ohne aber ein komplettes Betriebssystem zu beinhalten. Daher lassen sich Container mit einer leichtgewichtigen Virtualisierung vergleichen.</p>
<h3 id="installation-docker">Installation Docker</h3>
<h4 id="aufsetzen-repository">Aufsetzen repository</h4>
<p>Führe die folgende Schritte aus, um Docker zu installieren:</p>
<ol>
<li>Apt Package Index updaten:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">sudo apt-get update
</code></pre>
<ol start="2">
<li>Installiere die Pakete, welche erlauben Repositorys über HTTPS zu verwenden:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
</code></pre>
<ol start="3">
<li>Füge Docker’s offizieller GPG Key hinzu:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
</code></pre>
<p>Überprüfe, ob du den Key  mit dem Fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88 hast</p>
<pre class=" language-sh"><code class="prism  language-sh">sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) &lt;docker@docker.com&gt;
sub   4096R/F273FCD8 2017-02-22
</code></pre>
<ol start="4">
<li>Erstelle ein Repository mit:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
</code></pre>
<h4 id="installation-docker-1">Installation Docker</h4>
<ol>
<li>Installiere Docker wie folgt:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">sudo apt-get update
sudo apt-get install docker-ce
</code></pre>
<ol start="2">
<li>Überprüfe die Docker Installation mit:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">sudo docker run hello-world
</code></pre>
<h3 id="dockerfile-und-start-container">Dockerfile und Start Container</h3>
<ol>
<li>Erstelle das Dockerfile im Verzeichnis</li>
<li>Erstelle ein Image mit dem Dockerfile, starte und überprüfe es</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">docker build -t apache2

docker run --rm -d --name apache2 apache2

docker exec -t mysql bash
</code></pre>
<ol start="3">
<li>Zeige den aktiven Container an</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">docker ps
</code></pre>
<h2 id="webserver-testen">Webserver Testen</h2>
<p>Um sicher zu sein dass der Apache Server aktiv ist, habe ich folgendes gemacht:</p>
<ol>
<li>Browser öffnen</li>
<li>IP eingeben ( 172.17.0.2)<br>
Ausgabe: Default Apache2 Seite</li>
</ol>
<h2 id="firewall-regeln-nachschauen">Firewall Regeln nachschauen</h2>
<pre class=" language-sh"><code class="prism  language-sh">docker-machine ip default

curl http://172.17.0.2:8080
</code></pre>
<h2 id="monitoring">Monitoring</h2>
<p>Über CMD:</p>
<pre class=" language-sh"><code class="prism  language-sh">docker stats
</code></pre>
<p>Mit Cadvisor:<br>
Starte Cadvisor mit:</p>
<pre class=" language-sh"><code class="prism  language-sh">sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
</code></pre>
<p>Im Browser: localhost:8080/containers/</p>
<h2 id="ram-begrenzung">RAM Begrenzung</h2>
<p>Die maximale RAM kann man mit folgendem Befehl festgelegt werden:</p>
<pre class=" language-sh"><code class="prism  language-sh">docker run -m 2096m --memory-swap 2096m
</code></pre>
<h3 id="docker-user-erstellen">Docker User erstellen</h3>
<p>Erstelle ein User mit dem folgendem Befehl:</p>
<pre class=" language-sh"><code class="prism  language-sh">RUN groupadd -r User_Group &amp;&amp; useradd -r -g User_group athikatesting
</code></pre>
<p>Mit diesem Befehl loggt man sich ein:</p>
<pre class=" language-sh"><code class="prism  language-sh">docker run -ti name:Version /bin/bash
su athikatesting
</code></pre>
<h1 id="eigener-container-erstellen">Eigener Container erstellen</h1>
<ol>
<li>Vagrant File zu Dockerfile umwandeln</li>
<li>Im verzeichnis gehen, wo das Dockerfile ist</li>
<li>Dockerfile builden:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">docker build -t athika .
</code></pre>
<ol start="4">
<li>Dockerfile starten mit:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">docker run --rm -d --name athika athika
</code></pre>
<ol start="5">
<li>Funktionsfähigkeit überprüfen:</li>
</ol>
<pre class=" language-sh"><code class="prism  language-sh">docker exec -it athika bash
</code></pre>
<p>und im Container</p>
<pre class=" language-sh"><code class="prism  language-sh">ps -ef
netstat -tulpen
</code></pre>

