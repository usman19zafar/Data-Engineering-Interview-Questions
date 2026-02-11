```Code
Co-pilot
+-----------------+---------------------------+--------------------------------------+
| ls              | list files                | ls ~/                                 |
| cd              | change directory          | cd ~/folder                           |
| pwd             | show path                 | pwd                                   |
| mkdir           | make directory            | mkdir ~/newdir                        |
| rmdir           | remove directory          | rmdir ~/olddir                        |
| rm              | delete file               | rm ~/file.txt                         |
| cp              | copy file                 | cp ~/a.txt ~/b.txt                    |
| mv              | move/rename               | mv ~/a.txt ~/c.txt                    |
| touch           | create file               | touch ~/file.txt                      |
| cat             | print file                | cat ~/file.txt                        |
| less            | view paged                | less ~/file.txt                       |
| more            | view paged                | more ~/file.txt                       |
| head            | first lines               | head ~/file.txt                       |
| tail            | last lines                | tail ~/file.txt                       |
| grep            | search text               | grep "x" ~/file.txt                   |
| find            | search files              | find ~/ -name "*.txt"                 |
| locate          | fast search               | locate file.txt                       |
| which           | command path              | which python                          |
| whereis         | binary/docs               | whereis bash                          |
| chmod           | change perms              | chmod 755 ~/file                      |
| chown           | change owner              | chown user ~/file                     |
| chgrp           | change group              | chgrp staff ~/file                    |
| ps              | process list              | ps aux                                |
| top             | live processes            | top                                   |
| htop            | enhanced top              | htop                                  |
| kill            | stop process              | kill 1234                             |
| killall         | stop by name              | killall python                        |
| df              | disk usage                | df -h                                 |
| du              | folder size               | du -sh ~/                             |
| free            | memory usage              | free -h                               |
| uptime          | system load               | uptime                                |
| who             | logged users              | who                                   |
| whoami          | current user              | whoami                                |
| id              | user identity             | id                                    |
| uname           | system info               | uname -a                              |
| hostname        | machine name              | hostname                              |
| ping            | network test              | ping google.com                       |
| traceroute      | trace hops                | traceroute google.com                 |
| curl            | http client               | curl https://site                     |
| wget            | download                  | wget https://file                     |
| ssh             | remote login              | ssh user@host                         |
| scp             | secure copy               | scp file user@host:~/                 |
| rsync           | sync files                | rsync -av ~/src ~/dst                 |
| tar             | archive                   | tar -cvf a.tar ~/                     |
| gzip            | compress                  | gzip file                             |
| gunzip          | decompress                | gunzip file.gz                        |
| zip             | compress                  | zip a.zip file                        |
| unzip           | extract                   | unzip a.zip                           |
| nano            | open file                 | nano ~/file.txt                       |
| vi              | open file                 | vi ~/file.txt                         |
| vim             | open file                 | vim ~/file.txt                        |
| emacs           | open file                 | emacs ~/file.txt                      |
| echo            | print text                | echo "hi"                             |
| env             | show env vars             | env                                   |
| export          | set env var               | export X=1                            |
| alias           | create shortcut           | alias ll="ls -la"                     |
| unalias         | remove shortcut           | unalias ll                            |
| history         | command log               | history                               |
| clear           | clear screen              | clear                                 |
| date            | show date                 | date                                  |
| cal             | calendar                  | cal                                   |
| sleep           | pause                     | sleep 5                               |
| time            | measure runtime           | time ls                               |
| seq             | number list               | seq 1 10                              |
| cut             | extract columns           | cut -d, -f1 file                      |
| sort            | sort lines                | sort file                             |
| uniq            | dedupe                    | uniq file                             |
| awk             | text process              | awk '{print $1}' file                 |
| sed             | stream edit               | sed 's/a/b/' file                     |
| xargs           | build commands            | cat list | xargs rm                   |
| tee             | split output              | ls | tee out.txt                      |
| diff            | compare files             | diff a b                              |
| patch           | apply diff                | patch < fix.diff                      |
| ln              | create link               | ln -s a b                             |
| mount           | mount device              | mount /dev/sda1 /mnt                  |
| umount          | unmount device            | umount /mnt                           |
| systemctl       | service manager           | systemctl status ssh                  |
| service         | service control           | service ssh restart                   |
| journalctl      | logs                      | journalctl -u ssh                     |
| crontab         | schedule jobs             | crontab -e                            |
| at              | run later                 | echo "ls" | at now+1min               |
| useradd         | add user                  | useradd bob                           |
| userdel         | delete user               | userdel bob                           |
| usermod         | modify user               | usermod -aG sudo bob                  |
| groupadd        | add group                 | groupadd dev                          |
| groupdel        | delete group              | groupdel dev                          |
| passwd          | change password           | passwd bob                            |
| su              | switch user               | su bob                                |
| sudo            | run as root               | sudo ls                               |
| apt             | package manager           | sudo apt install x                    |
| yum             | package manager           | sudo yum install x                    |
| dnf             | package manager           | sudo dnf install x                    |
| pacman          | package manager           | sudo pacman -S x                      |
| snap            | package manager           | sudo snap install x                   |
| docker          | containers                | docker ps                             |
| docker-compose  | multi-container           | docker-compose up                     |
| git             | version control           | git clone repo                        |
| make            | build tool                | make                                  |
| gcc             | C compiler                | gcc a.c -o a                          |
+-----------------+---------------------------+--------------------------------------+
```

```code
chat Gpt
+--------------------------------------+----------------------------+-----------------------------+
| Utility                              | Command                    | Code                        |
+--------------------------------------+----------------------------+-----------------------------+
| Open file in editor                  | nano                       | nano ~/file                 |
| Open file in editor                  | vi                         | vi ~/file                   |
| Open file in editor                  | vim                        | vim ~/file                  |
| View file content                    | cat                        | cat file                    |
| View file content page-wise          | less                       | less file                   |
| View file content page-wise          | more                       | more file                   |
| Show first lines of file             | head                       | head file                   |
| Show last lines of file              | tail                       | tail file                   |
| Monitor command output               | watch                      | watch cmd                   |
| Search pattern in file               | grep                       | grep pattern file           |
| Text processing                      | awk                        | awk '{print $1}' file       |
| Text replace                         | sed                        | sed 's/a/b/' file           |
| Extract column                        | cut                        | cut -d: -f1 file            |
| Sort file                             | sort                       | sort file                   |
| Remove duplicates                     | uniq                       | uniq file                   |
| Count words/lines                     | wc                         | wc file                     |
| List directory                         | ls                         | ls                          |
| List detailed                         | ls -la                     | ls -la                      |
| Print working directory               | pwd                        | pwd                         |
| Change directory                      | cd                         | cd /path                    |
| Create directory                       | mkdir                      | mkdir dir                   |
| Create nested directories             | mkdir -p                   | mkdir -p a/b                |
| Remove empty directory                | rmdir                      | rmdir dir                   |
| Remove file                            | rm                         | rm file                     |
| Remove directory recursively          | rm -rf                     | rm -rf dir                  |
| Copy file                              | cp                         | cp a b                      |
| Copy directory                         | cp -r                       | cp -r a b                   |
| Move/rename file                       | mv                         | mv a b                      |
| Find file by name                      | find                       | find / -name file           |
| Locate file                            | locate                     | locate file                 |
| Show command path                       | which                      | which cmd                   |
| Show command location                   | whereis                    | whereis cmd                 |
| Change file permissions                 | chmod                      | chmod 755 file              |
| Change file owner                       | chown                      | chown user file             |
| Change file group                        | chgrp                      | chgrp group file            |
| Show file info                           | stat                       | stat file                   |
| Disk usage                               | df -h                       | df -h                        |
| Directory size                           | du -sh                      | du -sh dir                   |
| Show memory usage                        | free -h                     | free -h                       |
| Show processes                           | top                          | top                           |
| Show processes (interactive)             | htop                         | htop                          |
| Show all processes                        | ps aux                      | ps aux                        |
| Kill process                              | kill                         | kill pid                      |
| Kill process forcefully                   | kill -9                      | kill -9 pid                   |
| Show system uptime                         | uptime                       | uptime                        |
| Show current user                          | whoami                       | whoami                        |
| Show user ID                               | id                            | id                            |
| Show system info                           | uname                         | uname -a                      |
| Show hostname                              | hostname                      | hostname                       |
| Show hostname and info                     | hostnamectl                   | hostnamectl                   |
| Show IP addresses                           | ip a                          | ip a                           |
| Show routing table                          | ip r                          | ip r                           |
| Show network config                         | ifconfig                      | ifconfig                       |
| Test network                                | ping                          | ping host                      |
| Trace network route                          | traceroute                    | traceroute host                |
| Download file                               | curl                          | curl url                        |
| Download file                               | wget                          | wget url                        |
| SSH login                                   | ssh                           | ssh user@host                  |
| Copy file via SSH                           | scp                           | scp a user@host:b              |
| Sync files                                  | rsync                         | rsync -av a b                  |
| Compress directory                          | tar.gz                        | tar -czvf a.tar.gz dir         |
| Extract tar.gz                               | tar -xzvf                     | tar -xzvf a.tar.gz             |
| Compress file                               | zip                           | zip a.zip file                  |
| Extract zip                                  | unzip                         | unzip a.zip                     |
| Compress file                               | gzip                          | gzip file                       |
| Extract gzip                                 | gunzip                        | gunzip file.gz                  |
| Show service status                           | systemctl status             | systemctl status svc            |
| Start service                                 | systemctl start              | systemctl start svc             |
| Stop service                                  | systemctl stop               | systemctl stop svc              |
| Restart service                               | systemctl restart            | systemctl restart svc           |
| Show service logs                              | journalctl -u                | journalctl -u svc               |
| Show service status                            | service status               | service svc status              |
| Edit cron jobs                                 | crontab -e                   | crontab -e                      |
| List cron jobs                                 | crontab -l                   | crontab -l                      |
| Schedule job                                   | at                            | at now +1h                      |
| Show environment variables                     | env                           | env                             |
| Set environment variable                        | export                        | export VAR=1                    |
| Reload shell config                             | source                         | source ~/.bashrc                |
| Show command history                             | history                        | history                         |
| Clear terminal                                   | clear                          | clear                           |
| Reset terminal                                   | reset                          | reset                           |
| Create alias                                     | alias                          | alias ll='ls -la'               |
| Remove alias                                     | unalias                        | unalias ll                       |
| Show manual                                      | man                            | man cmd                          |
| Show info                                        | info                           | info cmd                         |
| Time command                                     | time                           | time cmd                         |
| Monitor command output                           | watch -n                        | watch -n1 cmd                    |
| Run command with xargs                            | xargs                           | xargs cmd                         |
| Output to file                                     | tee                             | tee file                          |
| Run command in background                           | nohup                           | nohup cmd &                       |
| Send job to background                               | bg                              | bg                                |
| Bring job to foreground                               | fg                              | fg                                |
| List jobs                                            | jobs                            | jobs                              |
| Remove job from shell                                  | disown                           | disown                             |
| Mount filesystem                                        | mount                            | mount                              |
| Unmount filesystem                                      | umount                           | umount                             |
| Show block devices                                        | lsblk                            | lsblk                              |
| Check mounted disks                                        | mount | grep disk                 | mount | grep disk                  |
| Shutdown system                                             | shutdown                         | shutdown now                       |
| Reboot system                                                | reboot                           | reboot                             |
+--------------------------------------+----------------------------+-----------------------------+
```

```code
Claude
+-----+------------------------------------------+--------------+------------------------------------------+
| #   | Utility                                  | Command Name | Code Example                             |
+-----+------------------------------------------+--------------+------------------------------------------+
| 1   | List directory contents                  | ls           | ls -la /home/user                        |
| 2   | Change directory                         | cd           | cd ~/Documents                           |
| 3   | Print working directory                  | pwd          | pwd                                      |
| 4   | Make directory                           | mkdir        | mkdir ~/new_folder                       |
| 5   | Remove empty directory                   | rmdir        | rmdir ~/old_folder                       |
| 6   | Remove files or directories              | rm           | rm ~/file.txt or rm -rf ~/folder         |
| 7   | Copy files or directories                | cp           | cp ~/source.txt ~/destination.txt        |
| 8   | Move or rename files                     | mv           | mv ~/old_name.txt ~/new_name.txt         |
| 9   | Create empty file or update timestamp    | touch        | touch ~/newfile.txt                      |
| 10  | Display file contents                    | cat          | cat ~/file.txt                           |
| 11  | View file contents page by page          | less         | less ~/large_file.log                    |
| 12  | View file contents                       | more         | more ~/file.txt                          |
| 13  | Display first lines of file              | head         | head -n 20 ~/file.txt                    |
| 14  | Display last lines of file               | tail         | tail -f ~/logfile.log                    |
| 15  | Search for files                         | find         | find ~/ -name "*.txt"                    |
| 16  | Find files by name (faster)              | locate       | locate docker-compose.yml                |
| 17  | Create links between files               | ln           | ln -s ~/original ~/link                  |
| 18  | Simple text editor                       | nano         | nano ~/docker-compose.yml                |
| 19  | Powerful text editor                     | vim          | vim ~/config.conf                        |
| 20  | Extensible text editor                   | emacs        | emacs ~/script.py                        |
| 21  | Compare files line by line               | diff         | diff ~/file1.txt ~/file2.txt             |
| 22  | Count words, lines, characters           | wc           | wc -l ~/file.txt                         |
| 23  | Change file permissions                  | chmod        | chmod 755 ~/script.sh                    |
| 24  | Change file owner                        | chown        | chown user:group ~/file.txt              |
| 25  | Change group ownership                   | chgrp        | chgrp developers ~/project               |
| 26  | Set default permissions mask             | umask        | umask 022                                |
| 27  | Search text patterns in files            | grep         | grep "error" ~/application.log           |
| 28  | Extended grep                            | egrep        | egrep "pattern1|pattern2" ~/file.txt     |
| 29  | Fixed-string grep                        | fgrep        | fgrep "exact string" ~/file.txt          |
| 30  | Pattern scanning and processing          | awk          | awk '{print $1}' ~/data.txt              |
| 31  | Stream editor for filtering text         | sed          | sed 's/old/new/g' ~/file.txt             |
| 32  | Display system information               | uname        | uname -a                                 |
| 33  | Show or set system hostname              | hostname     | hostname                                 |
| 34  | Show system uptime                       | uptime       | uptime                                   |
| 35  | Display current username                 | whoami       | whoami                                   |
| 36  | Display user and group IDs               | id           | id username                              |
| 37  | Display or set date and time             | date         | date +"%Y-%m-%d %H:%M:%S"                |
| 38  | Display calendar                         | cal          | cal 2026                                 |
| 39  | Report disk space usage                  | df           | df -h                                    |
| 40  | Estimate directory space usage           | du           | du -sh ~/Documents                       |
| 41  | Display memory usage                     | free         | free -h                                  |
| 42  | Display running processes                | ps           | ps aux                                   |
| 43  | Dynamic process viewer                   | top          | top                                      |
| 44  | Interactive process viewer               | htop         | htop                                     |
| 45  | Terminate processes by PID               | kill         | kill -9 1234                             |
| 46  | Kill processes by name                   | killall      | killall firefox                          |
| 47  | Signal processes by pattern              | pkill        | pkill -f python                          |
| 48  | Send job to background                   | bg           | bg %1                                    |
| 49  | Bring job to foreground                  | fg           | fg %1                                    |
| 50  | List background jobs                     | jobs         | jobs                                     |
| 51  | Run command immune to hangups            | nohup        | nohup ~/script.sh &                      |
| 52  | Test network connectivity                | ping         | ping google.com                          |
| 53  | Configure network interfaces (old)       | ifconfig     | ifconfig eth0                            |
| 54  | Show/manipulate routing and devices      | ip           | ip addr show                             |
| 55  | Network statistics                       | netstat      | netstat -tuln                            |
| 56  | Socket statistics                        | ss           | ss -tuln                                 |
| 57  | Trace route to host                      | traceroute   | traceroute google.com                    |
| 58  | Query DNS                                | nslookup     | nslookup google.com                      |
| 59  | DNS lookup utility                       | dig          | dig google.com                           |
| 60  | Download files from web                  | wget         | wget https://example.com/file.zip        |
| 61  | Transfer data from URLs                  | curl         | curl -O https://example.com/file.txt     |
| 62  | Secure shell remote login                | ssh          | ssh user@192.168.1.100                   |
| 63  | Secure copy files over network           | scp          | scp ~/file.txt user@host:~/              |
| 64  | Sync files and directories               | rsync        | rsync -avz ~/source/ user@host:~/dest/   |
| 65  | File transfer protocol client            | ftp          | ftp ftp.example.com                      |
| 66  | Archive files                            | tar          | tar -czf ~/archive.tar.gz ~/folder       |
| 67  | Compress files                           | gzip         | gzip ~/largefile.txt                     |
| 68  | Decompress gzip files                    | gunzip       | gunzip ~/file.txt.gz                     |
| 69  | Compress with bzip2                      | bzip2        | bzip2 ~/file.txt                         |
| 70  | Package and compress files               | zip          | zip -r ~/archive.zip ~/folder            |
| 71  | Extract zip files                        | unzip        | unzip ~/archive.zip                      |
| 72  | Execute command as superuser             | sudo         | sudo apt update                          |
| 73  | Switch user                              | su           | su - root                                |
| 74  | Add user account                         | useradd      | sudo useradd -m newuser                  |
| 75  | Delete user account                      | userdel      | sudo userdel -r username                 |
| 76  | Change user password                     | passwd       | passwd username                          |
| 77  | Debian/Ubuntu package manager            | apt-get      | sudo apt-get install package             |
| 78  | Modern apt interface                     | apt          | sudo apt install nginx                   |
| 79  | RedHat/CentOS package manager            | yum          | sudo yum install httpd                   |
| 80  | Modern Fedora package manager            | dnf          | sudo dnf install vim                     |
| 81  | Mount filesystem                         | mount        | sudo mount /dev/sdb1 /mnt                |
| 82  | Unmount filesystem                       | umount       | sudo umount /mnt                         |
| 83  | Partition table manipulator              | fdisk        | sudo fdisk /dev/sdb                      |
| 84  | Make filesystem                          | mkfs         | sudo mkfs.ext4 /dev/sdb1                 |
| 85  | Filesystem check and repair              | fsck         | sudo fsck /dev/sdb1                      |
| 86  | Sort lines of text                       | sort         | sort ~/names.txt                         |
| 87  | Remove duplicate lines                   | uniq         | uniq ~/sorted.txt                        |
| 88  | Cut columns from lines                   | cut          | cut -d',' -f1 ~/data.csv                 |
| 89  | Merge lines of files                     | paste        | paste ~/file1.txt ~/file2.txt            |
| 90  | Translate or delete characters           | tr           | tr '[:lower:]' '[:upper:]' < ~/file.txt  |
| 91  | Display text or variables                | echo         | echo "Hello World"                       |
| 92  | Formatted output                         | printf       | printf "Name: %s\n" "John"               |
| 93  | Show command history                     | history      | history | grep ssh                       |
| 94  | Display manual pages                     | man          | man ls                                   |
| 95  | Clear terminal screen                    | clear        | clear                                    |
| 96  | Change to previous directory             | cd -         | cd -                                     |
| 97  | Show file type                           | file         | file ~/unknown_file                      |
| 98  | Search command in PATH                   | which        | which python                             |
| 99  | Display all locations of command         | whereis      | whereis bash                             |
| 100 | Create command alias                     | alias        | alias ll='ls -la'                        |
+-----+------------------------------------------+--------------+------------------------------------------+
```
