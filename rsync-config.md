 

1. ssh-keygen -t rsa
 
이후 엔터 세번, passphrase를 empty 로 가져가기 위함. 패스워드 치지말것. 중요!!!!!!
아래와 같이 입력하면 /home/자신의 아이디/.ssh/id_rsa 와 id_rsa.pub가 생김.

Generating public/private rsa key pair.
Enter file in which to save the key (/home/bongkyo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:


2. ssh-copy-id -i ~/.ssh/id_rsa.pub bnd@dad주소

3. ssh -i ~/.ssh/id_rsa bnd@주소 으로 접속하여 패스워드 없이 접속되는 것 확인.

4. 아래를 실행하여 확인 (rsync.log)

rsync -avt --remove-source-files -e ssh -i /home/자기아이디/.ssh/id_rsa bnd@dad주소:하위경로 이미지저장경로 > /home/자기아이디/rsync.log 2>&1

<s>5. .bashrc 의 제일뒤에 다음의 라인들을 추가

exec ssh-agent $BASH -s 10<&0 << EOF.  
    ssh-add ~/.ssh/id_rsa &> /dev/null.   
    exec $BASH <&10-    
EOF.   
</s>                     

6. crontab -e

아래의 라인을 등록

\* * * * * rsync -rt --remove-source-files -e ssh -i /home/자기아이디/.ssh/id_rsa bnd@dad주소:하위경로 이미지저장경로 > /home/자기아이디/rsync.log 2>&1

[References]

https://www.thegeekstuff.com/2011/07/rsync-over-ssh-without-password/
https://unix.stackexchange.com/questions/90853/how-can-i-run-ssh-add-automatically-without-a-password-prompt
