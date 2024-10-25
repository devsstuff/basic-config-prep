grep -c "^bird" /usr/share/dict/words |head

grep -c "^bird" /usr/share/dict/words | head

grep -n "food" /usr/share/dict/words | head

grep -n "cat" /usr/share/dict/words | head

grep -E "^[a-zA-Z]{12}$" /usr/share/dict/words | head

grep -E "^[a-zA-Z]{7}$" /usr/share/dict/words | head

find / -type f -name "password*"

find / -type f -name "*.config"

. je za trenutni dir

WIN + R
 - type charmap za specijalne simbole
 - 
GREP / AWK
  
grep vs awk

uglavnom kad koristimo regex, u awk dodamo uzorak izmedju / /, npr grep ‘yes’ I ‘awk /yes/’

Find all words that start with "z" grep "^z", grep -c "^z"  – count of words

grep -c "^bird" /usr/share/dict/words

grep -c "^mom" /usr/share/dict/words

Find all words that contain an apostrophe grep “ ’ ”

Find all words that end with an apostrophe followed by "s" grep “ ‘ s$”

Find all words that contain an accented character 

      awk '/[\x80-\xFF]/' ili 
      grep -P "[\x7f-\xff]"

Words with special characters 

     grep -P '\w*[^\x00-\x7F]\w*' ili awk '/\w*[^\x00-\x7F]\w*/'

Find all words that have exactly five letters 

     grep -E “^[a-zA-Z]{5}$ ” but length is 5
     5 letters no mater length grep -o -E “[a-zA-Z]{5}”
     5 letters no matter letters or numbers grep “^.{5}$”
     5 letters  grep -E -o '\b\w{5}\b'
     
Find all words that start with a capital letter grep “ ^ [A-Z]”

Find all words that have a special character (e.g., é, Å).

Find all words that end with the letter "e" grep “ e$ ”

Find all words that are exactly six characters long grep “ ^.{6}$ ”

Find all words that contain at least one vowel (a, e, i, o, u) grep “ [aeiou] ”

Find all words that start with 'Al' and are followed by any number of letters or apostrophes grep  "^Al[a-z0-9']*"

Find all words that end with 's' or 'es' grep -E  "s$|es$"

Find all words that end with a letter but have a single apostrophe or nothing following it grep -E -c "[a-zA-Z]|'?$"

Find all words that contain the substring 'Alma' anywhere in the word grep “Alma”

Find all words that are exactly two characters long and contain at least one letter grep -E "^[a-zA-Z]{2}$" ili grep -E "^[a-zA-Z][a-zA-Z]$"

Find all words where the second character is a vowel (a, e, i, o, u) grep -E "^[a-zA-Z][aeiuo]"

Find all words that start with 'A' and have exactly seven characters grep -E "^A[a-zA-Z]{6}$"

Find all words that contain at least one accented character and have more than 5 characters grep -P "[À-ÿ].{5,}" – install pcregrep

Find all words that are exactly three letters long and end with 'n' grep -E "^.{2}n$" ili grep -E "^..n$"

Find all words where the first letter is 'A' and the last letter is 'a' grep -E "^A[a-zA-Z0-9]*a$" ili “^A.*a$”

Find all words that contain the substring 'son' in them grep  "son"

Find all words that contain digits (0-9) and are exactly 4 characters long grep -E "^[[:alnum:]]{4}$" /usr/share/dict/words | grep "[0-9]"

Find all words that contain 'A' followed by exactly three letters and end with 's' grep -E  "A[a-zA-Z]{3}s$"  ili "A[a-zA-Z]{3}.*s$"

Find all words that contain exactly one apostrophe grep -E "'{1}"

Find all words that end with 'ed' and have exactly 6 characters grep -E "^.{4}ed$"

Find all words that do not contain any vowels (a, e, i, o, u) grep -E '^[^aeiouAEIOU]+$'

find / -type f -name “config” | wc -l  - files with config in name

find / -type f -name “*.config” | wc -l – files with ending with .config

find / -type f  -exec grep -l “config” {} \; | wc  -l – files with content having “content” in it

**Naredbe kojima zelimo doci do info o procesima**

ps -eo comm,pid  – podaci vezano za proces

ps -eo comm,%cpu  --sort=comm ili --sort=-comm

ps aux | grep apache2  - podaci za proces

ps faux | grep apache2

ps faxl | head  -  da vidimo nazive kolumni

ps faxl | grep mysqld – npr, tu mozemo vidjeti I PID procesa, takodjer podaci za proces, child procese koje dodaje 2 po procesorskoj jezgri

Ovdje mozemo vidjeti STAT kolumnu (status), Ss – S sleeping status, s- session leader, R – running, + foreground process (aktivan process), l (Multithreaded) - The process is multithreaded
systemctl status apache2 vrati main pid

Koristio sam naredbe systemctl status mysqld, ps faux i ps faxl jer sam zelio vidjeti podatke vezane za status procesa (aktivan, neaktivan, main PID), PRI, NI, memoriju, cpu i STAT.
Podaci za STAT: Ss – S sleeping status, s- session leader, R – running, + foreground process (aktivan process), l (Multithreaded) - The process is multithreaded

ulimit – naredba kojom podesavamo odredjene limite za usera, npr. Ulimit -n broj fajlova koje korisnik moze otvoriti

conf file gdje se podesavaju pri I ni procesa koje korisnik pokrece (default) **/etc/security/limits.conf**

takodjer se tu podesavaju I broj fajlova koje korisnik moze otvoriti, nofile postavka, nproc je postavka za broj procesa

dodamo npr. za usera da moze otvoriti 12000 fajlova

**student soft nofile 12000** – mislim da je samo sa soft radilo, ako ne, stavi oba

**student hard nofile 12000** – hard treba biti maksimalna vrijednost

Imamo I vrijednosti priority I nice u conf fajlu

student hard priority 3  - npr. za sve procese koje starta user imaju 3 prioritet

Limiti se vjerojatno ucitavaju prilikom nove sesije, tako da vjerojatno nanovo login

cp /etc/security/limits.conf /etc/security/limits.d/student.conf

nakon sto odredimo PID procesa naredbom renice postavimo priority

sto je vrijednost pri veci prioritet procesa je manji, tj. Ako je manja vrijednost pri, ona je prioritet procesa veci. S NI mijenjamo PRI, tj. Samo vrijednost NI mijenjamo. NI ide od -20 do 19

**renice 19 -p <PID>**

**Postavi da webserver radi s upaljenim firewallom**

Provjerimo je li http servis dodan u public zone firewalld. Ako ne trebamo taj servis dodati u public zone od firewalld 
**sudo firewall-cmd --list-all**

Ovom naredbom to dodajemo **sudo firewall-cmd --add-service http --zone public**

Ako restartamo firewall podesenje nece ostati, zato ovom naredbom postavimo da ostane
**sudo firewall-cmd --runtime-to-permanent**

Provjerimo **sudo firewall-cmd --list-all**

**sudo firewall-cmd --list-all-zones** is listing all zones.

  - list public zone**

**Postavi da webserver radi s upaljenim selinuxom**

Da bi server radio s upaljenim selinuxom potrebno je omoguciti opciju za http servis da radi s odredjenim portom, kako je defaultni port 80 provjeriti cemo 
je li omogucen rad s istim
**sudo semanage port -l | grep http**

Provjeriti cemo i postavke za httpd
**sudo getsebool -a | grep httpd**

Provjeri selinux sa sestatus
Ako zelimo da selinux nakon pokretanja radi u Permissive nacinu rada, to mozemo podesiti u 
sudo nano **/etc/selinux/config** fajlu gdje dodamo **SELINUX=permissive**

Provjerimo selinux opcije za http servis je li omogucen rad s portom 80,
**sudo semanage port -l | grep http**

po defaultu je moguce da selinux to vec ima podeseno pa zato vjerojatno ne treba nista postaviti ako app radi na port 80

Provjeri postavke za httpd
**sudo getsebool -a | grep httpd**

**Postavi da webserver radi s upaljenim selinux na portu 99**
Najprije omogucimo port 99 u httpd

sudo nano /etc/httpd/conf/httpd.conf I dodaj Listen 99, provjeri I s sudo netstat -tulnp

Allow port 99 in firewall

**sudo firewall-cmd --add-port 99/tcp --zone public**

**sudo firewall-cmd  --runtime-to-permanent**

Provjeri sudo **semanage port -l | grep http**
Dodaj port u listu http_port_t **sudo semanage port -a -t http_port_t -p tcp 99**
Provjeri sudo **semanage port -l | grep http**

**Podesi posluzivanje aplikacije iz foldera koji nije u /var/www, npr. /web-aplikacija**
Dodaj folder, kreiraj vhost I podesi da gadja /web-aplikacija

Provjeri kontext /var/www I /web-aplikacija da mozemo vidjeti sto t
rebamo podesiti 

sudo ls -laZ /var/www
sudo ls -laZ /web-aplikacija

We need to change contenxt from default to httpd_sys_content_t. We will change it to all
under /web-aplikacija folder.

**sudo semanage fcontext -a -t httpd_sys_content_t "/web-aplikacija(/.*)?"** - ovo radi za sve foldere unutar

sudo restorecon -Rv /web-aplikacija – restore configuration

Add to web-aplilacija httpd conf file
<Directory /web-aplikacija>
	Require all granted
</Directory>
curl http://www.test.local:99

**Postavi da mysql radi s upaljenim firewallom I selinuxom**
Za testiranje s workstation koristit cemo mysql client se pokusamo spojiti na mysql server na drugom serveru

sudo dnf install mysql -y

Kreiraj usera u mysql koji ima pristup sa svih ip adresa radi lakseg testiranja, ako vec ne postoji

Defaultni port je 3306, ako zelimo dodati drugi port dodajemo ga u 
sudo nano /etc/my.cnf.d/mysql-server.cnf

I dodaom opciju npr. port=3307, ali najprije ugasi selinux, firewalld I mysql tako da nema problema oko promjene porta.

Dalje omogucujemo taj port u selinux I firewalld

**MYSQL I SELINUX**

Provjerimo koji su portovi omoguceni 
**semanage port -l | grep mysql**

**sudo semanage port -a -t mysqld_port_t -p tcp 3307**

**MYSQL I FIREWALL**

Provjerimo I u firewall servise I portove u public zoni
**sudo firewall-cmd --list-all** 

Dodajmo servis mysql
**sudo firewall-cmd --add-service mysql --zone public** 

Dodajmo port 3306 jer nije dodan
**sudo firewall-cmd --add-port 3306/tcp --zone public**

Postavimo da se postavke zadrze
**sudo firewall-cmd  --runtime-to-permanent**

Provjeri

**sudo firewall-cmd --list-all** 

Na workstation testiramo pristup ako smo instalirali mysql client

mysql -u user -p -h <ip>  - ako se spajamo na defaultni port 3306
mysql -u user -p -h <ip> -P 3307 – ako se spajamo na port 3307

**Processes**

1.Find out the list of processes with names sorted in alphabetical order ran by
root user and output it in alphabetical order
ps -u root u –sort=comm

2. From the list in the 1 st task, select only the process names
ps -u root u –sort=comm | awk '{print $11}'

4. From the list in the 2 nd task, add the parameters for each of the processes
ps -u root -eo args --sort=comm

6. Find out which process uses the most CPU cycles
ps -eo comm,%cpu --sort=-%cpu --no-headers | head -n 1

8. Find out which process has the 2 nd highest memory usage
ps -eo pid,comm,%mem --sort=-%mem --no-headers | head -n 2 | tail -n 1

10. Find out which process has been running for the longest
ps -eo pid,comm,etime --sort=etime --no-headers | head -n 1

**Regex**

1. From the file /usr/share/dict/words, select all the rows containing the word `fish`.
grep 'fish' /usr/share/dict/words

3. From the file /usr/share/dict/words, select all the rows containing the word `cat`. Also
select 2 rows before each row and 1 after.
grep -B 2 -A 1 'cat' /usr/share/dict/words

5. From the file /usr/share/dict/words, print the number of occurrences of the word `cat`.
grep -o -w 'cat' /usr/share/dict/words | wc -l

7. From the file /usr/share/dict/words, select all the rows containing the word `cat` as well
as the row number. Which row shows the word `catalog`?
grep -n 'cat' /usr/share/dict/words | grep 'catalog'
grep -n 'catalog' /usr/share/dict/words

9. From the file /usr/share/dict/words, select all the rows containing the letter `t`, a vowel
after the `t` and ending with `sh`.
grep 't[aeiou].*sh' /usr/share/dict/words

11. From the file /usr/share/dict/words, select all the rows that will match the words:
`abominable`, `abominate`, `anomie` and `atomize` exactly. Other rows must not be
selected.
grep -E '^(abominable|abominate|anomie|atomize)$' /usr/share/dict/words

13. From the file /usr/share/dict/words, how many words are there that contain the letter
`t`, have a vowel after the `t` and end with `sh`?
grep -c 't[aeiou].*sh' /usr/share/dict/words

15. From the file /usr/share/dict/words, select all the words that are exactly 14 characters
long.
grep -E '^.{14}$' /usr/share/dict/words

17. From the file /usr/share/dict/words, select all the rows that begin with `bl`, have a
vowel after `bl`, and anything after the vowel.
grep -E '^bl[aeiou].*' /usr/share/dict/words

19. From the file /usr/share/dict/words, select all the words that contain a 2 – digit
number.
grep -E '[0-9]{2}' /usr/share/dict/words

11. From the file /usr/share/dict/words, select all the words that:
12. 
a. Begin with any letter, after which they have the letter `e`, and anything after that OR

b. Begin with a number

This regular expression must be in a single command.

grep -E '^(e.*|[0-9].*)' /usr/share/dict/words

13. From the file /usr/share/dict/words, find the following words:

a. Bank

b. Banking

c. Flunking

d. Walking

This regular expression must be in a single command.

grep -E '^(Bank|Banking|Flunking|Walking)$' /usr/share/dict/words

15. Find all the files in the filesystem that end with `password`.
sudo find / -type f -name '*password'

17. Find all the files in the filesystem that begin with `password`.
sudo find / -type f -name 'password*'

19. Find all the files in the filesystem that are named `password`.
sudo find / -type f -name 'password'

21. Find all the files in the filesystem that contain `password`.
sudo grep -lR 'password' /

**Evo napomena vezano za libre office, iako napravim align screenshota i teksta, screenshot bez razloga zavrsi na iducoj stranici. 
Pa ako mozemo to uzeti u obzir ako za neki rijeseni zadatak slika nije tocno ispod teksta.**
https://student.algebra.hr/pretinac/main.php
https://rha.ole.redhat.com/rha/app/login

dkelava
43<[f0W:
