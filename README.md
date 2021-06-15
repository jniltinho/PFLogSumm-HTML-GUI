# PFLogSumm-HTML-GUI
Bash shell script to Generate POSTFIX statistics HTML report using pflogsumm as the backend

The script processes the pflogsumm output to an easy to view HTML report

## Screenshots of the web interface

![Screenshot1](Screenshot1.png)

![Screenshot1](Screenshot2.png)


## Requirements 

*pflogsumm* needs to be installed

For RedHat based operating systems

```
yum -y install postfix-perl-scripts 
```

For Ubuntu/Debian based operating systems

```
apt-get -y install pflogsumm 
```

## Install ISPConfig Webmail

You can clone or download the script direct to a location of your choice. Here is an example setup:
```
cd /opt
git clone https://github.com/jniltinho/PFLogSumm-HTML-GUI.git
mkdir -p /var/www/webmail.domain.com/web/stats/grafico

echo '#PFLOGSUMUI CONFIG

##  Postfix Log Location
LOGFILELOCATION="/var/log/mail.log"

##  pflogsumm details
PFLOGSUMMOPTIONS=" --verbose_msg_detail --zero_fill "
PFLOGSUMMBIN="/usr/sbin/pflogsumm  "

##  HTML Output
HTMLOUTPUTDIR="/var/www/webmail.domain.com/web/stats/grafico/"
HTMLOUTPUT_INDEXDASHBOARD="index.html"
' >/etc/pflogsumui.conf

/opt/PFLogSumm-HTML-GUI/pflogsummUIReport.sh

## Show web
## https://ip_ispconfig_admin:8080/grafico/
```

## Script updates

If you want to update to the latest version you can run this (provided you used GIT to install)

```
cd /opt/PFLogSumm-HTML-GUI
git pull
```

That will ensure the latest version


## Configuration

On first time run the script will automatically  create the default configuration file: /etc/pflogsumui.conf

```
#PFLOGSUMUI CONFIG

##  Postfix Log Location
LOGFILELOCATION="/var/log/mail.log"

##  pflogsumm details
PFLOGSUMMOPTIONS=" --verbose_msg_detail --zero_fill "
PFLOGSUMMBIN="/usr/sbin/pflogsumm  "

##  HTML Output
HTMLOUTPUTDIR="/var/www/webmail.domain.com/web/stats/grafico/"
HTMLOUTPUT_INDEXDASHBOARD="index.html"

```

The parts that might need changing according to your environment  is:

LOGFILELOCATION and HTMLOUTPUTDIR

The default locations are REDHAT/CENTOS based operating systems

## Create a crontab 

The script needs to run once a day to update the reports using CRON. Note, the scripts need access to the maillog as root or a SUDO user with access to the maillog and web directories.

### Example crontab entry for 11:50 PM

Because we want the report for the previous day, we run this report one minute before midnight

```
50 11 * * * /opt/PFLogSumm-HTML-GUI/pflogsummUIReport.sh >/dev/null 2>&1
```
## Note about ZIMBRA (if you are using it)

Zimbra: The World's Leading Open Source Email Collaboration Solution

Zimbra installs its own pflogsumm script. If you want to use that script instead you can create a symlink to fix the script paths

```
ln -s /opt/zimbra/common/bin/pflogsumm.pl /usr/sbin/pflogsumm
```

# WARNING: The reports expose user email accounts. You MUST password protect the directory you are hosting the files in
