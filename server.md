server-dev ec2-user@34.205.92.61
server-tst ec2-user@3.238.14.130
server-prd ec2-user@3.238.93.59

Rama
git rev-parse --abbrev-ref HEAD

commit
git rev-parse --short HEAD

gituser
git log -1 --pretty=format:'%an'

gitemail
git log -1 --pretty=format:'%ae'

giturl
git config --get remote.origin.url

version json

jq -r '.version' ./package.json

