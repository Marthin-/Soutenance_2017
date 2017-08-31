---

### Industrialisation de déploiement de plates-formes d'hébergement Internet

>> Merci à Benjamin NGUYEN, Mathieu BLANC, Patrick MARMAYOU

---

### Plan

<ul>
<li><p class="fragment">Présentation du sujet</p></li>
<li><p class="fragment">Environnement à industrialiser</p></li>
<li><p class="fragment">Puppet ^ Ansible </p></li>
</ul>

---

Présentation du sujet

<h4><span class="fragment"><span class=fragment">Industrialisation</span></span><span class="fragment"> de déploiement de plates-formes</span><span class="fragment"> d'hébergement Internet</span></h4>

---

### Quelques définitions

<section>
<p>Industrialisation</p>
<blockquote style="width:90%">À la fois réflexion sur l'amélioration de l'efficience d'un système, et ensemble des processus qui permettent cette amélioration.</blockquote>
</section>
<section>
<p>Automatisation</p>
<blockquote style="width:90%">Action d'interconnecter des élements disparates d'un système de manière à rendre ce système auto-géré, améliorant ses performances en diminuant le nombre d'interventions requises.</blockquote>
</section><section>
<blockquote>L'automatisation est donc un des moyens d'industrialiser un système</blockquote>
</section>
---

<h3>Environnement à industrialiser</h3>
<section>
* Plus de 150 hosts
* Entre 3 et 10 VM par host
* Deux DC
* Firewalls
</section>

<section>
* Hosts : CentOS / Ubuntu
* VM : Debian / OracleLinux 
* Firewalls : OpenBSD
</section>

---

<h3>Environnement à industrialiser</h3>

<section>
* Architectures web classiques
	* serveurs front (apache, nginx...)
	* serveurs BDD (postgresql, mariadb, percona...)
	* Serveurs de préproduction (tests pour les développeurs)
	* Serveurs de back-office, d'ERP...
</section>

<section>
* Infrastructure Smile Hosting
	* Monitoring (Shinken, nrpe, suri5, cacti)
	* Groupware/Communication clients (Redmine)
	* Backups (scripts, bacula)
	* Firewalls (packetfilter, iptables)
	* Puppet master
	* Gestion des info. machines (racktables)
</section>

---

<h3>Industrialisation du système</h3>

<section>
* Déploiement des hôtes de virtualisation
	* Installation de l'OS
	* Configuration réseau
	* Provisionning
</section><section>
* Configuration des VM/containers
	* Contrôle d'accès (création d'users, clefs ssh, identifiants ftp...)
	* Outils système (cron, logrotate...)
	* Configuration des composants web
		* conf. serveur web
		* conf. BDD / réplications...
	* Configuration de l'infrastructure
		* Gestion des firewalls
		* Gestion des configurations de monitoring
	* Gestion/Config. des backups
</section><section>

* Déploiement des sites web
	* préprod / prod
	* config. backend (php, magento...)

* Gestion de l'infrastructure
	* Gestion des firewalls
	* Gestion du monitoring
	* Gestion des serveurs de backup

</section>

---

### Puppet ^ Ansible

* Deux systèmes d'automatisation
* Deux modèles qui s'opposent
* Deux outils open source

---

##### Puppet ^ Ansible

* Agents vs Agentless
* Ruby vs Python
* Serveur vs Exécution locale
* Puppet language vs Yaml
* Complétude vs Simplicité
* Environnements vs Groupes

---

### Puppet

<ul>
<li><p class="fragment">Ruby</p>
<li><p class="fragment">Modèle agents-serveur (puppetmaster) : run régulières</p></li>
<li><p class="fragment">Langage spécifique</p><ul>
<span class="fragment"><li>Proche d'un langage de prog.</li>
<li>Variables modifiables en puppet, fonctions en ruby</li>
<li>Description d'états</li></ul></li></span>
<li><p class="fragment">Environnements d'exécution</p></li>

---

#### Puppet

```puppet
class helloworld::motd {

    file { '/etc/motd':
    owner  => 'root',
    group  => 'root',
    mode    => '0644',
    content => "hello, world!\n",
    }
 }
```

---

#### Ansible

<ul>
<li><p class="fragment">Python</p>
<li><p class="fragment">Modèle agentless : exécution à la demande</p></li>
<li><p class="fragment">Communication via ssh</p></li>
<li><p class="fragment">Utilise yaml</p><ul>
<span class="fragment"><li>Variables accessibles en python</li>
<li>Description d'états</li></ul></li></span>
<li><p class="fragment">Génère des scripts
<li><p class="fragment">Groupes de machines</p></li>

---

#### Ansible

```yaml

---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum: name=httpd state=latest
  - name: write the apache config file
    template: src=/srv/httpd.j2 dest=/etc/httpd.conf
    notify:
    - restart apache
```

---

### Puppet ^ Ansible

<section>
Un même besoin, deux approches et deux utilisations :

* Provisionning : Ansible
	* Plus adapté aux run uniques
	* Simplicité d'utilisation
		* Permet d'effectuer des actions simples sur un grand nombre de serveurs à la fois
		* Ne nécessite pas d'infrastructure
		* Limité par sa simplicité
</section><section>
* Configuration : Puppet
	* Modèle adapté aux run récurrentes
	* Langage plus évolué
		* Apporte une plus fine granularité dans la configuration
	* Produit plus mature qu'Ansible
		* Moins de breaking changes
		* Solutions communautaires plus nombreuses
</section>

---

<h2>Conclusion</h2>
