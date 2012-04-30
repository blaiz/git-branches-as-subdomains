Server:
Create configuration file for Apache and paste content of localhost-vhosts file inside to make it work with this Git structure
sudo su
a2enmod vhost_alias
nano /etc/apache2/conf.d/vhosts
service apache2 restart


Create the bare repository on the server:
mkdir -p /git/com/example.git
cd /git/com/example.git
git --bare init


Copy needed Git hooks in bare repo:
cp /path/to/post-* /git/com/example.git/hooks/
chmod +x /git/com/example.git/hooks/*


Client:
Clone that repository on the client:
mkdir -p /git/com
ln -s /git /web
cd /web/com/
git clone ssh://user@server/git/com/example.git


(Optional) Create the master branch
cd example/
Add or create new files in domain/www/
git add -A
git commit -m "Initial commit"
git push origin master


The website should now be available at www.example.com


Test website locally (for Ubuntu):
apt-get install bind9
cp /etc/bind/db.local /etc/bind/db.local.bak
echo -n "*.localhost. IN A " >> /etc/bind/db.local
ifconfig eth0 |grep "inet addr" |awk '{print $2}' |awk -F: '{print $2}' >> /etc/bind/db.local
service bind9 restart
echo "nameserver 127.0.0.1" >> /etc/resolv.conf
exit
Edit /etc/apache2/conf.d/virtual file to make it work with this Git structure


The website should now be available at www.example.com.localhost


Installing and using Git Flow:
Download the git-flow package for Precise Pangolin from the Ubuntu website
Install the package by double clicking on it
Now, in a git folder, initialiaze Git Flow with:
git flow init
Choose these names for branches:
1. develop
2. feature-
3. release-
4. hotfix-
5. support-
We cannot keep the default names (e.g. release) because it will be used as a subdomain, and subdomain names can't contain a forward slash. For the same reason, do not put an underscore in a branch name. Only characters a to z, 0 to 9 and hyphen.
To start a new feature branch:
git flow start test-branch
git push origin feature-test-branch
To merge back and delete a branch:
git flow finish test-branch
git push origin --delete feature-test-branch
For more commands, please check these examples on the git flow GitHub, and this blog post.


Inspiration/sources:
1. http://joemaller.com/990/a-web-focused-git-workflow/
2. http://superuser.com/questions/103516/apache-serve-files-from-git-repo-branches
3. http://httpd.apache.org/docs/2.2/vhosts/mass.html
4. http://nvie.com/posts/a-successful-git-branching-model/
5. http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/



Known issues:
1. Can’t go more than 2 subdomains deep but I don’t think that’s a big issue since we probably won’t have websites with a name composed of more than 2 subdomains. Example: test.lab.example.com will work, but not client.test.lab.example.com