```
1. Enable firewalld:
   systemctl enable --now firewalld

2. Remove public access to API:
   firewall-cmd --permanent --remove-port=6443/tcp

3. Allow specific IP:
   firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="YOUR.IP.ADD.RESS" port protocol="tcp" port="6443" accept'

4. Allow internal cluster subnet:
   firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.0.0.0/8" port protocol="tcp" port="6443" accept'

5. Reload rules:
   firewall-cmd --reload
``` 
