1.��װGit
Windows
http://msysgit.github.io/
Linux
1.$ apt-get install git
2.$ yum install git-core
Mac
http://sourceforge.net/projects/git-osx-installer/files/

2.����Git

	
# �������������Ϣ
$ git config --list
# ������Ϣ����
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com" 
$ git config --global color.ui "always"


3.�ļ��汾����	
$ git init              # ��ʼ��������һ��.git�ļ�
# ���
$ git add somefile.txt  # ��ӵ����ļ������ذ汾��
$ git add *.txt         # ������е�txt�ļ������ذ汾��
$ git add .             # ������е���Ŀ¼����������Ŀ¼�������ذ汾��
# �ύ
$ git commit -m "msg" somefile.txt                # �ύ �����ļ�
$ git commit -m "msg" -a                          # �ύ �����޸��ļ�
$ git commit -C head -a -amend                    # �����ύ����������µ��ύ��ʷ
# ����δ�ύ���ļ�
$ git checkout head a.txt b.txt     # ���������ļ�
$ git checkout head *.txt           # ��������txt�ļ�
$ git checkout head .               # ���������ļ�
# �������ύ���ļ�
$ git revert --no-commit head <filename>      # �������һ�ε��ύ

4.�汾��֧	
$ git branch                # �г����ط�֧
$ git branch -a             # �г��������з�֧
 
$ git checkout <branchName>       # ǩ����֧
$ git branch <branchName>         # ���ڵ�ǰ��֧�����µķ�֧
$ git checkout -b <branchName>    # ���ڵ�ǰ��֧�����µķ�֧��ǩ��
 
$ git branch -m <branchName> <newName>  # ���Ḳ���Ѵ��ڵ�ͬ����֧
$ git branch -M <branchName> <newName>  # �Ḳ���Ѵ��ڵ�ͬ����֧
 
$ git branch -d <newName>                 # �����֧δ�ϲ���ɾ��ʧ��
$ git branch -D <newName>                 # ǿ��ɾ����֧
 
$ git branch -r -d origin/<branchName>       #ɾ��Զ�̷�֧1
$ git push origin :<branchName>              #ɾ��Զ�̷�֧2
 
$ git branch -r                     # �г�����Զ�̿��֧
$ git remote prune origin           # ɾ��Զ�̿ⲻ���ڵķ�֧
$ git merge �Cno�Cff <branchName>       # ���ٺϲ���֧

5.��ǩ	
$ git tag                               # ��ʾ���б�ǩ�б�
 
$ git tag <tagName>                     # ��ǰ���һ���ύ��ķ�֧�ϴ�����ǩ
$ git tag <tagName> <branchName>        # Ϊ�ض���֧���һ���ύ���״̬������ǩ
$ git tag <tagName> <version>           # Ϊ��ʷ�汾�ύ������ǩ
 
$ git checkout <tagName>                # ǩ����ǩ�����ٲ鿴����ĳ����ǩ�µĶ��棬�������ύ��
 
$ git tag -d <tagName>                  # ɾ����ǩ

6.Զ��Remote	
$ ssh-keygen -t rsa -C "youremail@example.com"    #cat ~/.ssh/id_rsa.pub �ϴ���Կ
 
$ git clone <URL>                                 #��¡�汾��
 
$ git remote add <origin> <URL>                   #���Զ�̰汾��origin
$ git remote rm <origin>                          #ɾ��Զ�̰汾��origin
 
$ git fetch <origin>                              #��ȡ�����ϲ�
$ git pull = git pull <origin>                    #��ȡ���ϲ������ط�֧
 
$ git push <origin> master                        #����Զ�̿�origin  ��һ�μ��� -u

7.����	
$ git status    #��ǰ״̬
$ git log       #��ʷ��־
/.gitignore     #���������ļ�����ӵ�����У�Ҳ֧�ְ汾����
#������������
$ git config --global alias.st status       #����git st = git status;   ��������ͬ��