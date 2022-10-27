#!bin/bash

IPADDR="$1/$2"
function changeIP(){
nmcli con mod Wired\ connection\ 1 ipv4.method manual
nmcli con mod Wired\ connection\ 1 ipv4.addresses $IPADDR 
nmcli con mod Wired\ connection\ 1 ipv4.gateway "$3"
nmcli con down Wired\ connection\ 1
nmcli con up Wired\ connection\ 1
}
regax='^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$'
if [[ $1 =~ $regax ]] && [ $2 -gt 1 ] && [ $2 -lt 32 ] && [[ $3 =~ $regax ]]; then

changeIP

echo -e "\n\n\n\n"
echo "------------------------------------------------------" 
echo "IP address changed successfully!"
echo "Please confirm the result of ifconfig below"
echo "------------------------------------------------------"
echo -e "\n\n\n\n"
ifconfig enp0s3

else
echo "-------------------------------------------------------"
echo "=====================Error============================="

if [[ ! $1 =~ $regax ]]; then
echo "The first argument is illigal		e.g.)xxx.xxx.xxx.xxx    c.f.)Your input:$1"
fi
if [ $2 -lt 1 ] || [ $2 -gt 32 ]; then
echo "The secound argument is illigal		e.g.)24			c.f.)Your Input:$2"
fi
if [[ ! $3 =~ $regax ]]; then
echo "The third argument is illigal		e.g.)xxx.xxx.xxx.xxx	c.f.)Your Input:$3"
fi
echo "-------------------------------------------------------"
fi
