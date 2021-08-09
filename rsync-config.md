 

1. ssh-keygen -t rsa
 
이후 엔터 세번, passphrase를 empty 로 가져가기 위함. 패스워드 치지말것. 중요!!!!!!
아래와 같이 입력하면 /home/자신의 아이디/.ssh/id_rsa 와 id_rsa.pub가 생김.

Generating public/private rsa key pair.
Enter file in which to save the key (/home/bongkyo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:


2. ssh-copy-id -i ~/.ssh/id_rsa.pub bnd@221.157.88.60

