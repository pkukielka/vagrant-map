## vagrant-map

### Vagrant development environment for experimenting with maps and GIS


#### Motivation
Did you ever wanted to play a bit with Open Street Maps or GIS technologies in general  
but were discouraged by the amount of manual configuration needed to get started?  
If so, you found solution to your problem right now.


#### So, what is a solution?
Vagrant-map is minimal complete, fully configured environment which works  
out of the box, and allow you to, for example, view some maps in your browser.  
Right now provided VM contains:

  - PostgreSQL v. 9.1
  - Mapnik v. 2.1
  - Apache 2 with mod_tile
  - psycopg2, phppgadmin, osm2pgsql and mapnik-stylesheet
  - Some additional utilities (look at provisioning/playbooks/apt-get.yml to see all installed packages)

All automatically installed and configured to work with each other.

#### Installation
To build your own Vagrant VM you need to have latest versions of 
[Vagrant](http://www.vagrantup.com/) and [Ansible](http://ansible.cc/).  
If you don't have it yet, you need to install git and VirtualBox as well.

```bash
git clone https://github.com/pkukielka/vagrant-map.git
cd vagrant-map
vagrant up
```  

That's all you need to be up and running.  
Depending on your connection building VM can take a while  
(~300MB for VM and ~400MB for coastline data need to be dowloaded).

Now you can login to your virtual machine by issuing the following command:

```bash
vagrant ssh
```

Thanks to port redirection you can look at imported maps in your host browser.  
Just go to [http://locahost:8080](http://locahost:8080) to see world map. You can also check
[Cracow](http://localhost:8080/?zoom=15&lat=50.06013&lon=19.94137&layers=B) or
[New York](http://localhost:8080/?zoom=10&lat=40.84975&lon=-73.81733&layers=B).

#### Hacking
I tried to keep everything as simple as possible, so it should be easy to add any custom modifications ;)  
For start, if you want to add custom maps which should be loaded please look at provisioning/vars.yml file.  
Variable 'db_imports' is what you need. You can also change default user name and database name here,  
but it's not advised. On the other hand feel free to change your password right away.

If for whatever reason you don't want to run separate virtual machine but rather install everything on your  
host machine, then it's also possible with ansible:

```bash
cd vagrant-map/provisioning/playbooks
ansible-playbook main.yml
```

Just be sure that you have only localhost added to your ``` /etc/ansible/hosts``` or manually  adjust host  
in all vagrant-map playbooks to match your machine name. If this description doesn't make sense to you  
try to read about [ansible inventory & patterns](http://ansible.cc/docs/patterns.html) first.

If for any reason you will need to create next database you can use 'postgis_template' template:

```bash
createdb -T postgis_template new_database
```

#### ToDo

- Create separate playbook with postgresql, postgis and whole environment optimizations
- Change default database encoding to utf-8
- ...?