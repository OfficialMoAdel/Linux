---
tags:
  - linux
excalidraw-autoexport: 2025-04-11
source: https://notes.kodekloud.com/docs/Shell-Scripts-for-Beginners/Course-Introduction/Course-Introduction
Note: https://www.doof.me.uk/xcowsay/
---

Links: [[Linux]]
___
# Bash script
[Linux and Unix exit code tutorial with examples | George Ornbo](https://shapeshed.com/unix-exit-codes/)

## Lectures

#### first 



``` bash
#!/bin/bash
#create by MoAdel to print ls and date
echo -n "Please Enter your firest Name: "
read f_name
echo -n "Please Enter your Last Name: "
read L_name
echo  "Welcome $f_name $L_name to Linux course"
day=`date`
echo today is $day
#end
```
echo "# Bash-script" >> README.md
git init
git add .
git commit -m "first commit"
git branch -M main


``` bash
#!/bin/bash
#create by MoAdel to let || (()) || [] || $(expr $F_num + $S_num) || $(echo $F_num + $S_num | bc) 
echo -n "Please Enter your firest number: "
read F_num
echo -n "Please Enter your Last Name: "
read S_num
#let Resualt=$F_num+$S_num
#Resualt=$(($F_num+$S_num))
#Resualt=$[$F_num+$S_num]
#Resualt=$(expr $F_num + $S_num)
Resualt=$(echo $F_num + $S_num |bc)
echo "this Resualt of oddintion $F_num+$S_num = $Resualt"
#end
```

``` bash
#!/bin/bash
#create by MoAdel to let || (()) || [] || $(expr $F_num + $S_num) || $(echo $F_num + $S_num | bc)
echo  "first cal "
echo -n "Please Enter your firest number: "
read F_num
echo -n "Please Enter your Last number: "
read S_num
echo -n "Please Enter your Arithmetic Operation '+,-,*,/': "
read Operator
Resualt=$[$F_num$Operator$S_num]
echo "this Resualt of oddintion $F_num$Operator$S_num = $Resualt"
#end
```

``` bash
#!/bin/bash
#comments
STR1="Welcome To MoAdel Academy"
echo ${STR1/Academy/chanel}
echo ${STR1,} #first car small
echo ${STR1,,} # car small
echo ${STR1^} # first car capital
echo ${STR1^^} # car capital
echo ${STR1~~} # convert car capital to small and small to capital
STR2="I Love Linux ,Linux is more stable than Windows"
echo ${STR2}
echo ${STR2/Linux/Unix}
echo ${STR2//Linux/Unix}
echo ${#STR2}
echo ${#STR2:6:5}
echo $STR1 $STR2
```

```bash
#!/bin/bash
#comments
if [ $UID == 0 ]
then
echo "Welcome $LOGNAME , You have full acess"
else if [ $UID == 1000 ]
then
echo "Welcome $LOGNAME , You have normal user"
else 
echo "sorry $LOGNAME , You don't have acess"
fi
fi

#///////////////////////////////////////////////

if [ $UID -eq 0 ]
then
echo "Welcome $LOGNAME , You have full acess"
elif [ $UID -eq 1000 ]
then
echo "Welcome $LOGNAME , You have normal user"
else 
echo "sorry $LOGNAME , You don't have acess"
fi

# -eq == equal
# -ne not equal
# -gt > greater than
# -ge >= greater than or equal
# -lt <= less than
# -le <= less than or equal
```

```bash
#!/bin/bash
# Initialize the variable
x=1

# Start the while loop
while [ $x -lt 10 ]
do
  echo "x = $x"
  
  # Increment the variable
  ((x++))
done
```

```bash
#!/bin/bash

for i in {1..100..5}
        do
                echo "Value of i is $i"
done

echo -e "mohamed\nahmed\nali\n" >> names
num=1
for name in $(cat ./names)
        do
                echo "echo name umber $num is: $name"
                let num=$num+1  #|| ((num++))
done
```




```bash
useradd mohamedadel -m -d /home/mohamedadel -p p@ssword -s /bin/bash
```

```bash
#!/bin/bash
echo -e "This Script for adding new users\n"
## Get the user data for crating new user ##
#============================================
echo -n "Please Enter your username: "
read username
echo -n "Please Enter your password: "
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "Congrats the user $username is created"
fi

```
``

```bash
#!/bin/bash
echo -e "This Script for delete users\n"
## Get the user data for delete user ##
#============================================
echo -n "Please Enter your username: "
read username
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  userdel $username
  echo "Congrats the user $username is deleteed"
else
#delete command
echo "The enterd username $username  doesn't already exist"
fi
```

### if
```bash
#!/bin/bash
echo "This utiliy for user managent tasks"
echo "Please Selact from the below list"
echo "1-useradd"
echo "2-userdeletee"
echo "i.e:1 #for useradd"
read var
if [ $var -eq 1 ]
then
	echo -e "This Script for adding new users\n"
## Get the user data for crating new user ##
#============================================
echo -n "Please Enter your username"
read username
echo -n "Please Enter your password"
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd
	if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "Congrats the user $username is created"
	fi


elif [$var -eq 2 ]
	then
	echo -e "This Script for delete users\n"
## Get the user data for delete user ##
#============================================
echo -n "Please Enter your username"
read username
echo -n "Please Enter your password"
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  userdel $username
  echo "Congrats the user $username is deleteed"
else
#delete command
echo "The enterd username $username doesn't already exist"
	fi

else
echo"This Enterd option$var doesn't found"


fi
```

### loop 
```bash
#!/bin/bash
function user_add(){
echo -e "This Script for adding new users\n"
## Get the user data for crating new user ##
#============================================
echo -n "Please Enter your username: "
read username
echo -n "Please Enter your password: "
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd
if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "Congrats the user $username is created"
fi
}
function user_del(){
echo -e "This Script for delete users\n"
## Get the user data for delete user ##
#============================================
echo -n "Please Enter your username: "
read username
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  userdel $username
  echo "Congrats the user $username is deleteed"
else
#delete command
echo "The enterd username $username doesn't already exist"
fi
}
var=1
while [ $var -gt 0 ]
	do
echo "This utiliy for user managent tasks"
echo "Please Selact from the below list"
echo "1-useradd"
echo "2-userdeletee"
echo "3-Exit"
echo "i.e:1 #for useradd"
read var
if [ $var -eq 1 ]
	then
	user_add
elif [ $var -eq 2 ]
	then
	user_del
elif [ $var -eq 3 ]
	then
	var=0
else 
	echo "The entered option $var doesn't exist"
fi
	done
```

### switch
```bash
#!/bin/bash
function user_add(){
echo -e "This Script for adding new users\n"
## Get the user data for crating new user ##
#============================================
echo -n "Please Enter your username: "
read username
echo -n "Please Enter your password: "
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd
if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "$username:$password" | chpasswd
echo "Congrats the user $username is created"
fi
}
function user_del(){
echo -e "This Script for delete users\n"
## Get the user data for delete user ##
#============================================
echo -n "Please Enter your username: "
read username
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  userdel $username
  echo "Congrats the user $username is deleteed"
else
#delete command
echo "The enterd username $username doesn't already exist"
fi
}
select var in "useradd" "userdeletee" "Exit"
do
	case $var in 
		useradd)
			user_add
			;;
		userdeletee)
			user_del
			;;
		Exit)
			break
			;;
		*)
			echo "The enterd option $var doesn't exist"
			;;
		
	esac		
done

```


### Using Function External
```bash
function user_add(){
echo -e "This Script for adding new users\n"
## Get the user data for crating new user ##
#============================================
echo -n "Please Enter your username: "
read username
echo -n "Please Enter your password: "
read -s password
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "$username:$password" | chpasswd
echo "Congrats the user $username is created"
fi
}
```

```bash
function user_del(){
echo -e "This Script for delete users\n"
## Get the user data for delete user ##
#============================================
echo -n "Please Enter your username: "
read username
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  userdel $username
  echo "Congrats the user $username is deleteed"
else
#delete command
echo "The enterd username $username doesn't already exist"
fi
}
```

```bash
#!/bin/bash
# this code for mack somthing like include in any programming language
###
shopt -s expand_aliases
alias include='source'
###
include ./moudles/user_add
include ./moudles/user_del
select var in "useradd" "userdeletee" "Exit"
do
	case $var in 
		useradd)
			user_add
			;;
		userdeletee)
			user_del
			;;
		Exit)
			break
			;;
		*)
			echo "The enterd option $var doesn't exist"
			;;
	esac		
done

```


### Postion Parameter 
```bash
#!/bin/bash
username=$1
password=$2
## check if the enterd username is arady exist or not.
grep -w "$username" /etc/passwd > /dev/null
if [ $? -eq 0 ]
then
  echo "The enterd username $username is already exist"
else
#useradd command
useradd $username -m -d /home/$username -p $password -s /bin/bash
echo "$username:$password" | chpasswd
echo "Congrats the user $username is created"
fi
```

parment bash
	/etc/profile
		export PATH=$PATH:/bash_script
source /etc/profile
ولا كن فى هذه الحالة يتم وضع full path moudles  source myscripts/moudles/user_add



[awesome-lists/awesome-bash: A curated list of delightful Bash scripts and resources.](https://github.com/awesome-lists/awesome-bash?tab=readme-ov-file#shell-package-management)


### 1. 🔁 Loop Through Files in a Directory
```bash
for file in /var/log/*.log; do
  echo "Processing $file"
done
```
📌 Useful for log processing, backups, or applying changes to multiple files.
### 2. 🟢 Check If a Service Is Running
```bash
service_name="nginx"
if systemctl is-active --quiet $service_name; then
  echo "$service_name is running"
else
  echo "$service_name is not running"
fi
```

### 3. 📦 Check Disk Usage and Send Alert
```bash
usage=$(df -h / | grep '/' | awk '{print $5}' | sed 's/%//')
if [ "$usage" -gt 80 ]; then
  echo "Disk usage is at ${usage}%. Time to clean up!" | mail -s "Disk Alert" admin@example.com
fi
```

### 4. 📂 Create Timestamped Backups

```bash
tar -czvf backup_$(date +%F_%T).tar.gz /etc/nginx/
```
🗂️ Great for cron jobs or pre-deployment backups.

### 5. 🌐 Ping Multiple Hosts
```bash
for host in google.com github.com stackoverflow.com; do
  ping -c 1 $host && echo "$host is up" || echo "$host is down"
done
```

### 6. 🔒 Check for Failed SSH Logins
```bash
grep "Failed password" /var/log/auth.log | wc -l
```

### 7. 💻 Monitor CPU Usage
```bash
top -b -n1 | grep "Cpu(s)" | awk '{print $2 + $4 "% CPU Usage"}'
```

### 8. 🧼 Clean Old Log Files (Older Than 7 Days)
```bash
find /var/log -name "*.log" -type f -mtime +7 -exec rm -f {} \;
```


### 9. 🧪 Check If Port Is Open
```bash
port=22
if nc -zv localhost $port 2>/dev/null; then
  echo "Port $port is open"
else
  echo "Port $port is closed"
fi
```

### 10. 🔃 Auto Restart a Service If It's Down
```bash
service="docker"
if ! systemctl is-active --quiet $service; then
  systemctl restart $service
  echo "$service restarted at $(date)" >> /var/log/service_monitor.log
fi
```


### 11. 🔎 List Top 5 Memory-Consuming Processes
```
ps aux --sort=-%mem | head -n 6
```

### 12. 📤 Upload Files to S3 (Using AWS CLI)
```bash
aws s3 cp /backup/file.tar.gz s3://my-bucket-name/backups/
```

### 13. 🔁 Schedule Reboot Every Sunday
Add this to crontab:
```bash
0 3 * * 0 /sbin/shutdown -r now
```

### 14. 🧾 Generate System Report
```bash
echo "CPU Info:" > report.txt
lscpu >> report.txt
echo -e "\nMemory Info:" >> report.txt
free -h >> report.txt
echo -e "\nDisk Usage:" >> report.txt
df -h >> report.txt
```

### 15. 📡 Check Internet Connection
```bash
ping -q -c 1 -W 1 google.com > /dev/null
[ $? -eq 0 ] && echo "Online" || echo "Offline"
```

